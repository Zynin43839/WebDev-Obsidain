---
created: 2026-07-02
type: lesson
series: frontend
number: 05
topic: tanstack-query
status: seedling
---

# TanStack Query (React Query v5) — Data Fetching ที่ไม่ต้องเขียนเอง

> **เปรียบ:** TanStack Query = **ตู้เย็นอัจฉริยะ**  
> แทนที่ไปตลาด (fetch) ทุกครั้งที่หิว → เก็บของในตู้เย็น (cache) → เช็คว่าของสดมั้ย (staleTime) → เติมของตอนใกล้หมด (refetch)

---

## 1. ปัญหาที่ TanStack Query แก้

```typescript
// ❌ แบบเก่า: useEffect + useState ต้อง handle เองทุกอย่าง
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  fetch('/api/diary')
    .then(res => res.json())
    .then(setData)
    .catch(setError)
    .finally(() => setLoading(false));
}, []);

// ✅ TanStack Query
const { data, isLoading, error } = useQuery({
  queryKey: ['entries'],
  queryFn: () => fetch('/api/diary').then(r => r.json()),
});
```

**TanStack Query ให้ของฟรี:**
- ✅ Caching — ไม่ fetch ซ้ำถ้าข้อมูลยังไม่เก่า
- ✅ Background refetch — โหลดข้อมูลล่าสุดเงียบๆ
- ✅ Loading/Error state — แยกอัตโนมัติ
- ✅ Retry — ลองใหม่ถ้า error
- ✅ Deduplication — requests ซ้ำกันรวมเป็นอันเดียว
- ✅ Pagination + Infinite scroll — built-in

---

## 2. Setup

```typescript
// src/lib/query-client.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5,   // 5 นาที (data ยัง fresh)
      gcTime: 1000 * 60 * 30,     // 30 นาที (ก่อน garbage collect)
      retry: 2,                    // retry 2 ครั้ง
      refetchOnWindowFocus: true,  // refetch เมื่อกลับมาที่ tab
    },
  },
});
```

```typescript
// src/app/providers.tsx
'use client';
import { QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

export function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(() => new QueryClient({
    defaultOptions: {
      queries: {
        staleTime: 60 * 1000,
        retry: 1,
      },
    },
  }));

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

---

## 3. useQuery — ดึงข้อมูล

### 3.1 Basic

```typescript
function DiaryList() {
  const { data, isLoading, isError, error } = useQuery({
    queryKey: ['diary', 'entries'],
    queryFn: async () => {
      const res = await fetch('/api/diary');
      if (!res.ok) throw new Error('Failed to fetch');
      return res.json();
    },
  });

  if (isLoading) return <Skeleton />;
  if (isError) return <Error message={error.message} />;

  return data.map(entry => <DiaryCard key={entry.id} entry={entry} />);
}
```

### 3.2 Query Keys

```typescript
// ❌ keys กระจาย
useQuery({ queryKey: ['entries'], ... });      // component A
useQuery({ queryKey: ['diary_entries'], ... }); // component B — key ต่างกัน, cache ไม่แชร์!

// ✅ Query Key Factory
export const diaryKeys = {
  all:      ['diary'] as const,
  lists:    () => [...diaryKeys.all, 'list'] as const,
  list:     (filters: DiaryFilters) => [...diaryKeys.lists(), filters] as const,
  details:  () => [...diaryKeys.all, 'detail'] as const,
  detail:   (id: number) => [...diaryKeys.details(), id] as const,
};

// ใช้
useQuery({
  queryKey: diaryKeys.list({ page: 1, tag: 'project' }),
  queryFn: () => fetchEntries({ page: 1, tag: 'project' }),
});

// Invalidate (ไม่ต้องจำ key)
queryClient.invalidateQueries({ queryKey: diaryKeys.all });
```

### 3.3 Dependent Queries

```typescript
function DiaryWithTags({ entryId }: { entryId: number }) {
  // Query นี้รอ entryId ก่อน
  const entry = useQuery({
    queryKey: diaryKeys.detail(entryId),
    queryFn: () => fetchEntry(entryId),
    enabled: !!entryId,  // ไม่รันถ้า entryId เป็น undefined
  });

  const tags = useQuery({
    queryKey: ['tags', entryId],
    queryFn: () => fetchTags(entryId),
    enabled: !!entry.data,  // รอให้ entry โหลดเสร็จก่อน
  });

  if (!entry.data) return null;
  return <div>{/* render */}</div>;
}
```

---

## 4. useMutation — สร้าง/แก้ไข/ลบ

```typescript
import { useMutation, useQueryClient } from '@tanstack/react-query';

