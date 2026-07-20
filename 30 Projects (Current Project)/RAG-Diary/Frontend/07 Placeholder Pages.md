---
created: 2026-07-02
type: lesson
series: frontend
number: 07
topic: placeholder-pages
status: seedling
---

# Placeholder Pages — หน้า UI ที่รอให้เต็ม

> **เปรียบ:** Placeholder Page = **แบบร่างตึก**  
> วางโครงสร้าง, เดินท่อ, ตั้งเสา — ยังไม่ต้องตกแต่ง  
> พร้อมให้ช่าง (developer) มาสร้างของจริงทีละชั้น

---

## 1. Placeholder คืออะไร?

**Placeholder Pages** = หน้าที่มีโครงสร้างครบ แต่เนื้อหายังไม่สมบูรณ์
- มี route (NavLink กดได้)
- มี layout (Header, Sidebar, Footer)
- มี state management พื้นฐาน
- มี UX แบบ skeleton / empty state
- **ยังไม่มี business logic จริง**

### ทำไมต้องมี?

```
MVP:       [Login] → [Dashboard] → [Diary CRUD]
                ↕
Placeholder: [Login] → [Setting] → [Favorites] → [Profile]
                                ↕
Full App:    ทุก route เชื่อมถึงกันหมด

ข้อดี:
- user / tester เข้าถึงทุกหน้าได้ตั้งแต่ต้น
- UI consistency (ทุกหน้ามี layout เดียวกัน)
- developer เห็น dependency ก่อนลงมือ
- client เห็น scope ทั้ง project
```

---

## 2. ตัวอย่าง: Favorites Page

### 2.1 Data Type (ADT — Algebraic Data Type)

```typescript
// types.ts
type FavoritesState<T> =
  | { status: 'loading' }
  | { status: 'empty' }
  | { status: 'loaded'; items: T[] }
  | { status: 'error'; message: string };

// ใช้
const [favorites, setFavorites] = useState<FavoritesState<Entry>>({
  status: 'loading',
});
```

### 2.2 Component

```typescript
// app/favorites/page.tsx
'use client';
import { useState, useEffect } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Skeleton } from '@/components/ui/skeleton';

type FavoritesState = 
  | { status: 'loading' }
  | { status: 'empty' }
  | { status: 'loaded'; items: Entry[] }
  | { status: 'error'; message: string };

export default function FavoritesPage() {
  const [state, setState] = useState<FavoritesState>({ status: 'loading' });

  useEffect(() => {
    fetchFavorites()
      .then(items => {
        setState(items.length === 0
          ? { status: 'empty' }
          : { status: 'loaded', items }
        );
      })
      .catch(err => setState({ status: 'error', message: err.message }));
  }, []);

  // Render by state
  switch (state.status) {
    case 'loading':
      return <LoadingSkeleton />;
    case 'empty':
      return <EmptyState />;
    case 'error':
      return <ErrorState message={state.message} />;
    case 'loaded':
      return <FavoritesList items={state.items} />;
  }
}

function LoadingSkeleton() {
  return (
    <div className="space-y-4">
      {[1, 2, 3].map(i => (
        <Card key={i}>
          <CardHeader>
            <Skeleton className="h-4 w-48" />
          </CardHeader>
          <CardContent>
            <Skeleton className="h-3 w-full" />
            <Skeleton className="h-3 w-3/4 mt-2" />
          </CardContent>
        </Card>
      ))}
    </div>
  );
}

function EmptyState() {
  return (
    <div className="text-center py-12">
      <p className="text-muted-foreground">No favorites yet</p>
      <p>Star entries to add them here</p>
    </div>
  );
}
```

---

## 3. ตัวอย่าง: Settings Page

### 3.1 State Management with useReducer

```typescript
// app/settings/page.tsx
type Settings = {
  theme: 'light' | 'dark' | 'system';
  language: 'th' | 'en';
  pageSize: number;
  aiProvider: 'openai' | 'gemini';
};

type Action =
  | { type: 'SET_THEME'; payload: Settings['theme'] }
  | { type: 'SET_LANGUAGE'; payload: Settings['language'] }
  | { type: 'SET_PAGE_SIZE'; payload: number }
  | { type: 'RESET' };

const defaultSettings: Settings = {
  theme: 'system',
  language: 'th',
  pageSize: 10,
  aiProvider: 'openai',
};

function settingsReducer(state: Settings, action: Action): Settings {
  switch (action.type) {
    case 'SET_THEME':    return { ...state, theme: action.payload };
    case 'SET_LANGUAGE': return { ...state, language: action.payload };
    case 'SET_PAGE_SIZE': return { ...state, pageSize: action.payload };
    case 'RESET':        return defaultSettings;
    default:             return state;
  }
}

export default function SettingsPage() {
  const [settings, dispatch] = useReducer(settingsReducer, defaultSettings);
  const [isDirty, setIsDirty] = useState(false);

  // Track changes
  useEffect(() => {
    setIsDirty(true);
  }, [settings]);

  const handleSave = async () => {
    await saveSettings(settings);
    setIsDirty(false);
    toast.success('Settings saved');
  };

  return (
    <Card>
      <CardHeader>
        <CardTitle>Settings</CardTitle>
      </CardHeader>
      <CardContent className="space-y-4">
        <Select
          label="Theme"
          value={settings.theme}
          onChange={v => dispatch({ type: 'SET_THEME', payload: v })}
          options={[
            { value: 'light', label: 'Light' },
            { value: 'dark', label: 'Dark' },
            { value: 'system', label: 'System' },
          ]}
        />
        {/* other fields */}
        <div className="flex gap-2">
          <Button onClick={handleSave} disabled={!isDirty}>
            Save
          </Button>
          <Button variant="outline" onClick={() => dispatch({ type: 'RESET' })}>
            Reset
          </Button>
        </div>
      </CardContent>
    </Card>
  );
}
```

