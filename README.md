# NextJS

# Section 1: Getting Started

---

## 1. Welcome To The Course!

**What is this:** An introduction to the course structure, goals, and what you will build by the end.

**Description:** This lesson sets the stage for the entire course. It covers the motivation behind learning NextJS, what projects you will build, and how the course is structured. No code is written here — it's a high-level orientation.

---

## 2. What Is NextJS? Why Would You Use It?

**What is this:** An explanation of what NextJS is and the problems it solves over plain React.

**Description:** NextJS is a React framework built on top of React that adds server-side rendering (SSR), static site generation (SSG), file-based routing, and full-stack capabilities out of the box. While React is a UI library, NextJS is an opinionated framework that handles the complete web application layer.

**Key reasons to use NextJS over plain React:**
- Built-in routing (no need for React Router)
- Server-side rendering and static generation for SEO and performance
- Full-stack support — API routes live in the same project
- Automatic code splitting and optimized builds
- File-system based routing convention

**Examples:**

```tsx
// Plain React: you manage routing yourself
import { BrowserRouter, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Route path="/about" component={About} />
    </BrowserRouter>
  );
}
```

```tsx
// NextJS: file structure IS the routing
// app/about/page.tsx → automatically becomes /about
export default function AboutPage() {
  return <h1>About</h1>;
}
```

---

## 3. Key Features & Benefits Of NextJS

**What is this:** A walkthrough of the core features that make NextJS powerful.

**Description:** This lesson covers the major pillars of NextJS and why each one matters for building modern web applications.

**Key features:**

| Feature | What it means |
|---|---|
| File-based routing | Pages are defined by folder/file structure, no router config needed |
| Server-Side Rendering (SSR) | Pages are rendered on the server per request — great for dynamic, SEO-sensitive content |
| Static Site Generation (SSG) | Pages are pre-rendered at build time — blazing fast, ideal for content that doesn't change often |
| Incremental Static Regeneration (ISR) | Rebuild individual pages in the background without a full redeploy |
| API Routes / Route Handlers | Write backend endpoints inside the same Next project |
| Server Components | Components that run only on the server — no JS sent to the client |
| Image & Font Optimization | Built-in `<Image>` and `<Font>` components with automatic optimization |

**Examples:**

```tsx
// SSR — data fetched fresh on every request (App Router)
// app/products/page.tsx
async function ProductsPage() {
  const res = await fetch('https://api.example.com/products', {
    cache: 'no-store', // opt out of caching → SSR behavior
  });
  const products = await res.json();

  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}

export default ProductsPage;
```

```tsx
// SSG — data fetched once at build time (App Router)
// app/blog/page.tsx
async function BlogPage() {
  const res = await fetch('https://api.example.com/posts', {
    cache: 'force-cache', // default — static generation
  });
  const posts = await res.json();

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default BlogPage;
```

```ts
// API Route / Route Handler — backend inside Next
// app/api/hello/route.ts
import { NextResponse } from 'next/server';

export function GET() {
  return NextResponse.json({ message: 'Hello from the server!' });
}
```

---

## 4. Creating a First NextJS App

**What is this:** How to scaffold a new NextJS project using the official CLI.

**Description:** NextJS provides a `create-next-app` CLI tool that sets up everything — TypeScript, ESLint, Tailwind (optional), the App Router, and folder structure — in one command.

**Examples:**

```bash
# Create a new NextJS app
npx create-next-app@latest my-app

# Prompts you'll see:
# ✔ Would you like to use TypeScript? Yes
# ✔ Would you like to use ESLint? Yes
# ✔ Would you like to use Tailwind CSS? Yes
# ✔ Would you like to use the `src/` directory? No
# ✔ Would you like to use App Router? Yes
# ✔ Would you like to customize the import alias? No

cd my-app
npm run dev
```

