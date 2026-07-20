---
created: 2026-07-02
type: lesson
series: backend
number: 09
topic: hybrid-search-tag
status: seedling
---

# Hybrid Search & Tag Filtering — ค้นหาอัจฉริยะ + กรองตามหมวด

> **เปรียบ:** 
> - Hybrid Search = **เสิร์ช Google + ถามเพื่อน** พร้อมกัน (keyword + semantic) 
> - Tag Filtering = **ป้ายสีติดแฟ้ม** — หาง่าย, จัดกลุ่ม, กรองเร็ว

---

## Part 1: Hybrid Search

## 1.1 ปัญหาของ Semantic Search อย่างเดียว

| Query | Semantic Search (embedding) | Keyword Search (full-text) |
|-------|---------------------------|---------------------------|
| "เรียนล้ำหน้า" | ✅ หา "advanced topics" ได้ | ❌ เจอเฉพาะคำว่า "ล้ำหน้า" |
| "ผมรักคุณ" | ❌ หา "I love you" ไม่เจอ | ✅ ถ้ามีคำตรง |
| "method POST" | ❌ สับสน (POST ≠ mail) | ✅ เจอตรงๆ |
| "ความรู้สึกวันนี้" | ✅ หา mood entries ได้ | ❌ พลาดคำพ้อง |

**Hybrid Search รวมข้อดีของทั้งสอง:**

```
User Query: "วิธีการ login"


Semantic Path:                     Keyword Path:
สร้าง embedding                     parse คำสำคัญ
   ↓                                    ↓
หา entries ที่ความหมายคล้าย         หา entries ที่มีคำตรง
   ↓                                    ↓
score: 0.85                         score: 0.60
   ↓                                    ↓
         RRF (Reciprocal Rank Fusion)
                     ↓
         รวม rank จากทั้งสอง path
                     ↓
         ผลลัพธ์ที่แม่นยำขึ้น
```

## 1.2 Smooth vs Sharp Embedding

**Smooth Embedding (vector dimension ที่น้อย/เรียบ):**
- จับ concept กว้างๆ ได้ดี
- entries ที่ topic เดียวกัน → vector ใกล้กัน
- **เหมาะกับ:** semantic search ทั่วไป

**Sharp Embedding (vector dimension ที่มาก/แหลม):**
- จับ nuance และ specific detail
- คำคล้ายแต่คนละความหมาย → vector ไกลกัน
- **เหมาะกับ:** ต้องการ precision สูง

> **RAG-Diary:** OpenAI `text-embedding-3-small` = 1536 dimensions (default) — balance ระหว่าง smooth vs sharp

## 1.3 Reciprocal Rank Fusion (RRF)

RRF = วิธีรวม rank จากหลาย search method โดยไม่ต้อง normalize score

```
RRF Score = Σ (1 / (60 + rank(s)))

เมื่อ: rank(s) = อันดับของ entry จาก search method s
      60     = constant (ป้องกัน bias ตอน rank น้อย)

Example:
Entry A: semantic rank=1, keyword rank=10
RRF = 1/(60+1) + 1/(60+10) = 0.0164 + 0.0143 = 0.0307

Entry B: semantic rank=5, keyword rank=3
RRF = 1/(60+5) + 1/(60+3) = 0.0154 + 0.0159 = 0.0313 ✅ ชนะ
```

### Implementation

```typescript
async function hybridSearch(query: string, limit = 10) {
  // 1. Semantic search
  const embedding = await generateEmbedding(query);
  const semanticResults = await findSimilarEntries(embedding, limit * 2);

  // 2. Keyword search (using PostgreSQL full-text)
  const keywordResults = await supabase
    .from('diary_entries')
    .select('id, title, content')
    .textSearch('content', query, { type: 'websearch' })
    .limit(limit * 2);

  // 3. RRF merge
  const combined = new Map<number, EntryResult>();

  semanticResults.forEach((entry, i) => {
    combined.set(entry.id, {
      ...entry,
      rrfScore: 1 / (60 + i + 1),
      semanticRank: i + 1,
    });
  });

  keywordResults.forEach((entry, i) => {
    if (combined.has(entry.id)) {
      combined.get(entry.id)!.rrfScore += 1 / (60 + i + 1);
      combined.get(entry.id)!.keywordRank = i + 1;
    } else {
      combined.set(entry.id, {
        ...entry,
        rrfScore: 1 / (60 + i + 1),
        keywordRank: i + 1,
      });
    }
  });

  // 4. Sort by RRF score
  return [...combined.values()]
    .sort((a, b) => b.rrfScore - a.rrfScore)
    .slice(0, limit);
}
```

---

## Part 2: Tag Filtering

## 2.1 Tag Schema Design

### Many-to-Many: Diary Entry ↔ Tag

```sql
CREATE TABLE tags (
  id    SERIAL PRIMARY KEY,
  name  TEXT UNIQUE NOT NULL,      -- e.g. 'project', 'idea', 'journal'
  color TEXT DEFAULT '#6366f1'     -- for UI display
);

CREATE TABLE diary_entry_tags (
  entry_id  INTEGER REFERENCES diary_entries(id) ON DELETE CASCADE,
  tag_id    INTEGER REFERENCES tags(id) ON DELETE CASCADE,
  PRIMARY KEY (entry_id, tag_id)
);

-- Index for fast filtering
CREATE INDEX idx_entry_tags_tag ON diary_entry_tags(tag_id);
CREATE INDEX idx_entry_tags_entry ON diary_entry_tags(entry_id);
```

