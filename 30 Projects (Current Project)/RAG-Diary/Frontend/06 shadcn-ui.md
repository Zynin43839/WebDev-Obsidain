---
created: 2026-07-02
type: lesson
series: frontend
number: 06
topic: shadcn-ui
status: seedling
---

# shadcn/ui — UI Components ที่คุณเป็นเจ้าของ

> **เปรียบ:** shadcn/ui = **IKEA ของวงการ UI**  
> ไม่ใช่ซื้อเฟอร์นิเจอร์สำเร็จรูป (npm package)  
> แต่ได้ **แบบแปลน + ไม้ + เครื่องมือ** มาประกอบเอง (copy source code)  
> คุณแก้ไข ปรับแต่ง ได้ตามใจ — ไม่มีใครมาเปลี่ยนของในบ้านคุณ

---

## 1. shadcn/ui คืออะไร (และไม่ใช่อะไร)

### shadcn/ui ≠ npm package
```bash
# ❌ ไม่ใช่แบบนี้
npm install shadcn-ui  # ไม่มี package นี้

# ✅ แต่เป็นแบบนี้
npx shadcn@latest add button
# copy ไฟล์ button.tsx มาไว้ใน components/ui/
```

### Philosophy

```
ธรรมดา (MUI, Chakra):        shadcn/ui:
npm install @mui/material    npx shadcn add button
     ↓                            ↓
node_modules/@mui/Button.js   components/ui/button.tsx
     ↓                            ↓
 ❌ แก้ยาก (ต้อง override)     ✅ แก้ได้เลย (owned)
 ❌ version lock               ✅ ไม่มี version
 ❌ bundle ทั้ง library         ✅ import เท่าที่ใช้
```

---

## 2. Installation (Next.js 16 + Tailwind v4)

### 2.1 Prerequisites

```bash
# Next.js 16 + TypeScript + Tailwind
npx create-next-app@latest my-app --typescript --tailwind --eslint --app --src-dir
cd my-app
```

### 2.2 Init shadcn/ui

```bash
npx shadcn@latest init
```

**คำถามระหว่าง init:**
```
Which style? → New York (recommended)
Base color?  → Neutral (default)
CSS variables? → Yes
```

**สิ่งที่生成:**
```
├── components.json          # config
├── lib/utils.ts             # cn() helper
├── app/globals.css          # CSS variables + theming
└── tailwind.config.ts       # updated paths
```

### 2.3 Add Components

```bash
# ทีละตัว
npx shadcn@latest add button
npx shadcn@latest add card input label

# หรือเพิ่มทีเดียว
npx shadcn@latest add button card dialog dropdown-menu
```

### 2.4 ใช้ใน Component

```typescript
// components/diary/diary-form.tsx
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

export function DiaryForm() {
  return (
    <Card>
      <CardHeader>
        <CardTitle>New Entry</CardTitle>
      </CardHeader>
      <CardContent>
        <Input placeholder="Title" />
        <Input placeholder="Content" />
        <Button>Save</Button>
      </CardContent>
    </Card>
  );
}
```

---

## 3. Theming with CSS Variables

### 3.1 Architecture

```
globals.css
├── :root           → light theme variables
└── .dark           → dark theme variables
       │
       ▼
Tailwind utilities
├── bg-background   → uses --background
├── text-foreground → uses --foreground
└── border-border   → uses --border
```

### 3.2 CSS Variables (globals.css)

```css
@import "tailwindcss";

@theme inline {
  --color-background: hsl(0 0% 100%);
  --color-foreground: hsl(0 0% 3.9%);
  --color-primary: hsl(0 0% 9%);
  --color-primary-foreground: hsl(0 0% 98%);
  --color-muted: hsl(0 0% 96.1%);
  --color-muted-foreground: hsl(0 0% 45.1%);
  --color-border: hsl(0 0% 89.8%);
  --radius: 0.5rem;
}

.dark {
  --color-background: hsl(0 0% 3.9%);
  --color-foreground: hsl(0 0% 98%);
  --color-primary: hsl(0 0% 98%);
  --color-primary-foreground: hsl(0 0% 9%);
  --color-muted: hsl(0 0% 14.9%);
  --color-muted-foreground: hsl(0 0% 63.9%);
  --color-border: hsl(0 0% 14.9%);
}
```

### 3.3 Semantic Token Convention