function useCreateEntry() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: async (newEntry: EntryInput) => {
      const res = await fetch('/api/diary', {
        method: 'POST',
        body: JSON.stringify(newEntry),
      });
      if (!res.ok) throw new Error('Failed to create');
      return res.json();
    },
    onSuccess: () => {
      // หลังสร้างสำเร็จ → ทำให้ list cache เก่า → refetch อัตโนมัติ
      queryClient.invalidateQueries({ queryKey: diaryKeys.lists() });
    },
    onError: (err) => {
      toast.error(err.message);
    },
  });
}

// In component
function DiaryForm() {
  const createEntry = useCreateEntry();

  const handleSubmit = (data: EntryInput) => {
    createEntry.mutate(data, {
      onSuccess: () => navigate('/diary'),
    });
  };

  return (
    <form onSubmit={handleSubmit}>
      <Button disabled={createEntry.isPending}>
        {createEntry.isPending ? 'Creating...' : 'Save'}
      </Button>
    </form>
  );
}
```

### Mutation States

```
mutate() → isPending: true
   ↓
Success → isSuccess: true + invalidateQueries
   ↓
Error   → isError: true + onError rollback
```

---

## 5. Optimistic Updates

```typescript
function useToggleFavorite() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: ({ id, favorite }: { id: number; favorite: boolean }) =>
      fetch(`/api/diary/${id}`, {
        method: 'PATCH',
        body: JSON.stringify({ favorite }),
      }),

    // 1. อัปเดต UI ทันที (ก่อน server ตอบ)
    onMutate: async ({ id, favorite }) => {
      // ยกเลิก refetch ที่กำลังรอ (ป้องกัน overwrite)
      await queryClient.cancelQueries({ queryKey: diaryKeys.detail(id) });

      // เก็บค่าเก่าไว้ (rollback ถ้า error)
      const previous = queryClient.getQueryData(diaryKeys.detail(id));

      // อัปเดต cache ทันที
      queryClient.setQueryData(diaryKeys.detail(id), (old: any) => ({
        ...old,
        favorite,
      }));

      return { previous };
    },

    // 2. ถ้า error → คืนค่าเก่า
    onError: (err, { id }, context) => {
      queryClient.setQueryData(diaryKeys.detail(id), context?.previous);
      toast.error('Failed to update');
    },

    // 3. ไม่ว่ายังไง → refetch เพื่อ sync กับ server
    onSettled: (_, __, { id }) => {
      queryClient.invalidateQueries({ queryKey: diaryKeys.detail(id) });
    },
  });
}
```

---

## 6. Prefetching

```typescript
// Prefetch ตอน hover (ให้ user ไม่ต้องรอ)
function DiaryCard({ entry }: { entry: Entry }) {
  const queryClient = useQueryClient();

  return (
    <div
      onMouseEnter={() => {
        queryClient.prefetchQuery({
          queryKey: diaryKeys.detail(entry.id),
          queryFn: () => fetchEntry(entry.id),
          staleTime: 10 * 1000, // 10 วินาทีก่อน prefetch เก่า
        });
      }}
      onClick={() => navigate(`/diary/${entry.id}`)}
    >
      {entry.title}
    </div>
  );
}
```

---

## 7. DevTools

```bash
npm install -D @tanstack/react-query-devtools
```

```typescript
<QueryClientProvider client={queryClient}>
  {children}
  <ReactQueryDevtools initialIsOpen={false} />
</QueryClientProvider>
```

**DevTools แสดง:**
- ทุก query keys ที่ active
- status (fresh/stale/fetching)
- data ล่าสุด
- timing (last fetch, stale time)
- cache size

---

## คำถามให้คิด

1. **staleTime vs gcTime:** ต่างกันยังไง? ถ้า staleTime > gcTime จะเกิดอะไร?
2. **Query Key Factory:** ถ้าเปลี่ยน filter แต่ key structure เหมือนเดิม — cache จะ reuse หรือ refetch?
3. **Optimistic Update:** ถ้า server reject (favorite = false) แต่เรา set true ไปแล้ว → rollback กลับมา user จะงงมั้ย?
4. **Prefetching:** hover ทุก card → request เยอะเกินไปมั้ย? มี limit ไหม?
5. **Mutations:** `mutate` vs `mutateAsync` ต่างกันยังไง? ควรใช้อันไหนเมื่อไหร่?

## Links

- Related: [[Frontend/03 React Basics]], [[Frontend/04 Frontend-Backend Integration]]
- Code ref: [[frontend/src/app/providers.tsx]], [[frontend/src/components/diary/]]
- Source: Web search — TanStack Query v5 docs, Best practices 2026
- Q&A from: Socratic session 2026-07-02