### Entity Relationship

```
diary_entries ──1:N── diary_entry_tags ──N:1── tags
     │                    │                      │
  id:int              entry_id:int            id:int
  title:text          tag_id:int              name:text
  content:text                               color:text
```

**ทำไมต้อง bridge table (`diary_entry_tags`)?**
- 1 entry ❌ มี 1 tag → use case จริงต้องหลาย tag
- 1 tag ✅ มีหลาย entry → many-to-many
- ถ้าใช้ `tags TEXT[]` ใน diary_entries → query ยาก, ไม่มี index, normalize ไม่ได้

## 2.2 CRUD Tags

```typescript
// Add tag to entry
async function addTag(entryId: number, tagName: string) {
  // Upsert tag (create if not exists)
  const { data: tag } = await supabase
    .from('tags')
    .upsert({ name: tagName }, { onConflict: 'name' })
    .select()
    .single();

  // Link tag to entry
  await supabase
    .from('diary_entry_tags')
    .upsert({ entry_id: entryId, tag_id: tag.id });
}

// Remove tag from entry
async function removeTag(entryId: number, tagId: number) {
  await supabase
    .from('diary_entry_tags')
    .delete()
    .match({ entry_id: entryId, tag_id: tagId });
}

// Get tags for an entry
async function getEntryTags(entryId: number) {
  const { data } = await supabase
    .from('diary_entry_tags')
    .select('tags(*)')
    .eq('entry_id', entryId);
  return data?.map((d: any) => d.tags) ?? [];
}
```

## 2.3 Filtering by Tags

```typescript
// Find entries with ALL specified tags (AND)
async function findByTags(tagNames: string[]) {
  const { data } = await supabase
    .from('diary_entries')
    .select(`
      *,
      diary_entry_tags!inner(
        tags!inner(name)
      )
    `)
    .in('diary_entry_tags.tags.name', tagNames);

  // Filter entries that have ALL tags
  return data?.filter(entry =>
    tagNames.every(tag =>
      entry.diary_entry_tags.some((det: any) => det.tags.name === tag)
    )
  );
}

// Find entries with ANY of specified tags (OR)
async function findByAnyTag(tagNames: string[]) {
  const { data } = await supabase
    .from('diary_entries')
    .select(`
      *,
      diary_entry_tags!inner(
        tags!inner(name)
      )
    `)
    .in('diary_entry_tags.tags.name', tagNames);

  return data; // Supabase handles OR via .in()
}
```

### Performance Note

- **AND filter** ใช้ `!inner` join → ได้ entries ที่มี tag match จากนั้น filter ฝั่ง client
- **OR filter** ใช้ `.in()` → Supabase query โดยตรง
- ถ้า entries มาก (>100K) → ควรสร้าง materialized view หรือใช้ array column + GIN index

## 2.4 Combined: Hybrid Search + Tag Filter

```typescript
async function searchWithFilters(
  query: string,
  tagNames?: string[],
  limit = 10
) {
  let results = await hybridSearch(query, limit * 2);

  if (tagNames && tagNames.length > 0) {
    const tagEntryIds = await getEntryIdsByTags(tagNames);
    results = results.filter(entry => tagEntryIds.has(entry.id));
  }

  return results.slice(0, limit);
}
```

---

## 3. Supabase Full-Text Search Setup

```sql
-- Create GIN index for full-text search
ALTER TABLE diary_entries
  ADD COLUMN search_vector tsvector
  GENERATED ALWAYS AS (
    to_tsvector('english', coalesce(title, '') || ' ' || coalesce(content, ''))
  ) STORED;

CREATE INDEX idx_diary_search ON diary_entries USING GIN(search_vector);

-- Search function
CREATE OR REPLACE FUNCTION search_entries(query_text TEXT)
RETURNS SETOF diary_entries AS $$
  SELECT *
  FROM diary_entries
  WHERE search_vector @@ plainto_tsquery('english', query_text)
  ORDER BY ts_rank(search_vector, plainto_tsquery('english', query_text)) DESC;
$$ LANGUAGE sql STABLE;
```

### RPC Call

```typescript
const { data } = await supabase.rpc('search_entries', {
  query_text: query,
});
```

---

## คำถามให้คิด

1. **Hybrid Search:** ถ้า semantic กับ keyword ให้ผลตรงข้าม — RRF จะเลือกให้ยังไง?
2. **Embedding Dimension:** 1536 dimensions เยอะไปไหม? ถ้าใช้ 256 dimensions จะต่างกันยังไง?
3. **Tag Design:** ทำไม tag ถึงควรเป็น `TEXT UNIQUE` แทนที่จะเป็น `SERIAL` แล้ว `name` ไม่ unique?
4. **Bridge Table:** query AND filter ต้อง filter ฝั่ง client — มีวิธีให้ database ทำทั้งหมดมั้ย?
5. **Full-Text Search:** `to_tsvector('english', ...)` ใช้ได้กับภาษาไทยมั้ย? ถ้าไม่ได้ต้องทำยังไง?

## Links

- Related: [[Backend/01 RAG Flow]], [[Backend/05 RAG Integration]]
- Code ref: [[server/src/lib/schema.sql]], [[server/src/modules/ai/rag.ts]]
- Source: Alex Xu — System Design Interview (NotebookLM)
- Q&A from: Socratic session 2026-07-02