```
# Default folder structure (App Router)
my-app/
├── app/
│   ├── layout.tsx       ← root layout (wraps all pages)
│   ├── page.tsx         ← homepage → route: /
│   └── globals.css
├── public/              ← static assets
├── next.config.ts
├── package.json
└── tsconfig.json
```

---

## 5. NextJS vs "Just React" — Analyzing The NextJS Project

**What is this:** A comparison of the project structure and mental model differences between a plain React (Vite/CRA) app and a NextJS app.

**Description:** This lesson walks through the generated NextJS project and contrasts it with a typical React SPA. The key shift is that NextJS is not purely client-rendered — it introduces the concept of server components, layouts, and route segments as first-class building blocks.

**Examples:**

```
// Plain React (Vite) — everything is client-side
src/
├── main.tsx         ← mounts <App /> into the DOM
├── App.tsx          ← you define routes here manually
└── components/

// NextJS (App Router) — server-first by default
app/
├── layout.tsx       ← persistent shell (header, providers)
├── page.tsx         ← route segment for /
└── about/
    └── page.tsx     ← route segment for /about
```

```tsx
// React SPA: one HTML file, JS does everything
// index.html → <div id="root"> → React hydrates

// NextJS: server sends fully-rendered HTML
// browser receives actual content, not an empty shell
// → better SEO, faster first paint
```

---

## 6. Editing The First App

**What is this:** Making your first edits to the NextJS starter and seeing hot reload in action.

**Description:** This lesson covers the basics of editing [app/page.tsx](app/page.tsx) — the homepage — and observing how NextJS instantly reflects changes in the browser via Fast Refresh. It reinforces understanding of which file maps to which URL.

**Examples:**

```tsx
// app/page.tsx — the / route
export default function Home() {
  return (
    <main>
      <h1>Hello, NextJS!</h1>
      <p>Edit this file and save to see hot reload.</p>
    </main>
  );
}
```

```tsx
// app/layout.tsx — wraps every page
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'My App',
  description: 'Built with NextJS',
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

---

## 7. Pages Router vs App Router — One Framework, Two Approaches

**What is this:** An explanation of the two routing systems in NextJS and why the App Router is the modern standard.

**Description:** NextJS has two routing paradigms. The **Pages Router** (`pages/` directory) is the original approach — simpler, widely documented, still supported. The **App Router** (`app/` directory) was introduced in NextJS 13 and is now the recommended approach. It enables React Server Components, nested layouts, streaming, and co-located data fetching.

**Comparison:**

| | Pages Router | App Router |
|---|---|---|
| Directory | `pages/` | `app/` |
| Default rendering | Client components | Server components |
| Data fetching | `getServerSideProps`, `getStaticProps` | `async` components + `fetch()` |
| Layouts | Custom `_app.tsx` | `layout.tsx` per segment |
| Status | Stable, legacy | Recommended (Next 13+) |

**Examples:**

```tsx
// Pages Router — pages/about.tsx
// Data fetching is separate from the component
export async function getServerSideProps() {
  const data = await fetchData();
  return { props: { data } };
}

export default function AboutPage({ data }) {
  return <div>{data.title}</div>;
}
```

```tsx
// App Router — app/about/page.tsx
// Component itself is async — fetching is co-located
export default async function AboutPage() {
  const data = await fetchData();
  return <div>{data.title}</div>;
}
```

---

## 8. How To Get The Most Out Of This Course

**What is this:** Study strategies and tips for effectively following along with the course.

**Description:** This lesson provides guidance on how to engage with the material — when to pause, when to code along, how to handle errors, and how to use the provided resources. No code is covered here.

**Key tips:**
- Code along with every example rather than just watching
- Pause and experiment when something is unclear
- Use the Q&A section for blockers before moving on
- Check the attached resources for starter files and finished code snapshots

---

## 9. Learning Community & Course Resources

**What is this:** Links and pointers to the course community, supplementary resources, and support channels.

**Description:** This lesson points to the course Discord, GitHub repo with project code, and any external documentation links referenced throughout the course. No code is covered here.