| Token | ใช้กับ |
|-------|-------|
| `background` | พื้นหลังหลัก |
| `foreground` | ตัวอักษรบนพื้นหลังหลัก |
| `primary` | ปุ่มหลัก, link, active state |
| `primary-foreground` | ตัวอักษรบน primary |
| `muted` | พื้นหลังรอง (card, sidebar) |
| `muted-foreground` | ตัวอักษร muted |
| `border` | เส้นขอบ |
| `ring` | focus ring |
| `destructive` | delete, error สีแดง |

---

## 4. Dark Mode

```bash
npm install next-themes
```

```typescript
// components/theme-provider.tsx
'use client';
import { ThemeProvider as NextThemesProvider } from 'next-themes';

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  return (
    <NextThemesProvider attribute="class" defaultTheme="system" enableSystem>
      {children}
    </NextThemesProvider>
  );
}
```

```typescript
// app/layout.tsx
import { ThemeProvider } from '@/components/theme-provider';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html suppressHydrationWarning>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

```typescript
// components/theme-toggle.tsx
'use client';
import { useTheme } from 'next-themes';
import { Button } from '@/components/ui/button';

export function ThemeToggle() {
  const { theme, setTheme } = useTheme();

  return (
    <Button
      variant="outline"
      onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
    >
      {theme === 'dark' ? '☀️' : '🌙'}
    </Button>
  );
}
```

> **หลักการ:** dark mode = เปลี่ยน CSS variables เท่านั้น component code ไม่เปลี่ยน

---

## 5. Components ที่พบบ่อย

### Button Variants

```typescript
<Button>Default</Button>
<Button variant="secondary">Secondary</Button>
<Button variant="destructive">Delete</Button>
<Button variant="outline">Outline</Button>
<Button variant="ghost">Ghost</Button>
<Button variant="link">Link</Button>
<Button size="sm">Small</Button>
<Button size="lg">Large</Button>
<Button asChild>
  <Link href="/diary">Link Button</Link>
</Button>
```

### Form + Validation (React Hook Form + Zod)

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Form, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form';

const diarySchema = z.object({
  title: z.string().min(1, 'Title is required'),
  content: z.string().min(1, 'Content is required'),
});

function DiaryForm() {
  const form = useForm({
    resolver: zodResolver(diarySchema),
  });

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)}>
        <FormField
          control={form.control}
          name="title"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Title</FormLabel>
              <Input {...field} />
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Save</Button>
      </form>
    </Form>
  );
}
```

---

## 6. Custom Components

เนื่องจาก shadcn/ui copy source code มาให้ — การ customize ทำได้ 2 ระดับ:

```typescript
// ระดับ 1: แก้ CSS variables (ทั้ง app)
// :root { --radius: 0.75rem; } → ทุก component radius ใหญ่ขึ้น

// ระดับ 2: แก้ source code component (เฉพาะจุด)
// components/ui/button.tsx
import { cva } from 'class-variance-authority';

const buttonVariants = cva(
  'inline-flex items-center justify-center ...',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground',
        destructive: 'bg-destructive text-destructive-foreground',
        outline: 'border border-input',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
        link: 'text-primary underline-offset-4 hover:underline',
        // ✅ เพิ่ม variant ของตัวเอง
        gradient: 'bg-gradient-to-r from-blue-500 to-purple-500 text-white',
      },
    },
  }
);
```

---

## คำถามให้คิด

1. **Code Generation:** การ copy source code เข้า project (แทน npm install) — ข้อดีข้อเสียคืออะไร?
2. **CSS Variables:** ทำไม shadcn/ui ถึงใช้ HSL แทน hex หรือ rgb?
3. **Dark Mode:** `next-themes` ใช้ `attribute="class"` — class `.dark` ถูก toggle ที่ element ไหน?
4. **Bundle Size:** ถ้าใช้ 5 components จาก shadcn/ui — bundle จะใหญ่กว่า MUI ที่มี tree-shaking มั้ย?
5. **Versioning:** ถ้า shadcn/ui อัปเดต Button component — เราจะได้อัปเดตอัตโนมัติมั้ย?

## Links

- Related: [[Frontend/03 React Basics]], [[Frontend/04 Frontend-Backend Integration]]
- Code ref: [[frontend/src/app/globals.css]], [[frontend/components.json]]
- Source: Web search — shadcn/ui docs, Tailwind v4 guide
- Q&A from: Socratic session 2026-07-02