---

## 4. ตัวอย่าง: Dashboard (Widget-based)

### 4.1 Widget System

```typescript
// lib/widgets.ts
interface Widget {
  id: string;
  title: string;
  component: React.ComponentType;
  gridArea?: string;
}

const defaultWidgets: Widget[] = [
  { id: 'recent', title: 'Recent Entries', component: RecentWidget },
  { id: 'stats', title: 'Stats', component: StatsWidget },
  { id: 'ai-summary', title: 'AI Summary', component: AISummaryWidget },
];
```

### 4.2 Dashboard Layout

```typescript
// app/dashboard/page.tsx
'use client';
import { useState } from 'react';
import { DragDropContext, Droppable, Draggable } from '@hello-pangea/dnd';

export default function DashboardPage() {
  const [widgets, setWidgets] = useState(defaultWidgets);

  const handleDragEnd = (result: any) => {
    if (!result.destination) return;
    const items = Array.from(widgets);
    const [reordered] = items.splice(result.source.index, 1);
    items.splice(result.destination.index, 0, reordered);
    setWidgets(items);
  };

  return (
    <div className="space-y-6">
      <h1>Dashboard</h1>
      <DragDropContext onDragEnd={handleDragEnd}>
        <Droppable droppableId="widgets">
          {(provided) => (
            <div ref={provided.innerRef} {...provided.droppableProps}
                 className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {widgets.map((widget, index) => (
                <Draggable key={widget.id} draggableId={widget.id} index={index}>
                  {(provided) => (
                    <Card ref={provided.innerRef} {...provided.draggableProps}>
                      <CardHeader>
                        <CardTitle {...provided.dragHandleProps}>
                          {widget.title}
                        </CardTitle>
                      </CardHeader>
                      <CardContent>
                        <widget.component />
                      </CardContent>
                    </Card>
                  )}
                </Draggable>
              ))}
              {provided.placeholder}
            </div>
          )}
        </Droppable>
      </DragDropContext>
      <Button variant="outline" onClick={() => setWidgets(prev => prev.slice(0, -1))}>
        Remove Last Widget
      </Button>
    </div>
  );
}
```

---

## 5. BFF Pattern (Backend for Frontend)

เมื่อมีหลายหน้า → ควรมี API endpoint ที่ออกแบบเฉพาะหน้า

```
❌ หน้า Dashboard ต้องเรียก:
GET /api/diary          → entries ล่าสุด
GET /api/diary/stats    → จำนวน entries
GET /api/ai/summary     → AI summary

✅ BFF endpoint:
GET /api/dashboard
Response:
{
  recentEntries: [...],
  stats: { total: 42, thisWeek: 5 },
  aiSummary: "You've been learning about refactoring..."
}
```

### BFF Server Route

```typescript
// server/src/modules/dashboard/dashboard.route.ts
app.get('/api/dashboard', async ({ user }) => {
  const [entries, stats, summary] = await Promise.all([
    getRecentEntries(user.id, 5),
    getEntryStats(user.id),
    getAISummary(user.id),
  ]);

  return { recentEntries: entries, stats, aiSummary: summary };
});
```

---

## 6. Placeholder Implementation Checklist

```
[ ] Route exists (Next.js App Router file)
[ ] Page component exported
[ ] Loading state (skeleton)
[ ] Empty state (no data message)
[ ] Error state (retry button)
[ ] Navigation link visible
[ ] Layout matches app theme
[ ] Backend route exists (even if returns mock)
[ ] API types defined
[ ] Test IDs for E2E
```

---

## คำถามให้คิด

1. **ADT State:** 4 states (loading/empty/loaded/error) — ครบทุกกรณีมั้ย? มีกรณีที่ต้องการ state เพิ่มไหม?
2. **useReducer vs useState:** ถ้า settings มี 10+ fields — useReducer ดีกว่า useState ยังไง?
3. **Widget Dashboard:** drag-and-drop widget — ตำแหน่งควรบันทึกไว้ที่ server หรือ local storage?
4. **BFF:** BFF เพิ่ม endpoint อีก layer — trade-off คืออะไร? คุ้มกับ performance improvement มั้ย?
5. **Placeholder First:** การสร้าง placeholder ก่อน implementation จริง — เสียเวลาเพิ่มตอนแรก แต่ได้อะไรตอบแทน?

## Links

- Related: [[Frontend/00 HTML]], [[Frontend/02 JavaScript DOM]], [[Frontend/04 Frontend-Backend Integration]]
- Code ref: [[frontend/app/]], [[frontend/src/components/diary/]]
- Source: Alex Xu — System Design Interview (NotebookLM)
- Q&A from: Socratic session 2026-07-02
