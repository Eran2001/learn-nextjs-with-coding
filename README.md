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

---

# Section 3: NextJS Essentials (App Router)

---

## 85. Module Introduction

**What is this:** An overview of what Section 3 covers — building real NextJS apps using the App Router.

**Description:** This module introduces the App Router paradigm, file-based routing, server vs client components, layouts, dynamic routes, server actions, data fetching, caching, and metadata. The section builds a full "Foodies" app from scratch to demonstrate these concepts in practice.

---

## 86. Starting Setup

**What is this:** Bootstrapping the project and reviewing the starter files provided for this section.

**Description:** This lesson walks through the provided starter project — its folder structure, pre-installed dependencies, and what still needs to be built. It establishes the baseline before any feature work begins.

---

## 87. Understanding File-based Routing & React Server Components

**What is this:** How the `app/` directory maps file paths to URL routes, and why components are server-side by default.

**Description:** In the App Router, every `page.js` file inside `app/` becomes a route. Components in the App Router are **React Server Components** by default — they render on the server, never ship their code to the client, and can directly `await` data without `useEffect`.

**Examples:**

```jsx
// app/page.js → route: /
export default function HomePage() {
  return <h1>Welcome to Foodies!</h1>;
}

// app/meals/page.js → route: /meals
export default function MealsPage() {
  return <h1>All Meals</h1>;
}

// app/meals/share/page.js → route: /meals/share
export default function ShareMealPage() {
  return <h1>Share a Meal</h1>;
}
```

```
app/
├── page.js            →  /
├── meals/
│   ├── page.js        →  /meals
│   └── share/
│       └── page.js    →  /meals/share
```

---

## 88. Adding Another Route via the File System

**What is this:** Creating a new route simply by adding a folder and `page.js` file.

**Description:** No router configuration is needed. Adding a new folder with a `page.js` inside `app/` is all it takes to register a new URL route in NextJS.

**Examples:**

```jsx
// Create app/community/page.js → route becomes /community
export default function CommunityPage() {
  return (
    <main>
      <h1>Our Community</h1>
      <p>Join thousands of food lovers.</p>
    </main>
  );
}
```

---

## 89. Navigating Between Pages — Wrong & Right Solution

**What is this:** Why `<a href>` breaks the SPA experience and how `<Link>` fixes it.

**Description:** Using a plain `<a>` tag causes a full page reload, losing the single-page app benefits. NextJS provides the `<Link>` component which performs client-side navigation — only the changed content re-renders, the page shell stays mounted.

**Examples:**

```jsx
// Wrong — full page reload on every click
export default function Nav() {
  return (
    <nav>
      <a href="/">Home</a>
      <a href="/meals">Meals</a>
    </nav>
  );
}
```

```jsx
// Right — client-side navigation, no reload
import Link from 'next/link';

export default function Nav() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/meals">Meals</Link>
    </nav>
  );
}
```

---

## 90. Working with Pages & Layouts

**What is this:** How `layout.js` wraps pages and persists UI across route changes.

**Description:** Every route segment can have a `layout.js`. The root `app/layout.js` is required and wraps the entire app. Layouts are not remounted on navigation — they stay alive while only the `{children}` (the active page) swaps out.

**Examples:**

```jsx
// app/layout.js — root layout, wraps every page
export const metadata = {
  title: 'Foodies',
  description: 'Delicious meals, shared by a community.',
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <header>
          <nav>/* site-wide nav here */</nav>
        </header>
        <main>{children}</main>
      </body>
    </html>
  );
}
```

```jsx
// app/meals/layout.js — nested layout, only wraps /meals and below
export default function MealsLayout({ children }) {
  return (
    <>
      <p>Browse and share amazing meals!</p>
      {children}
    </>
  );
}
```

---

## 91. Reserved File Names, Custom Components & How To Organize A NextJS Project

**What is this:** The special filenames NextJS recognizes and a recommended folder structure for custom components.

**Description:** NextJS reserves specific filenames inside the `app/` directory. Any other component you create should live outside `app/` (e.g., in `components/`) to keep things clean. Custom components are regular React components — nothing NextJS-specific about them.

**Examples:**

```
// Reserved filenames in app/ — NextJS handles these automatically
page.js         ← defines the UI for a route
layout.js       ← wraps a route segment and its children
loading.js      ← shown while a page is loading (Suspense boundary)
error.js        ← shown when an error is thrown in the segment
not-found.js    ← shown for 404s within the segment
route.js        ← API route handler (backend endpoint)
```

```
// Recommended project structure
app/
  page.js
  layout.js
  meals/
    page.js
components/       ← your custom UI components go here
  main-header/
    main-header.js
    main-header.module.css
lib/              ← utility functions, db helpers
assets/           ← images imported in JS
public/           ← static files served directly
```

---

## 92. Reserved Filenames

**What is this:** A reference summary of all special filenames the App Router recognizes.

**Description:** This is a quick-reference lesson listing all filenames that NextJS treats specially inside `app/`. Creating one of these files in a route segment automatically wires it into the framework's behavior for that segment.

**Reserved files:**

| File | Purpose |
|---|---|
| `page.js` | The route's UI — makes the segment publicly accessible |
| `layout.js` | Shared UI that wraps the segment and persists across navigations |
| `loading.js` | Automatic Suspense boundary — shown while the page streams in |
| `error.js` | Error boundary — shown when an error is thrown (must be a Client Component) |
| `not-found.js` | Rendered when `notFound()` is called or a 404 occurs |
| `route.js` | API endpoint — handles HTTP methods (GET, POST, etc.) |
| `template.js` | Like layout but remounts on every navigation |
| `default.js` | Fallback for parallel routes |

---

## 93. Configuring Dynamic Routes & Using Route Parameters

**What is this:** How to create routes with variable URL segments using bracket syntax.

**Description:** Dynamic routes are defined by wrapping a folder name in square brackets: `[slug]`. The segment value is available in the component via the `params` prop. This is how you build detail pages (e.g., `/meals/spaghetti`).

**Examples:**

```
app/
└── meals/
    ├── page.js            →  /meals
    └── [mealSlug]/
        └── page.js        →  /meals/:mealSlug
```

```jsx
// app/meals/[mealSlug]/page.js
export default function MealDetailPage({ params }) {
  const { mealSlug } = params;

  return (
    <main>
      <h1>Meal: {mealSlug}</h1>
    </main>
  );
}
// visiting /meals/spaghetti → params.mealSlug = 'spaghetti'
```

---

## 94. Onwards to the Main Project: The Foodies App

**What is this:** Transition lesson introducing the "Foodies" app that will be built throughout the rest of the section.

**Description:** The Foodies app is a meal-sharing community platform. Users can browse meals and submit their own with an image. It will demonstrate routing, layouts, server components, data fetching, server actions, image handling, SQLite, and caching. No new concepts here — just project context.

---

## 95. Exercise: Your Task

**What is this:** A hands-on exercise to build out the Foodies app routes and layout independently before seeing the solution.

**Description:** You are given a design and asked to set up the routing structure, root layout, and navigation for the Foodies app on your own. This reinforces the file-based routing and layout concepts from earlier lessons.

---

## 96. Exercise: Solution

**What is this:** Walkthrough of the instructor's solution to the exercise.

**Description:** This lesson reviews the expected folder structure, layout file, and navigation component for the Foodies app starter. Compare your solution and note any differences.

**Examples:**

```jsx
// app/layout.js
import MainHeader from '@/components/main-header/main-header';

export const metadata = {
  title: 'NextLevel Food',
  description: 'Delicious meals, shared by a food-loving community.',
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <MainHeader />
        {children}
      </body>
    </html>
  );
}
```

```jsx
// components/main-header/main-header.js
import Link from 'next/link';
import Image from 'next/image';
import logoImg from '@/assets/logo.png';

export default function MainHeader() {
  return (
    <header>
      <Link href="/">
        <Image src={logoImg} alt="A plate with food on it" priority />
        NextLevel Food
      </Link>
      <nav>
        <ul>
          <li><Link href="/meals">Browse Meals</Link></li>
          <li><Link href="/community">Foodies Community</Link></li>
        </ul>
      </nav>
    </header>
  );
}
```

---

## 97. Revisiting The Concept Of Layouts

**What is this:** A deeper look at how nested layouts compose and when to use them.

**Description:** This lesson revisits layouts with a more practical lens — showing how the root layout and a nested meals layout stack together. The key insight is that layouts receive `children` and render them inside their own JSX, forming a layout tree that matches the route hierarchy.

**Examples:**

```
Route: /meals/spaghetti

Layout tree rendered:
  RootLayout (app/layout.js)
    └── MealsLayout (app/meals/layout.js)
          └── MealDetailPage (app/meals/[mealSlug]/page.js)
```

```jsx
// app/meals/layout.js
export default function MealsLayout({ children }) {
  return (
    <>
      <p>Browse and share amazing meals from around the world.</p>
      {children}
    </>
  );
}
```

---

## 98. Adding a Custom Component To A Layout

**What is this:** Importing and rendering a custom component inside a layout file.

**Description:** Layouts are just components — you can import anything into them. This lesson adds a `<MainHeader>` component to the root layout to give the app persistent navigation.

**Examples:**

```jsx
// app/layout.js
import MainHeader from '@/components/main-header/main-header';

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <MainHeader />
        {children}
      </body>
    </html>
  );
}
```

---

## 99. Styling NextJS Project: Your Options & Using CSS Modules

**What is this:** The different ways to style a NextJS app and how CSS Modules work.

**Description:** NextJS supports global CSS, CSS Modules, Tailwind CSS, and CSS-in-JS. CSS Modules are the most common built-in approach — each `.module.css` file is scoped to the component that imports it, preventing class name collisions.

**Examples:**

```css
/* components/main-header/main-header.module.css */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem 1rem;
}

.nav a {
  color: #ddd6cb;
  text-decoration: none;
}

.nav a:hover {
  color: #f9572a;
}
```

```jsx
// components/main-header/main-header.js
import classes from './main-header.module.css';

export default function MainHeader() {
  return (
    <header className={classes.header}>
      <nav className={classes.nav}>
        {/* ... */}
      </nav>
    </header>
  );
}
// The class names are hashed at build time → no collisions
```

---

## 100. Optimizing Images with the NextJS Image Component

**What is this:** Using `<Image>` from `next/image` instead of `<img>` for automatic optimization.

**Description:** The NextJS `<Image>` component automatically handles lazy loading, resizing, format conversion (WebP), and preventing layout shift. For local images imported as modules, width and height are inferred automatically.

**Examples:**

```jsx
import Image from 'next/image';
import logoImg from '@/assets/logo.png'; // local import — size inferred

export default function MainHeader() {
  return (
    <header>
      <Image src={logoImg} alt="NextLevel Food logo" priority />
      {/* priority — disables lazy loading for above-the-fold images */}
    </header>
  );
}
```

```jsx
// Remote images — width and height required
<Image
  src="https://example.com/meal.jpg"
  alt="A delicious meal"
  width={400}
  height={300}
/>

// Also configure allowed remote domains in next.config.js:
// images: { remotePatterns: [{ hostname: 'example.com' }] }
```

---

## 101. Using More Custom Components

**What is this:** Breaking UI into smaller reusable components and composing them inside pages.

**Description:** This lesson adds more custom components (e.g., `<MealsGrid>`, `<MealItem>`) to the Foodies app. It reinforces the pattern of keeping components in `components/` and importing them into pages and layouts.

**Examples:**

```jsx
// components/meals/meals-grid.js
import MealItem from './meal-item';
import classes from './meals-grid.module.css';

export default function MealsGrid({ meals }) {
  return (
    <ul className={classes.meals}>
      {meals.map((meal) => (
        <MealItem key={meal.id} {...meal} />
      ))}
    </ul>
  );
}
```

```jsx
// components/meals/meal-item.js
import Link from 'next/link';
import Image from 'next/image';
import classes from './meal-item.module.css';

export default function MealItem({ title, slug, image, summary, creator }) {
  return (
    <article className={classes.meal}>
      <header>
        <div className={classes.image}>
          <Image src={image} alt={title} fill />
        </div>
        <div className={classes.headerText}>
          <h2>{title}</h2>
          <p>by {creator}</p>
        </div>
      </header>
      <div className={classes.content}>
        <p className={classes.summary}>{summary}</p>
        <div className={classes.actions}>
          <Link href={`/meals/${slug}`}>View Details</Link>
        </div>
      </div>
    </article>
  );
}
```

---

## 102. Populating The Starting Page Content

**What is this:** Adding real content and components to the homepage.

**Description:** This lesson fills out the Foodies homepage with actual JSX structure — a hero section, call-to-action links, and imports of shared components. It makes the starting page look complete before hooking up real data.

**Examples:**

```jsx
// app/page.js
import Link from 'next/link';
import classes from './page.module.css';

export default function HomePage() {
  return (
    <>
      <header className={classes.header}>
        <div className={classes.slideshow}>{/* slideshow here */}</div>
        <div>
          <div className={classes.hero}>
            <h1>NextLevel Food for NextLevel Foodies</h1>
            <p>Taste & share food from all over the world.</p>
          </div>
          <div className={classes.cta}>
            <Link href="/community">Join the Community</Link>
            <Link href="/meals">Explore Meals</Link>
          </div>
        </div>
      </header>
      <main>
        <section className={classes.section}>
          <h2>How it works</h2>
          <p>...</p>
        </section>
      </main>
    </>
  );
}
```

---

## 103. Preparing an Image Slideshow

**What is this:** Building an image slideshow component that cycles through images automatically.

**Description:** The slideshow rotates through a set of food images using `useState` and `useEffect` for the interval timer. Because it uses hooks, it **must be a Client Component** — this is the first time in the section where `'use client'` is needed.

**Examples:**

```jsx
// components/images/image-slideshow.js
'use client';

import { useState, useEffect } from 'react';
import Image from 'next/image';
import burgerImg from '@/assets/burger.jpg';
import curryImg from '@/assets/curry.jpg';
import dumplingsImg from '@/assets/dumplings.jpg';

const images = [burgerImg, curryImg, dumplingsImg];

export default function ImageSlideshow() {
  const [currentImageIndex, setCurrentImageIndex] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCurrentImageIndex((prev) => (prev < images.length - 1 ? prev + 1 : 0));
    }, 5000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      {images.map((image, index) => (
        <Image
          key={index}
          src={image}
          alt="Delicious food"
          className={index === currentImageIndex ? 'active' : ''}
        />
      ))}
    </div>
  );
}
```

---

## 104. React Server Components vs Client Components — When To Use What

**What is this:** The mental model for deciding whether a component should be a Server Component or a Client Component.

**Description:** Server Components run only on the server — no JS sent to the browser, can `await` data directly, cannot use hooks or browser APIs. Client Components run in the browser — can use `useState`, `useEffect`, event handlers, and browser APIs. The default in App Router is server. You opt into client with `'use client'` at the top of the file.

**Decision guide:**

| Need | Use |
|---|---|
| Fetch data from DB / API | Server Component |
| Access filesystem, env secrets | Server Component |
| `useState` / `useEffect` | Client Component |
| Event handlers (`onClick`, etc.) | Client Component |
| Browser APIs (`localStorage`, etc.) | Client Component |
| Pure display with no interactivity | Server Component (preferred) |

**Examples:**

```jsx
// Server Component — no 'use client', can await data
// app/meals/page.js
import { getMeals } from '@/lib/meals';

export default async function MealsPage() {
  const meals = await getMeals(); // direct DB call — fine on server
  return <MealsGrid meals={meals} />;
}
```

```jsx
// Client Component — needs interactivity
// components/nav-link.js
'use client';

import { usePathname } from 'next/navigation';
import Link from 'next/link';

export default function NavLink({ href, children }) {
  const path = usePathname();
  return (
    <Link href={href} className={path === href ? 'active' : ''}>
      {children}
    </Link>
  );
}
```

---

## 105. Using Client Components Efficiently

**What is this:** How to minimize the client bundle by keeping Client Components small and leaf-level.

**Description:** A common mistake is marking a large parent component as `'use client'` just because one child needs interactivity — this pushes the entire subtree to the client. The fix is to extract only the interactive part into its own small Client Component and keep the parent a Server Component.

**Examples:**

```jsx
// Bad — entire header becomes a client component just for active link styling
'use client';
import Link from 'next/link';
import Image from 'next/image';
import { usePathname } from 'next/navigation';
// ... all the static header markup is now client-side

export default function MainHeader() { ... }
```

```jsx
// Good — extract only the interactive piece
// components/main-header/nav-link.js
'use client';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
import classes from './nav-link.module.css';

export default function NavLink({ href, children }) {
  const path = usePathname();
  return (
    <Link href={href} className={path.startsWith(href) ? classes.active : ''}>
      {children}
    </Link>
  );
}

// components/main-header/main-header.js — stays a Server Component
import NavLink from './nav-link';

export default function MainHeader() {
  return (
    <header>
      <nav>
        <NavLink href="/meals">Browse Meals</NavLink>
        <NavLink href="/community">Foodies Community</NavLink>
      </nav>
    </header>
  );
}
```

---

## 106. Outputting Meals Data & Images With Unknown Dimensions

**What is this:** Handling `<Image>` when the image dimensions are not known ahead of time.

**Description:** When rendering user-submitted images or remote images without fixed dimensions, use the `fill` prop on `<Image>` combined with a positioned parent container. This lets the image fill its container without needing explicit `width` / `height`.

**Examples:**

```jsx
// components/meals/meal-item.js
import Image from 'next/image';
import classes from './meal-item.module.css';

export default function MealItem({ title, image, summary, creator, slug }) {
  return (
    <article className={classes.meal}>
      <header>
        <div className={classes.image}>
          {/* fill — image stretches to fill the parent div */}
          <Image src={image} alt={title} fill />
        </div>
        <h2>{title}</h2>
      </header>
    </article>
  );
}
```

```css
/* meal-item.module.css */
.image {
  position: relative; /* required for fill to work */
  height: 200px;
}
```

---

## 107. Setting Up A SQLite Database

**What is this:** Adding a local SQLite database to the NextJS project for persistent meal data.

**Description:** SQLite is a file-based database — no server needed. The `better-sqlite3` package provides a synchronous Node.js API to query it. The database file and seed script live in the project root and are only accessed from server-side code.

**Examples:**

```bash
npm install better-sqlite3
```

```js
// initdb.js — run once to create and seed the database
const sql = require('better-sqlite3');
const db = sql('meals.db');

db.exec(`
  CREATE TABLE IF NOT EXISTS meals (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    slug TEXT NOT NULL UNIQUE,
    title TEXT NOT NULL,
    image TEXT NOT NULL,
    summary TEXT NOT NULL,
    instructions TEXT NOT NULL,
    creator TEXT NOT NULL,
    creator_email TEXT NOT NULL
  )
`);
// insert seed data...
```

```bash
node initdb.js  # run once to initialize the database
```

---

## 108. Fetching Data By Leveraging NextJS & Fullstack Capabilities

**What is this:** Querying the SQLite database directly from a Server Component — no API layer needed.

**Description:** Because Server Components run on the server, they can import and call Node.js modules like `better-sqlite3` directly. There is no need for a separate REST API — the component fetches its own data.

**Examples:**

```js
// lib/meals.js
import sql from 'better-sqlite3';

const db = sql('meals.db');

export function getMeals() {
  return db.prepare('SELECT * FROM meals').all();
}
```

```jsx
// app/meals/page.js
import { getMeals } from '@/lib/meals';
import MealsGrid from '@/components/meals/meals-grid';

export default async function MealsPage() {
  const meals = await getMeals(); // Node.js call — only runs on server
  return (
    <main>
      <MealsGrid meals={meals} />
    </main>
  );
}
```

---

## 109. Adding A Loading Page

**What is this:** Using `loading.js` to show a fallback UI while a page is fetching data.

**Description:** Creating a `loading.js` file next to a `page.js` automatically wraps that page in a Suspense boundary. While the async Server Component resolves, NextJS renders the `loading.js` content instead of a blank screen.

**Examples:**

```jsx
// app/meals/loading.js
import classes from './loading.module.css';

export default function MealsLoadingPage() {
  return <p className={classes.loading}>Fetching meals...</p>;
}
```

```
app/meals/
├── page.js        ← async — fetches data
├── loading.js     ← shown automatically while page.js is pending
└── error.js
```

---

## 110. Using Suspense & Streamed Responses For Granular Loading State Management

**What is this:** Wrapping specific async parts of a page in `<Suspense>` for fine-grained loading control.

**Description:** `loading.js` applies to the whole page. For more control — showing a skeleton for just one section while the rest renders immediately — wrap the async component directly in `<Suspense>` with a `fallback`. This enables streaming: the server sends HTML in chunks as each part resolves.

**Examples:**

```jsx
// app/meals/page.js
import { Suspense } from 'react';
import MealsGrid from '@/components/meals/meals-grid';
import { getMeals } from '@/lib/meals';

async function Meals() {
  const meals = await getMeals();
  return <MealsGrid meals={meals} />;
}

export default function MealsPage() {
  return (
    <main>
      <h1>All Meals</h1>
      {/* only this part shows a loading state — the heading renders immediately */}
      <Suspense fallback={<p>Loading meals...</p>}>
        <Meals />
      </Suspense>
    </main>
  );
}
```

---

## 111. Handling Errors

**What is this:** Using `error.js` to gracefully catch and display errors thrown inside a route segment.

**Description:** `error.js` is an automatic error boundary for its route segment. It must be a Client Component (because React error boundaries require client-side lifecycle hooks). It receives `error` and `reset` props — `reset` lets the user retry without a full reload.

**Examples:**

```jsx
// app/meals/error.js
'use client';

export default function MealsError({ error, reset }) {
  return (
    <main className="error">
      <h1>An error occurred!</h1>
      <p>Failed to fetch meal data. Please try again later.</p>
      <p><i>{error.message}</i></p>
      <button onClick={reset}>Try Again</button>
    </main>
  );
}
```

---

## 112. Handling "Not Found" States

**What is this:** Using `not-found.js` and the `notFound()` helper to show a 404 page.

**Description:** Calling `notFound()` from anywhere in a Server Component immediately stops rendering and shows the nearest `not-found.js` file. The root `app/not-found.js` is a catch-all for any unmatched route.

**Examples:**

```jsx
// app/not-found.js — global 404 fallback
export default function NotFound() {
  return (
    <main className="not-found">
      <h1>Not Found</h1>
      <p>Unfortunately, we could not find the requested page or resource.</p>
    </main>
  );
}
```

```jsx
// app/meals/[mealSlug]/page.js
import { notFound } from 'next/navigation';
import { getMeal } from '@/lib/meals';

export default async function MealDetailPage({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) {
    notFound(); // stops rendering, shows not-found.js
  }

  return <div>{meal.title}</div>;
}
```

---

## 113. Loading & Rendering Meal Details via Dynamic Routes & Route Parameters

**What is this:** Fetching and displaying a single meal using its slug from the URL.

**Description:** The dynamic segment `[mealSlug]` is available via `params.mealSlug`. The page uses this to query the DB for a specific meal and renders its full detail view including image, creator, instructions, and summary.

**Examples:**

```js
// lib/meals.js
export function getMeal(slug) {
  return db.prepare('SELECT * FROM meals WHERE slug = ?').get(slug);
}
```

```jsx
// app/meals/[mealSlug]/page.js
import Image from 'next/image';
import { notFound } from 'next/navigation';
import { getMeal } from '@/lib/meals';
import classes from './page.module.css';

export default function MealDetailPage({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) notFound();

  // render instructions as HTML (instructor sets innerHTML — see lesson 121 for XSS note)
  meal.instructions = meal.instructions.replace(/\n/g, '<br />');

  return (
    <>
      <header className={classes.header}>
        <div className={classes.image}>
          <Image src={meal.image} alt={meal.title} fill />
        </div>
        <div className={classes.headerText}>
          <h1>{meal.title}</h1>
          <p className={classes.creator}>by <a href={`mailto:${meal.creator_email}`}>{meal.creator}</a></p>
          <p className={classes.summary}>{meal.summary}</p>
        </div>
      </header>
      <main>
        <p
          className={classes.instructions}
          dangerouslySetInnerHTML={{ __html: meal.instructions }}
        />
      </main>
    </>
  );
}
```

---

## 114. Throwing Not Found Errors For Individual Meals

**What is this:** Calling `notFound()` when a meal slug has no matching DB record.

**Description:** This is the same `notFound()` pattern from lesson 112 applied specifically to the meal detail page. If the DB returns nothing for the given slug, `notFound()` halts rendering and shows the nearest `not-found.js`.

**Examples:**

```jsx
// app/meals/[mealSlug]/page.js
import { notFound } from 'next/navigation';
import { getMeal } from '@/lib/meals';

export default function MealDetailPage({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) {
    notFound(); // shows app/meals/[mealSlug]/not-found.js or falls back to app/not-found.js
  }

  return <div>{meal.title}</div>;
}
```

---

## 115. Getting Started with the "Share Meal" Form

**What is this:** Building the initial form UI for submitting a new meal.

**Description:** This lesson creates the `/meals/share` page with a form that collects the meal's title, summary, instructions, creator name, email, and an image. At this stage the form is static — submission logic comes later.

**Examples:**

```jsx
// app/meals/share/page.js
import classes from './page.module.css';

export default function ShareMealPage() {
  return (
    <>
      <header className={classes.header}>
        <h1>Share your <span className={classes.highlight}>favorite meal</span></h1>
        <p>Or any other meal you feel needs sharing!</p>
      </header>
      <main className={classes.main}>
        <form className={classes.form}>
          <div className={classes.row}>
            <p>
              <label htmlFor="name">Your name</label>
              <input type="text" id="name" name="name" required />
            </p>
            <p>
              <label htmlFor="email">Your email</label>
              <input type="email" id="email" name="email" required />
            </p>
          </div>
          <p>
            <label htmlFor="title">Title</label>
            <input type="text" id="title" name="title" required />
          </p>
          <p>
            <label htmlFor="summary">Short Summary</label>
            <input type="text" id="summary" name="summary" required />
          </p>
          <p>
            <label htmlFor="instructions">Instructions</label>
            <textarea id="instructions" name="instructions" rows="10" required />
          </p>
          {/* image picker goes here */}
          <p className={classes.actions}>
            <button type="submit">Share Meal</button>
          </p>
        </form>
      </main>
    </>
  );
}
```

---

## 116. Getting Started with a Custom Image Picker Input Component

**What is this:** Building a custom file input that replaces the native browser file picker with a styled button.

**Description:** The native `<input type="file">` is hard to style. This lesson creates a `<ImagePicker>` Client Component that hides the real input and triggers it programmatically via a `ref` when a custom button is clicked.

**Examples:**

```jsx
// components/meals/image-picker.js
'use client';

import { useRef } from 'react';
import classes from './image-picker.module.css';

export default function ImagePicker({ label, name }) {
  const imageInputRef = useRef();

  function handlePickClick() {
    imageInputRef.current.click(); // programmatically trigger hidden input
  }

  return (
    <div className={classes.picker}>
      <label htmlFor={name}>{label}</label>
      <div className={classes.controls}>
        <input
          className={classes.input}
          type="file"
          id={name}
          name={name}
          accept="image/png, image/jpeg"
          ref={imageInputRef}
        />
        <button type="button" onClick={handlePickClick}>
          Pick an Image
        </button>
      </div>
    </div>
  );
}
```

---

## 117. Adding an Image Preview to the Picker

**What is this:** Showing a preview of the selected image before the form is submitted.

**Description:** When the user selects an image, `FileReader` converts it to a data URL and stores it in state. The preview renders using a regular `<img>` tag (not `<Image>`) since the data URL is local and doesn't go through Next's image optimization pipeline.

**Examples:**

```jsx
// components/meals/image-picker.js
'use client';

import { useRef, useState } from 'react';

export default function ImagePicker({ label, name }) {
  const [pickedImage, setPickedImage] = useState(null);
  const imageInputRef = useRef();

  function handleImageChange(event) {
    const file = event.target.files[0];
    if (!file) {
      setPickedImage(null);
      return;
    }
    const fileReader = new FileReader();
    fileReader.onload = () => setPickedImage(fileReader.result);
    fileReader.readAsDataURL(file);
  }

  return (
    <div>
      <div>
        {pickedImage ? (
          // eslint-disable-next-line @next/next/no-img-element
          <img src={pickedImage} alt="Preview of your pick" />
        ) : (
          <p>No image picked yet.</p>
        )}
      </div>
      <input
        type="file"
        name={name}
        ref={imageInputRef}
        onChange={handleImageChange}
        accept="image/png, image/jpeg"
      />
      <button type="button" onClick={() => imageInputRef.current.click()}>
        Pick an Image
      </button>
    </div>
  );
}
```

---

## 118. Improving the Image Picker Component

**What is this:** Polishing the ImagePicker — hiding the native input and cleaning up the UI.

**Description:** This lesson refines the component from lesson 117: the native `<input>` is visually hidden via CSS, the preview image fills its container, and the layout is tidied up with CSS Modules. No new concepts — just UX improvements.

---

## 119. Introducing & Using Server Actions for Handling Form Submissions

**What is this:** Using Server Actions — async functions that run on the server — to handle form submissions without an API route.

**Description:** Server Actions are functions marked with `'use server'` that can be passed directly as a form's `action` prop. When the form submits, NextJS calls the function on the server with the `FormData`. No `fetch()`, no API route, no event handler needed.

**Examples:**

```jsx
// app/meals/share/page.js
import { redirect } from 'next/navigation';

async function shareMeal(formData) {
  'use server'; // marks this as a Server Action

  const meal = {
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
    creator: formData.get('name'),
    creator_email: formData.get('email'),
  };

  // save to DB (next lesson)
  console.log(meal);
  redirect('/meals');
}

export default function ShareMealPage() {
  return (
    <form action={shareMeal}>
      {/* form fields */}
      <button type="submit">Share Meal</button>
    </form>
  );
}
```

---

## 120. Storing Server Actions in Separate Files

**What is this:** Moving Server Actions out of page components and into a dedicated `lib/actions.js` file.

**Description:** Inlining Server Actions in page files works but doesn't scale. The convention is to put them in a separate file marked with `'use server'` at the top — this makes the entire file a "server actions module" where every export is treated as a Server Action.

**Examples:**

```js
// lib/actions.js
'use server'; // every export in this file is a Server Action

import { redirect } from 'next/navigation';
import { saveMeal } from './meals';

export async function shareMeal(formData) {
  const meal = {
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
    creator: formData.get('name'),
    creator_email: formData.get('email'),
  };

  await saveMeal(meal);
  redirect('/meals');
}
```

```jsx
// app/meals/share/page.js
import { shareMeal } from '@/lib/actions';

export default function ShareMealPage() {
  return (
    <form action={shareMeal}>
      {/* form fields */}
    </form>
  );
}
```

---

## 121. Creating a Slug & Sanitizing User Input for XSS Protection

**What is this:** Auto-generating a URL slug from the meal title and sanitizing HTML in instructions to prevent XSS.

**Description:** The slug is derived from the title using `slugify`. User-submitted instructions may contain characters that get rendered as HTML (via `dangerouslySetInnerHTML`), so they must be sanitized with `xss` before saving to the DB.

**Examples:**

```bash
npm install slugify xss
```

```js
// lib/meals.js
import slugify from 'slugify';
import xss from 'xss';
import sql from 'better-sqlite3';

const db = sql('meals.db');

export async function saveMeal(meal) {
  meal.slug = slugify(meal.title, { lower: true });
  meal.instructions = xss(meal.instructions); // sanitize before storing

  // handle image saving (next lesson)...

  db.prepare(`
    INSERT INTO meals
      (title, summary, instructions, creator, creator_email, image, slug)
    VALUES
      (@title, @summary, @instructions, @creator, @creator_email, @image, @slug)
  `).run(meal);
}
```

---

## 122. Storing Uploaded Images & Storing Data in the Database

**What is this:** Saving the uploaded image file to disk and inserting the complete meal record into SQLite.

**Description:** The image arrives as a `File` object from `FormData`. It's converted to a `Buffer` and written to the `public/` folder so NextJS can serve it as a static asset. The file path is then stored as a string in the DB.

**Examples:**

```js
// lib/meals.js
import { writeFile } from 'fs/promises';
import path from 'path';

export async function saveMeal(meal) {
  meal.slug = slugify(meal.title, { lower: true });
  meal.instructions = xss(meal.instructions);

  const image = meal.image; // File object from FormData
  const extension = image.name.split('.').pop();
  const fileName = `${meal.slug}.${extension}`;

  const bufferedImage = await image.arrayBuffer();
  await writeFile(
    path.join(process.cwd(), 'public', 'images', fileName),
    Buffer.from(bufferedImage)
  );

  meal.image = `/images/${fileName}`; // store relative public path

  db.prepare(`
    INSERT INTO meals (title, summary, instructions, creator, creator_email, image, slug)
    VALUES (@title, @summary, @instructions, @creator, @creator_email, @image, @slug)
  `).run(meal);
}
```

---

## 123. Managing the Form Submission Status with useFormStatus

**What is this:** Using the `useFormStatus` hook to disable the submit button while a Server Action is in progress.

**Description:** `useFormStatus` is a React hook that reads the pending state of the nearest parent `<form>`. It must be used in a **Client Component** that is a child of the form — not in the form's own component. The `pending` value is `true` while the Server Action is executing.

**Examples:**

```jsx
// components/meals/meal-form-submit.js
'use client';

import { useFormStatus } from 'react-dom';

export default function MealFormSubmit() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Submitting...' : 'Share Meal'}
    </button>
  );
}
```

```jsx
// app/meals/share/page.js
import MealFormSubmit from '@/components/meals/meal-form-submit';

export default function ShareMealPage() {
  return (
    <form action={shareMeal}>
      {/* fields */}
      <MealFormSubmit />
    </form>
  );
}
```

---

## 124. Adding Server-Side Input Validation

**What is this:** Validating form data inside a Server Action before saving to the DB.

**Description:** Never trust client-side validation alone. Inside the Server Action, check that required fields are non-empty before proceeding. If validation fails, return an error object instead of redirecting — the component can then display the error.

**Examples:**

```js
// lib/actions.js
'use server';

function isInvalidText(text) {
  return !text || text.trim() === '';
}

export async function shareMeal(formData) {
  const meal = {
    creator: formData.get('name'),
    creator_email: formData.get('email'),
    title: formData.get('title'),
    summary: formData.get('summary'),
    instructions: formData.get('instructions'),
    image: formData.get('image'),
  };

  if (
    isInvalidText(meal.title) ||
    isInvalidText(meal.summary) ||
    isInvalidText(meal.instructions) ||
    isInvalidText(meal.creator) ||
    !meal.creator_email.includes('@') ||
    !meal.image || meal.image.size === 0
  ) {
    return { message: 'Invalid input.' }; // return error, do not redirect
  }

  await saveMeal(meal);
  redirect('/meals');
}
```

---

## 125. Using useFormState()

**What is this:** Using `useFormState` to receive and display the value returned by a Server Action.

**Description:** `useFormState` (from `react-dom`) connects a form to a Server Action and gives the component access to whatever the action returns. It replaces the standard `action` prop with an enhanced version. The component that calls it must be a Client Component.

**Examples:**

```jsx
// components/meals/share-form.js
'use client';

import { useFormState } from 'react-dom';
import { shareMeal } from '@/lib/actions';
import MealFormSubmit from './meal-form-submit';

export default function ShareForm() {
  const [state, formAction] = useFormState(shareMeal, { message: null });

  return (
    <form action={formAction}>
      {/* fields */}
      {state.message && <p>{state.message}</p>}
      <MealFormSubmit />
    </form>
  );
}
```

```js
// lib/actions.js — action now receives prevState as first arg
export async function shareMeal(prevState, formData) {
  // ...validation...
  if (invalid) return { message: 'Invalid input.' };
  await saveMeal(meal);
  redirect('/meals');
}
```

---

## 126. Working with Server Action Responses & useFormState

**What is this:** Wiring up the full flow — Server Action returns an error, `useFormState` surfaces it in the UI.

**Description:** This lesson completes the form submission flow. The Server Action validates input and returns `{ message }` on failure. `useFormState` receives this and the component renders the error message below the form. On success, `redirect()` is called and the user is sent to `/meals`.

**Examples:**

```jsx
// Full working form with error display
'use client';

import { useFormState } from 'react-dom';
import { shareMeal } from '@/lib/actions';

export default function ShareForm() {
  const [state, formAction] = useFormState(shareMeal, { message: null });

  return (
    <form action={formAction}>
      <input type="text" name="name" required />
      <input type="email" name="email" required />
      <input type="text" name="title" required />
      {/* ...other fields... */}
      {state.message && (
        <p style={{ color: 'red' }}>{state.message}</p>
      )}
      <button type="submit">Share Meal</button>
    </form>
  );
}
```

---

## 127. Building For Production & Understanding NextJS Caching

**What is this:** How NextJS aggressively caches pages at build time and what that means for dynamic data.

**Description:** Running `npm run build` pre-renders all static pages. Pages that fetch data are also cached — meaning the data is frozen at build time by default. In production, visiting `/meals` returns the cached HTML, not fresh DB data. This is NextJS's **Full Route Cache**.

**Examples:**

```bash
npm run build   # pre-renders and caches all routes
npm start       # serves the production build
```

```
# Build output tells you how each route is rendered:
○  /              → static (no data fetching)
●  /meals         → SSG (data fetched at build time, cached)
ƒ  /meals/[slug]  → dynamic (per-request, not cached)
```

---

## 128. Triggering Cache Revalidations

**What is this:** Using `revalidatePath()` to invalidate the cache after a mutation so fresh data is served.

**Description:** After a new meal is saved via a Server Action, the `/meals` cache is stale. Calling `revalidatePath('/meals')` tells NextJS to regenerate that route on the next request. Pass `'layout'` as the second argument to also revalidate all nested routes under that path.

**Examples:**

```js
// lib/actions.js
'use server';

import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';
import { saveMeal } from './meals';

export async function shareMeal(prevState, formData) {
  // ...validation and save...
  await saveMeal(meal);

  revalidatePath('/meals');        // revalidate just /meals
  // revalidatePath('/meals', 'layout'); // revalidate /meals and all sub-routes

  redirect('/meals');
}
```

---

## 129. Don't Store Files Locally On The Filesystem!

**What is this:** Why saving uploaded images to the local filesystem breaks in production on serverless platforms.

**Description:** Writing to `public/images/` works in development, but serverless deployments (Vercel, AWS Lambda) have ephemeral filesystems — uploaded files disappear between function invocations. The solution is to store files in cloud object storage (e.g., AWS S3) instead.

**Key points:**
- Serverless functions are stateless — local writes do not persist
- Any file saved to disk during a request may be gone by the next request
- Always use cloud storage (S3, Cloudinary, Supabase Storage) for user uploads in production

---

## 130. Bonus: Storing Uploaded Images In The Cloud (AWS S3)

**What is this:** Replacing local file writes with uploads to an AWS S3 bucket.

**Description:** Instead of writing the image buffer to disk, the `saveMeal` function uploads it to S3 using the AWS SDK. The S3 object URL is then stored in the DB. The bucket must be configured with the appropriate public read permissions.

**Examples:**

```bash
npm install @aws-sdk/client-s3
```

```js
// lib/meals.js
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';

const s3 = new S3Client({ region: 'us-east-1' });

export async function saveMeal(meal) {
  meal.slug = slugify(meal.title, { lower: true });
  meal.instructions = xss(meal.instructions);

  const image = meal.image;
  const extension = image.name.split('.').pop();
  const fileName = `${meal.slug}.${extension}`;

  const bufferedImage = await image.arrayBuffer();

  await s3.send(new PutObjectCommand({
    Bucket: 'your-s3-bucket-name',
    Key: fileName,
    Body: Buffer.from(bufferedImage),
    ContentType: image.type,
  }));

  meal.image = `https://your-s3-bucket-name.s3.amazonaws.com/${fileName}`;

  db.prepare(`INSERT INTO meals (...) VALUES (...)`).run(meal);
}
```

---

## 131. Adding Static Metadata

**What is this:** Setting page-level SEO metadata using the `metadata` export.

**Description:** Export a `metadata` object from any `page.js` or `layout.js` to set the `<title>`, `<meta description>`, and other head tags for that route. This is static — the values are fixed strings known at build time.

**Examples:**

```js
// app/meals/page.js
export const metadata = {
  title: 'All Meals',
  description: 'Browse the delicious meals shared by our vibrant community.',
};

export default async function MealsPage() {
  // ...
}
```

```js
// app/layout.js — root metadata (used as fallback)
export const metadata = {
  title: {
    template: '%s | NextLevel Food', // %s is replaced by child page titles
    default: 'NextLevel Food',
  },
  description: 'Delicious meals, shared by a food-loving community.',
};
```

---

## 132. Adding Dynamic Metadata

**What is this:** Generating metadata at request time using `generateMetadata` for dynamic routes.

**Description:** For dynamic routes like `/meals/[mealSlug]`, the metadata depends on the actual data — you need the meal's title to set the `<title>` tag. Export an async `generateMetadata` function that receives `params` and returns a metadata object.

**Examples:**

```js
// app/meals/[mealSlug]/page.js
import { getMeal } from '@/lib/meals';
import { notFound } from 'next/navigation';

export async function generateMetadata({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) notFound();

  return {
    title: meal.title,
    description: meal.summary,
  };
}

export default function MealDetailPage({ params }) {
  const meal = getMeal(params.mealSlug);
  if (!meal) notFound();
  return <div>{meal.title}</div>;
}
```

---

## 133. Module Summary

**What is this:** A recap of everything covered in Section 3.

**Description:** This final lesson reviews the full set of concepts introduced in the section — no new code. Use it to consolidate your understanding before moving on.

**What was covered:**
- File-based routing with the App Router (`page.js`, `layout.js`, `loading.js`, `error.js`, `not-found.js`)
- Server Components vs Client Components (`'use client'`)
- Dynamic routes (`[slug]`) and route parameters via `params`
- Nested layouts and the layout tree
- CSS Modules for scoped styling
- `<Image>` component with `fill` for unknown dimensions
- `<Link>` for client-side navigation
- SQLite with `better-sqlite3` for server-side data
- Server Actions (`'use server'`) for form handling without API routes
- `useFormStatus` and `useFormState` for form state management
- Server-side input validation
- `revalidatePath()` for cache invalidation after mutations
- `notFound()` and `redirect()` helpers
- Static and dynamic metadata (`metadata` export vs `generateMetadata`)
- AWS S3 for production file storage

---

# Section 4: Routing & Page Rendering — Deep Dive

---

## 134. Module Introduction

**What is this:** An overview of the advanced routing and rendering concepts covered in Section 4.

**Description:** This section goes beyond basic file-based routing and digs into parallel routes, intercepting routes, catch-all routes, route groups, middleware, and API route handlers. The goal is a thorough understanding of how the App Router handles complex real-world navigation patterns.

---

## 135. Project Setup, Overview & An Exercise!

**What is this:** Introducing the section's demo project and giving you an exercise to attempt before seeing the solution.

**Description:** A new starter project is provided (a news/archive app). You're asked to set up the routing structure independently — defining pages, layouts, and navigation — before the instructor walks through the solution in the next two lessons.

---

## 136. Exercise Solution — Part 1

**What is this:** Walkthrough of the routing structure and main layout for the exercise project.

**Description:** This lesson covers creating the root layout, main navigation with `<Link>`, and the top-level page files (`/`, `/news`, `/archive`). It confirms the expected folder structure and component organization.

**Examples:**

```jsx
// app/layout.js
import Link from 'next/link';

export const metadata = { title: 'NextJS Deep Dive' };

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <header>
          <nav>
            <ul>
              <li><Link href="/news">News</Link></li>
              <li><Link href="/archive">Archive</Link></li>
            </ul>
          </nav>
        </header>
        <main>{children}</main>
      </body>
    </html>
  );
}
```

```jsx
// app/news/page.js → /news
export default function NewsPage() {
  return <h1>News</h1>;
}

// app/archive/page.js → /archive
export default function ArchivePage() {
  return <h1>Archive</h1>;
}
```

---

## 137. Exercise Solution — Part 2

**What is this:** Completing the exercise solution — dynamic news detail route and archive filtering.

**Description:** This lesson adds the dynamic `[slug]` route for individual news articles and the archive route that accepts year/month segments. It reinforces dynamic routing and `params` usage from Section 3.

**Examples:**

```jsx
// app/news/[slug]/page.js → /news/:slug
export default function NewsDetailPage({ params }) {
  return <h1>News Article: {params.slug}</h1>;
}
```

```jsx
// app/archive/[year]/page.js → /archive/:year
export default function ArchiveYearPage({ params }) {
  return <h1>Archive for {params.year}</h1>;
}
```

---

## 138. App Styling & Using Dummy Data

**What is this:** Applying CSS styles and wiring up hardcoded dummy data to make the demo app look realistic.

**Description:** This lesson adds CSS Modules to the project pages and imports a local dummy data file (a JS object with news articles) instead of a real database. The focus is on making the UI presentable before introducing advanced routing concepts.

**Examples:**

```js
// lib/news.js — dummy data (no DB needed for this section)
const NEWS = [
  {
    id: 'n1',
    slug: 'will-ai-replace-humans',
    title: 'Will AI Replace Humans?',
    image: 'ai-image.jpg',
    date: '2024-05-01',
    content: 'Detailed article content here...',
  },
  // more articles...
];

export function getAllNews() {
  return NEWS;
}

export function getNewsItem(slug) {
  return NEWS.find((news) => news.slug === slug);
}

export function getLatestNews() {
  return NEWS.sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 3);
}

export function getAvailableNewsYears() {
  return [...new Set(NEWS.map((n) => new Date(n.date).getFullYear()))];
}
```

---

## 139. Handling "Not Found" Errors & Showing "Not Found" Pages

**What is this:** Using `notFound()` and `not-found.js` for route segments that have no matching data.

**Description:** This is the same pattern from Section 3 applied to the news app. If a news slug doesn't exist in the dummy data, `notFound()` is called and the nearest `not-found.js` is shown. A segment-level `not-found.js` takes priority over the root one.

**Examples:**

```jsx
// app/news/[slug]/page.js
import { notFound } from 'next/navigation';
import { getNewsItem } from '@/lib/news';

export default function NewsDetailPage({ params }) {
  const newsItem = getNewsItem(params.slug);

  if (!newsItem) notFound();

  return (
    <article>
      <h1>{newsItem.title}</h1>
      <p>{newsItem.content}</p>
    </article>
  );
}
```

```jsx
// app/news/[slug]/not-found.js
export default function NotFound() {
  return (
    <div>
      <h2>News article not found!</h2>
      <p>Unfortunately, the requested article could not be found.</p>
    </div>
  );
}
```

---

## 140. Setting Up & Using Parallel Routes

**What is this:** Rendering multiple independent pages simultaneously in the same layout using `@slot` folders.

**Description:** Parallel routes let you render more than one page component in a single layout at the same time — useful for dashboards, split views, or modal-style UIs. Each parallel slot is defined with an `@slotName` folder. The layout receives each slot as a prop.

**Examples:**

```
app/
└── archive/
    ├── layout.js         ← receives @archive and @latest as props
    ├── page.js           ← fallback for the default slot
    ├── @archive/
    │   └── page.js       ← renders in the archive slot
    └── @latest/
        └── page.js       ← renders in the latest slot
```

```jsx
// app/archive/layout.js
export default function ArchiveLayout({ archive, latest }) {
  return (
    <div style={{ display: 'flex', gap: '2rem' }}>
      <div style={{ flex: 2 }}>{archive}</div>
      <div style={{ flex: 1 }}>{latest}</div>
    </div>
  );
}
// archive and latest are the parallel slot components — rendered side by side
```

```jsx
// app/archive/@latest/page.js
import { getLatestNews } from '@/lib/news';

export default function LatestNewsPage() {
  const latestNews = getLatestNews();
  return (
    <ul>
      {latestNews.map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}
```

---

## 141. Working with Parallel Routes & Nested Routes

**What is this:** Adding nested dynamic routes inside a parallel slot.

**Description:** Each `@slot` folder is its own route subtree — it can have nested folders and dynamic segments just like a regular route. This lesson adds a `[year]` nested route inside `@archive` to filter news by year, while `@latest` stays unchanged.

**Examples:**

```
app/archive/
├── layout.js
├── @archive/
│   ├── page.js              → /archive (shows all years)
│   └── [year]/
│       └── page.js          → /archive/2024 (shows articles for 2024)
└── @latest/
    └── page.js              → always shows latest news
```

```jsx
// app/archive/@archive/[year]/page.js
import { getNewsForYear } from '@/lib/news';

export default function FilteredNewsPage({ params }) {
  const news = getNewsForYear(params.year);
  return (
    <ul>
      {news.map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}
```

```jsx
// app/archive/layout.js — layout gets children too if a page.js exists at this level
export default function ArchiveLayout({ archive, latest, children }) {
  return (
    <div>
      {children} {/* rendered if visiting /archive directly */}
      <div style={{ display: 'flex' }}>
        <div>{archive}</div>
        <div>{latest}</div>
      </div>
    </div>
  );
}
```

---

## 142. Configuring Catch-All Routes

**What is this:** Using `[...segments]` to match any number of URL path segments with a single route file.

**Description:** A catch-all route `[...slug]` captures zero or more path segments into an array. This is useful when the depth of the URL is variable — e.g., `/archive/2024`, `/archive/2024/04`, `/archive/2024/04/15` all handled by one file.

**Examples:**

```
app/archive/@archive/
└── [[...filter]]/
    └── page.js   → matches /archive, /archive/2024, /archive/2024/04, etc.
```

```jsx
// app/archive/@archive/[[...filter]]/page.js
// [[...filter]] — double brackets make it optional (matches /archive too)
import { getAvailableNewsYears, getNewsForYear, getNewsForYearAndMonth } from '@/lib/news';

export default function FilteredNewsPage({ params }) {
  const filter = params.filter; // e.g. ['2024'] or ['2024', '04'] or undefined

  if (!filter) {
    // /archive — no filter, show available years
    const years = getAvailableNewsYears();
    return <ul>{years.map(y => <li key={y}>{y}</li>)}</ul>;
  }

  if (filter.length === 1) {
    // /archive/2024 — filter by year
    const news = getNewsForYear(filter[0]);
    return <ul>{news.map(n => <li key={n.id}>{n.title}</li>)}</ul>;
  }

  if (filter.length === 2) {
    // /archive/2024/04 — filter by year and month
    const news = getNewsForYearAndMonth(filter[0], filter[1]);
    return <ul>{news.map(n => <li key={n.id}>{n.title}</li>)}</ul>;
  }
}
```

---

## 143. Catch-All Fallback Routes & Dealing With Multiple Path Segments

**What is this:** The difference between `[...slug]` and `[[...slug]]` and how to handle invalid segment combinations.

**Description:** `[...slug]` requires at least one segment — it won't match the base route. `[[...slug]]` (double brackets) is optional — it also matches the base route with no segments. When a user hits an invalid filter combination (e.g., 3 segments), call `notFound()`.

**Examples:**

```jsx
// [...slug]   — required: matches /archive/2024 but NOT /archive
// [[...slug]] — optional: matches /archive AND /archive/2024

// app/archive/@archive/[[...filter]]/page.js
import { notFound } from 'next/navigation';

export default function FilteredNewsPage({ params }) {
  const filter = params.filter;

  // guard: reject anything with more than 2 segments
  if (filter && filter.length > 2) notFound();

  // normal rendering...
}
```

---

## 144. Throwing (Route-related) Errors

**What is this:** Throwing errors from Server Components and how NextJS catches them.

**Description:** You can throw any `Error` from a Server Component (e.g., when data is unavailable). NextJS catches it and renders the nearest `error.js` boundary. This is separate from `notFound()` — use errors for unexpected failures, `notFound()` for missing resources.

**Examples:**

```jsx
// app/news/[slug]/page.js
import { getNewsItem } from '@/lib/news';
import { notFound } from 'next/navigation';

export default function NewsDetailPage({ params }) {
  const newsItem = getNewsItem(params.slug);

  if (!newsItem) notFound(); // expected missing resource → not-found.js

  // unexpected failure — triggers error.js
  if (someConditionThatShouldNeverHappen) {
    throw new Error('Failed to load news item.');
  }

  return <article>{newsItem.title}</article>;
}
```

---

## 145. Handling Errors With Error Pages

**What is this:** Creating `error.js` files to gracefully catch and display route-level errors.

**Description:** An `error.js` file is an automatic error boundary for its route segment. It must be a Client Component. It receives `error` (the thrown Error object) and `reset` (a function to retry). Placing it at different levels controls how much of the UI it replaces on failure.

**Examples:**

```jsx
// app/news/[slug]/error.js — handles errors in this segment only
'use client';

export default function NewsError({ error, reset }) {
  return (
    <div>
      <h2>An error occurred while loading this article.</h2>
      <p>{error.message}</p>
      <button onClick={reset}>Try Again</button>
    </div>
  );
}
```

```jsx
// app/error.js — root-level error boundary, catches anything not handled closer
'use client';

export default function GlobalError({ error, reset }) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={reset}>Try Again</button>
    </div>
  );
}
```

---

## 146. Server vs Client Components

**What is this:** A deeper revisit of the Server vs Client Component distinction with more nuanced examples.

**Description:** This lesson reinforces the rules from Section 3 with new scenarios: when a Server Component imports a Client Component, the Client Component boundary is respected — its children are still server-rendered unless they also opt in with `'use client'`. The `'use client'` directive marks the boundary, not every component below it individually.

**Examples:**

```jsx
// Server Component — no directive, renders on server
// app/news/page.js
import NewsList from '@/components/news-list'; // also a Server Component

export default function NewsPage() {
  return <NewsList />;
}
```

```jsx
// Client Component — opts in with directive
// components/nav-link.js
'use client';
import { usePathname } from 'next/navigation';
import Link from 'next/link';

export default function NavLink({ href, children }) {
  const path = usePathname();
  return <Link href={href} className={path === href ? 'active' : ''}>{children}</Link>;
}

// Any component imported BY NavLink also runs on the client
// But components passed as props/children to NavLink from the server stay server-rendered
```

---

## 147. Nested Routes Inside Dynamic Routes

**What is this:** Adding nested route segments inside a dynamic `[slug]` folder.

**Description:** Dynamic route segments can have their own sub-folders, creating nested dynamic routes. This is useful for detail pages that have their own tabs or sub-pages (e.g., `/news/[slug]/comments`).

**Examples:**

```
app/news/
└── [slug]/
    ├── page.js             → /news/:slug
    └── image/
        └── page.js         → /news/:slug/image
```

```jsx
// app/news/[slug]/image/page.js
import { getNewsItem } from '@/lib/news';
import { notFound } from 'next/navigation';

export default function NewsImagePage({ params }) {
  const newsItem = getNewsItem(params.slug);
  if (!newsItem) notFound();

  return (
    <div>
      <img src={newsItem.image} alt={newsItem.title} />
    </div>
  );
}
```

---

## 148. Intercepting Navigation & Using Interception Routes

**What is this:** Using interception routes to show a modal when navigating to a route, while showing the full page on direct access.

**Description:** Interception routes let you "intercept" a navigation to a route and show a different UI in-place (e.g., a modal) — without the user leaving the current page. On direct URL access or refresh, the real page renders normally. Interception is defined using `(.)`, `(..)`, `(..)(..)`, or `(...)` prefix conventions.

**Interception prefixes:**

| Prefix | Intercepts |
|---|---|
| `(.)slug` | Same level |
| `(..)slug` | One level up |
| `(..)(..)slug` | Two levels up |
| `(...)slug` | From the root |

**Examples:**

```
app/news/
├── [slug]/
│   ├── page.js              → full page on direct access
│   └── image/
│       └── page.js          → full image page on direct access
└── (.)news/
    └── [slug]/
        └── image/
            └── page.js      → intercepted: shows modal when navigating there from the news list
```

```jsx
// app/news/(.)news/[slug]/image/page.js
// This intercepts navigation to /news/[slug]/image and renders a modal instead

import { getNewsItem } from '@/lib/news';
import { notFound } from 'next/navigation';

export default function InterceptedImagePage({ params }) {
  const newsItem = getNewsItem(params.slug);
  if (!newsItem) notFound();

  return (
    <div className="modal">
      <img src={newsItem.image} alt={newsItem.title} />
    </div>
  );
}
```

---

## 149. Combining Parallel & Intercepting Routes

**What is this:** Using interception routes together with parallel routes to implement a modal pattern.

**Description:** The classic pattern: a `@modal` parallel slot + an interception route. When a user clicks a link, the interception route renders the modal in the `@modal` slot (overlaying the current page). On direct access or refresh, the real route renders and the modal slot is empty (via `default.js`).

**Examples:**

```
app/
├── layout.js              ← receives @modal slot
├── @modal/
│   ├── default.js         ← empty — no modal by default
│   └── (.)news/
│       └── [slug]/
│           └── image/
│               └── page.js  ← modal content when intercepted
└── news/
    └── [slug]/
        └── image/
            └── page.js      ← full page on direct access
```

```jsx
// app/layout.js — renders modal slot alongside normal content
export default function RootLayout({ children, modal }) {
  return (
    <html lang="en">
      <body>
        {modal}   {/* renders the intercepted modal, or nothing via default.js */}
        {children}
      </body>
    </html>
  );
}
```

```jsx
// app/@modal/default.js — renders nothing, no modal by default
export default function ModalDefault() {
  return null;
}
```

```jsx
// app/@modal/(.)news/[slug]/image/page.js
'use client';

import { useRouter } from 'next/navigation';
import { getNewsItem } from '@/lib/news';

export default function InterceptedImageModal({ params }) {
  const router = useRouter();
  const newsItem = getNewsItem(params.slug);

  return (
    <div className="modal-backdrop" onClick={() => router.back()}>
      <div className="modal" onClick={(e) => e.stopPropagation()}>
        <img src={newsItem.image} alt={newsItem.title} />
      </div>
    </div>
  );
}
```

---

## 150. Replace page.js with default.js

**What is this:** Using `default.js` as a fallback for parallel route slots when no matching sub-route exists.

**Description:** When a parallel slot has no `page.js` that matches the current URL, NextJS needs a fallback to avoid a 404. `default.js` serves this role — it renders when the slot has no active match. Returning `null` from `default.js` means the slot renders nothing (invisible).

**Examples:**

```jsx
// app/@modal/default.js — render nothing when no modal is active
export default function Default() {
  return null;
}
```

```jsx
// app/archive/@latest/default.js — keep showing latest news even when archive slot navigates
import { getLatestNews } from '@/lib/news';

export default function LatestNewsDefault() {
  const latestNews = getLatestNews();
  return (
    <ul>
      {latestNews.map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}
```

---

## 151. Navigating Programmatically

**What is this:** Using the `useRouter` hook to navigate without a `<Link>` component.

**Description:** For navigating in response to events (button clicks, form submissions, after a timeout) you use `useRouter` from `next/navigation`. It provides `push`, `replace`, `back`, and `refresh` methods. The component must be a Client Component.

**Examples:**

```jsx
// components/modal.js
'use client';

import { useRouter } from 'next/navigation';

export default function Modal({ children }) {
  const router = useRouter();

  function closeModal() {
    router.back(); // go back to previous route — closes the modal
  }

  return (
    <div className="backdrop" onClick={closeModal}>
      <dialog open onClick={(e) => e.stopPropagation()}>
        {children}
      </dialog>
    </div>
  );
}
```

```jsx
// Other router methods:
router.push('/news');        // navigate to a new route (adds to history)
router.replace('/news');     // navigate without adding to history
router.back();               // go back one step
router.refresh();            // re-fetch server data for the current route
```

---

## 152. Defining the Base HTML Document

**What is this:** Understanding that the root `layout.js` is the base HTML document — there is no separate `_document.js` in the App Router.

**Description:** In the Pages Router, `_document.js` defined the `<html>`, `<head>`, and `<body>` tags. In the App Router this is gone — the root `app/layout.js` is responsible for rendering the full HTML shell. You add fonts, global attributes, and `<head>` metadata directly here.

**Examples:**

```jsx
// app/layout.js — this IS the HTML document
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });

export const metadata = {
  title: 'NextJS Deep Dive',
  description: 'Advanced routing and rendering patterns.',
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      {/* <head> is managed automatically by NextJS via the metadata export */}
      <body className={inter.className}>
        {children}
      </body>
    </html>
  );
}
```

---

## 153. Using & Understanding Route Groups

**What is this:** Using `(folderName)` to organize routes without affecting the URL structure.

**Description:** Wrapping a folder in parentheses creates a **route group** — it's invisible to the URL but lets you organize files, apply separate layouts, or exclude segments from routes. This is useful for grouping marketing pages under one layout and app pages under another, without changing their URLs.

**Examples:**

```
app/
├── (marketing)/
│   ├── layout.js       ← layout only for marketing pages (no auth header)
│   ├── page.js         → /  (homepage)
│   └── about/
│       └── page.js     → /about
└── (app)/
    ├── layout.js       ← layout only for app pages (with sidebar)
    ├── dashboard/
    │   └── page.js     → /dashboard
    └── settings/
        └── page.js     → /settings

// (marketing) and (app) don't appear in the URL at all
```

```jsx
// app/(marketing)/layout.js — only wraps marketing pages
export default function MarketingLayout({ children }) {
  return (
    <div className="marketing-wrapper">
      <header>Public Site Header</header>
      {children}
    </div>
  );
}
```

---

## 154. Building APIs with Route Handlers

**What is this:** Creating backend API endpoints inside the NextJS app using `route.js` files.

**Description:** `route.js` files define HTTP endpoint handlers. Export named functions matching HTTP methods (`GET`, `POST`, `PUT`, `DELETE`, etc.). They receive a `Request` object and return a `Response` (or `NextResponse`). Route handlers can access the DB directly — they run on the server.

**Examples:**

```js
// app/api/news/route.js → GET /api/news
import { NextResponse } from 'next/server';
import { getAllNews } from '@/lib/news';

export function GET() {
  const news = getAllNews();
  return NextResponse.json({ news });
}
```

```js
// app/api/news/[slug]/route.js → GET /api/news/:slug
import { NextResponse } from 'next/server';
import { getNewsItem } from '@/lib/news';

export function GET(request, { params }) {
  const newsItem = getNewsItem(params.slug);

  if (!newsItem) {
    return NextResponse.json({ message: 'Not found' }, { status: 404 });
  }

  return NextResponse.json({ newsItem });
}
```

```js
// POST handler — read request body
export async function POST(request) {
  const body = await request.json();
  // process body...
  return NextResponse.json({ message: 'Created' }, { status: 201 });
}
```

---

## 155. Using Middleware

**What is this:** Running code before every request using a `middleware.js` file in the project root.

**Description:** Middleware runs on the Edge before a request hits a route handler or page. It can redirect, rewrite, add headers, or block requests. The `matcher` config controls which routes trigger it. Common use cases: auth guards, locale redirects, A/B testing.

**Examples:**

```js
// middleware.js — must be in the project root (next to app/)
import { NextResponse } from 'next/server';

export function middleware(request) {
  const { pathname } = request.nextUrl;

  // Example: redirect /old-news to /news
  if (pathname === '/old-news') {
    return NextResponse.redirect(new URL('/news', request.url));
  }

  // Example: add a custom header to every response
  const response = NextResponse.next();
  response.headers.set('x-custom-header', 'my-value');
  return response;
}

// Only run middleware on these paths (optional — defaults to all routes)
export const config = {
  matcher: ['/news/:path*', '/archive/:path*'],
};
```

```js
// Auth guard example
export function middleware(request) {
  const isAuthenticated = request.cookies.get('token');

  if (!isAuthenticated && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url));
  }

  return NextResponse.next();
}
```

---

# Section 5: Data Fetching — Deep Dive

---

## 157. Module Introduction

**What is this:** An overview of the different data fetching strategies available in NextJS and when to use each one.

**Description:** This section explores the full spectrum of data fetching in NextJS — client-side with `useEffect`, server-side with async Server Components, and fetching directly from a database without any HTTP layer. It also covers Suspense-based granular loading states and migrating from a REST API to a direct DB connection.

---

## 158. Adding a Backend

**What is this:** Setting up a separate backend API (Express/Node) that the NextJS app will fetch data from.

**Description:** For the first part of this section, the project uses a standalone backend server (a simple Express app) that exposes REST endpoints. This simulates a real-world setup where the frontend and backend are separate services. The NextJS app fetches from this API — first on the client, then on the server.

**Examples:**

```js
// backend/app.js (separate Express server — not part of the Next app)
const express = require('express');
const app = express();

const MESSAGES = [
  { id: 'm1', text: 'Hello World' },
  { id: 'm2', text: 'NextJS is great' },
];

app.use((req, res, next) => {
  res.setHeader('Access-Control-Allow-Origin', '*'); // allow NextJS dev server
  next();
});

app.get('/messages', (req, res) => {
  res.json({ messages: MESSAGES });
});

app.listen(8080, () => console.log('Backend running on :8080'));
```

```bash
# Run both servers concurrently during development
node backend/app.js   # backend on :8080
npm run dev           # NextJS on :3000
```

---

## 159. Option 1: Client-side Data Fetching

**What is this:** Fetching data from the client using `useEffect` and `useState` — the traditional React SPA approach.

**Description:** Client-side fetching works inside a Client Component. The component renders first with empty state, then `useEffect` fires after mount, calls `fetch()`, and updates state with the result. This is fine for non-SEO-critical, user-specific, or frequently updated data, but the initial render has no data (bad for SEO, shows flash of empty content).

**Examples:**

```jsx
// app/messages/page.js
'use client';

import { useState, useEffect } from 'react';

export default function MessagesPage() {
  const [messages, setMessages] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function loadMessages() {
      setIsLoading(true);
      try {
        const response = await fetch('http://localhost:8080/messages');
        if (!response.ok) throw new Error('Failed to fetch messages.');
        const data = await response.json();
        setMessages(data.messages);
      } catch (err) {
        setError(err.message);
      }
      setIsLoading(false);
    }
    loadMessages();
  }, []);

  if (isLoading) return <p>Loading messages...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <ul>
      {messages.map((msg) => (
        <li key={msg.id}>{msg.text}</li>
      ))}
    </ul>
  );
}
```

---

## 160. Option 2: Server-side Data Fetching

**What is this:** Fetching data inside an async Server Component — the NextJS-native approach.

**Description:** Server Components can be `async` functions. You `await fetch()` directly in the component body — no `useEffect`, no `useState`, no loading state boilerplate. The HTML sent to the browser already includes the data. This is better for SEO and performance because the page arrives pre-populated.

**Examples:**

```jsx
// app/messages/page.js — Server Component (no 'use client')
export default async function MessagesPage() {
  const response = await fetch('http://localhost:8080/messages');

  if (!response.ok) {
    throw new Error('Failed to fetch messages.');
  }

  const data = await response.json();
  const { messages } = data;

  return (
    <ul>
      {messages.map((msg) => (
        <li key={msg.id}>{msg.text}</li>
      ))}
    </ul>
  );
}
// No client JS for fetching — data arrives with the HTML
```

---

## 161. Why Use A Separate Backend? Fetching Directly From The Source!

**What is this:** Skipping the HTTP layer entirely by importing DB functions directly into Server Components.

**Description:** When your NextJS app and your data source are on the same server, making an HTTP request to your own API is unnecessary overhead. Server Components run on the server — they can import and call database functions (or any Node.js module) directly. This is simpler, faster, and avoids the network round-trip.

**Examples:**

```js
// lib/messages.js — direct DB access (e.g. better-sqlite3)
import sql from 'better-sqlite3';
const db = sql('messages.db');

export function getMessages() {
  return db.prepare('SELECT * FROM messages').all();
}
```

```jsx
// app/messages/page.js — no fetch(), no API, no network hop
import { getMessages } from '@/lib/messages';

export default async function MessagesPage() {
  const messages = getMessages(); // direct DB call on the server

  return (
    <ul>
      {messages.map((msg) => (
        <li key={msg.id}>{msg.text}</li>
      ))}
    </ul>
  );
}

// When to still use a separate backend + fetch():
// - The backend is a different team's service
// - You need to share the API with a mobile app
// - The DB is on a different infrastructure
// Otherwise — go direct.
```

---

## 162. Showing a "Loading" Fallback

**What is this:** Using `loading.js` and `<Suspense>` to show a fallback UI while async Server Components resolve.

**Description:** This lesson revisits `loading.js` and `<Suspense>` (introduced in Section 3) in the context of server-side data fetching. When an async Server Component is awaiting data, the nearest `loading.js` or `<Suspense fallback>` is shown until the data resolves and the component can render.

**Examples:**

```jsx
// app/messages/loading.js — auto-shown while page.js is pending
export default function MessagesLoading() {
  return <p>Loading messages, please wait...</p>;
}
```

```jsx
// Or use Suspense directly for more granular control
import { Suspense } from 'react';
import Messages from '@/components/messages';

export default function MessagesPage() {
  return (
    <main>
      <h1>Messages</h1>
      <Suspense fallback={<p>Loading messages...</p>}>
        <Messages /> {/* async component — fetches its own data */}
      </Suspense>
    </main>
  );
}
```

---

## 163. Migrating An Entire Application To A Local Data Source (Database)

**What is this:** Replacing all `fetch()` calls to the external backend with direct database queries.

**Description:** This lesson walks through a full migration — removing the Express backend dependency and replacing every `fetch('http://localhost:8080/...')` call with a direct import from `lib/messages.js` (which uses `better-sqlite3`). The result is a simpler, fully server-rendered app with no separate backend process.

**Examples:**

```js
// Before — fetch from external API
const response = await fetch('http://localhost:8080/messages');
const data = await response.json();
const messages = data.messages;
```

```js
// After — direct DB call, no HTTP
import { getMessages } from '@/lib/messages';
const messages = getMessages();
```

```js
// lib/messages.js — full implementation
import sql from 'better-sqlite3';

const db = sql('messages.db');

db.exec(`
  CREATE TABLE IF NOT EXISTS messages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL
  )
`);

export function getMessages() {
  return db.prepare('SELECT * FROM messages').all();
}

export function addMessage(text) {
  db.prepare('INSERT INTO messages (text) VALUES (?)').run(text);
}
```

---

## 164. Granular Data Fetching With Suspense

**What is this:** Splitting a page into multiple async components, each wrapped in its own `<Suspense>`, so they load independently.

**Description:** Instead of one big async page that waits for all data before rendering anything, you break the page into smaller async components — each fetches its own data and is wrapped in its own `<Suspense>`. This way, fast sections render immediately while slow sections stream in when ready. This is **streaming** in practice.

**Examples:**

```jsx
// components/messages.js — async component, fetches its own data
import { getMessages } from '@/lib/messages';

export default async function Messages() {
  const messages = await getMessages();

  if (!messages || messages.length === 0) {
    return <p>No messages found.</p>;
  }

  return (
    <ul>
      {messages.map((msg) => (
        <li key={msg.id}>{msg.text}</li>
      ))}
    </ul>
  );
}
```

```jsx
// app/messages/page.js — page renders instantly, Messages streams in separately
import { Suspense } from 'react';
import Messages from '@/components/messages';

export default function MessagesPage() {
  // This component is NOT async — it renders immediately
  // Only <Messages> is async and suspends
  return (
    <main>
      <h1>My Messages</h1>
      <Suspense fallback={<p>Loading messages...</p>}>
        <Messages />
      </Suspense>
    </main>
  );
}

// Benefit: if you add more async sections, each suspends independently
// → the user sees content progressively, not a blank screen until all data arrives
```

---

# Section 6: Mutating Data — Deep Dive

---

## 165. Module Introduction

**What is this:** An overview of the mutation patterns covered in Section 6 — forms, Server Actions, validation, optimistic updates, and caching.

**Description:** This section focuses on the write side of data — creating, updating, and deleting records. It covers Server Actions in depth: how to wire them to forms, validate input, provide user feedback, handle images, trigger cache revalidation, and implement optimistic UI updates.

---

## 166. Starting Project & Analyzing Mutation Options

**What is this:** Reviewing the section's starter project and comparing the available approaches for handling form submissions in NextJS.

**Description:** The starter is a messages app with a form. The lesson compares three mutation options:
1. **Client-side only** — `fetch` POST inside a Client Component with `useEffect`/`useState`
2. **API Route Handler** — a `POST /api/messages` endpoint called via `fetch`
3. **Server Actions** — the recommended NextJS-native approach: async server functions called directly from forms

**Comparison:**

| Approach | Where it runs | Setup needed |
|---|---|---|
| Client-side fetch | Browser | Separate API endpoint, CORS if needed |
| Route Handler | Server (via HTTP) | `route.js` file, still an HTTP round-trip |
| Server Action | Server (direct call) | Just a function — no API, no HTTP overhead |

---

## 167. Setting Up A Form Action

**What is this:** Wiring a native HTML form to a Server Action using the `action` prop.

**Description:** In NextJS, you pass a Server Action function directly to a form's `action` prop. When the form submits, NextJS calls the function on the server with the `FormData`. No JavaScript event handler is needed on the client — this even works with JS disabled.

**Examples:**

```jsx
// app/messages/new/page.js
export default function NewMessagePage() {
  async function createMessage(formData) {
    'use server';
    const text = formData.get('message');
    console.log('Received on server:', text);
  }

  return (
    <form action={createMessage}>
      <label htmlFor="message">Your Message</label>
      <textarea id="message" name="message" rows="5" required />
      <button type="submit">Send</button>
    </form>
  );
}
```

---

## 168. Creating a Server Action

**What is this:** Writing a proper Server Action that receives `FormData`, validates it, and saves to the database.

**Description:** A Server Action is an `async` function marked with `'use server'`. It runs exclusively on the server — the client only triggers it. It receives the form's `FormData`, extracts values, and performs the mutation (DB insert, file write, etc.).

**Examples:**

```jsx
// app/messages/new/page.js
import { redirect } from 'next/navigation';
import { addMessage } from '@/lib/messages';

export default function NewMessagePage() {
  async function createMessage(formData) {
    'use server';

    const text = formData.get('message');

    if (!text || text.trim() === '') {
      return; // validation — handle errors properly in later lessons
    }

    addMessage(text);
    redirect('/messages'); // redirect after mutation
  }

  return (
    <form action={createMessage}>
      <textarea name="message" rows="5" required />
      <button type="submit">Send</button>
    </form>
  );
}
```

---

## 169. Storing Data in Databases

**What is this:** Implementing the `addMessage` DB function that the Server Action calls.

**Description:** The mutation is handled by a plain function in `lib/messages.js` that uses `better-sqlite3`. The Server Action imports and calls it — the DB write happens entirely on the server, never exposed to the client.

**Examples:**

```js
// lib/messages.js
import sql from 'better-sqlite3';

const db = sql('messages.db');

db.exec(`
  CREATE TABLE IF NOT EXISTS messages (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    text TEXT NOT NULL
  )
`);

export function getMessages() {
  return db.prepare('SELECT * FROM messages').all();
}

export function addMessage(text) {
  db.prepare('INSERT INTO messages (text) VALUES (?)').run(text);
}
```

```jsx
// lib/actions.js — Server Action calls the DB helper
'use server';

import { redirect } from 'next/navigation';
import { addMessage } from './messages';

export async function createMessage(formData) {
  const text = formData.get('message');
  addMessage(text);
  redirect('/messages');
}
```

---

## 170. Providing User Feedback with The useFormStatus Hook

**What is this:** Using `useFormStatus` to disable the submit button and show a loading state while the Server Action is running.

**Description:** `useFormStatus` (from `react-dom`) reads the pending state of the nearest parent form's Server Action. It must live in a **child** Client Component of the form — not in the form's own component. While the action is pending, `pending` is `true`.

**Examples:**

```jsx
// components/form-submit.js
'use client';

import { useFormStatus } from 'react-dom';

export default function FormSubmit() {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Sending...' : 'Send Message'}
    </button>
  );
}
```

```jsx
// app/messages/new/page.js
import FormSubmit from '@/components/form-submit';
import { createMessage } from '@/lib/actions';

export default function NewMessagePage() {
  return (
    <form action={createMessage}>
      <textarea name="message" rows="5" required />
      <FormSubmit />
    </form>
  );
}
```

---

## 171. Using useFormState Hook

**What is this:** Using `useFormState` to receive and render the value returned by a Server Action.

**Description:** `useFormState` (from `react-dom`) connects a form to a Server Action and gives access to whatever the action returns — typically an error or success message. The component using it must be a Client Component. The action receives `prevState` as its first argument before `formData`.

**Examples:**

```jsx
// components/message-form.js
'use client';

import { useFormState } from 'react-dom';
import { createMessage } from '@/lib/actions';
import FormSubmit from './form-submit';

export default function MessageForm() {
  const [formState, formAction] = useFormState(createMessage, { errors: [] });

  return (
    <form action={formAction}>
      <textarea name="message" rows="5" required />
      {formState.errors.length > 0 && (
        <ul>
          {formState.errors.map((err) => <li key={err}>{err}</li>)}
        </ul>
      )}
      <FormSubmit />
    </form>
  );
}
```

---

## 172. Validating User Input With Help Of The useFormState Hook

**What is this:** Running server-side validation in a Server Action and returning errors back to the form via `useFormState`.

**Description:** Validation happens inside the Server Action on the server. If input is invalid, the action returns an object with an `errors` array instead of redirecting. `useFormState` passes this return value back to the component as `formState`, which renders the errors.

**Examples:**

```js
// lib/actions.js
'use server';

import { redirect } from 'next/navigation';
import { addMessage } from './messages';

export async function createMessage(prevState, formData) {
  const text = formData.get('message');

  const errors = [];

  if (!text || text.trim() === '') {
    errors.push('Message must not be empty.');
  }
  if (text.trim().length < 5) {
    errors.push('Message must be at least 5 characters long.');
  }

  if (errors.length > 0) {
    return { errors }; // returned to useFormState as formState
  }

  addMessage(text);
  redirect('/messages'); // only redirect on success
}
```

---

## 173. Adjusting Server Actions for useFormState

**What is this:** The one change required to make a Server Action compatible with `useFormState` — adding `prevState` as the first parameter.

**Description:** `useFormState` injects `prevState` as the first argument to the action (before `formData`). Without this parameter, the action receives the previous state as `formData`, and the actual form data is lost. This is a common gotcha when upgrading existing actions.

**Examples:**

```js
// Before — works with plain form action, breaks with useFormState
export async function createMessage(formData) {
  const text = formData.get('message');
  // ...
}

// After — compatible with useFormState
export async function createMessage(prevState, formData) {
  //                                ^^^^^^^^^^
  // prevState is whatever was returned last time (or the initial state)
  const text = formData.get('message');
  // ...
}
```

---

## 174. Storing Server Actions In Separate Files

**What is this:** Moving Server Actions out of page components into a dedicated `lib/actions.js` file for reusability and clarity.

**Description:** Inline Server Actions (defined inside a component with `'use server'`) work but only for that component. Moving them to a separate file with `'use server'` at the top makes every export in that file a Server Action — importable from anywhere in the app.

**Examples:**

```js
// lib/actions.js
'use server'; // all exports in this file are Server Actions

import { redirect } from 'next/navigation';
import { addMessage } from './messages';

export async function createMessage(prevState, formData) {
  const text = formData.get('message');

  if (!text || text.trim().length < 5) {
    return { errors: ['Message must be at least 5 characters.'] };
  }

  addMessage(text);
  redirect('/messages');
}
```

```jsx
// components/message-form.js — imports the action from the separate file
'use client';

import { useFormState } from 'react-dom';
import { createMessage } from '@/lib/actions'; // clean import

export default function MessageForm() {
  const [formState, formAction] = useFormState(createMessage, { errors: [] });
  return <form action={formAction}>{/* ... */}</form>;
}
```

---

## 175. "use server" Does Not Guarantee Server-side Execution!

**What is this:** An important security reminder — `'use server'` marks a function as callable from the client, not as private server code.

**Description:** `'use server'` does **not** mean the function is hidden or secure. It means NextJS exposes it as a callable server endpoint. Any data you pass to it or return from it travels over the network. Sensitive logic (like checking auth) must be done **inside** the action, not assumed to be safe because it's on the server.

**Key points:**
- Server Actions are exposed as HTTP POST endpoints under the hood
- Anyone can call them directly — treat them like API routes for security purposes
- Always validate and authorize inside the action itself
- Never trust the client to send only valid/safe data

**Examples:**

```js
// lib/actions.js
'use server';

import { getSession } from '@/lib/auth';

export async function deleteMessage(prevState, formData) {
  // Always check auth INSIDE the action — never assume the caller is trusted
  const session = await getSession();

  if (!session) {
    return { errors: ['Not authenticated.'] };
  }

  const id = formData.get('id');
  // proceed with deletion...
}
```

---

## 176. Uploading & Storing Images

**What is this:** Handling file uploads in a Server Action — reading the `File` from `FormData` and writing it to disk or cloud storage.

**Description:** File inputs in a form send the file as a `File` object inside `FormData`. Inside the Server Action, you access it with `formData.get('image')`, convert it to a `Buffer`, and write it to the filesystem (dev) or upload to S3 (production). The stored path is saved to the DB.

**Examples:**

```jsx
// form with file input
<form action={createPost}>
  <input type="text" name="title" />
  <input type="file" name="image" accept="image/*" />
  <button type="submit">Post</button>
</form>
```

```js
// lib/actions.js
'use server';

import { writeFile } from 'fs/promises';
import path from 'path';
import { addPost } from './posts';

export async function createPost(prevState, formData) {
  const title = formData.get('title');
  const image = formData.get('image'); // File object

  if (!image || image.size === 0) {
    return { errors: ['Please select an image.'] };
  }

  const extension = image.name.split('.').pop();
  const fileName = `${Date.now()}.${extension}`;
  const filePath = path.join(process.cwd(), 'public', 'uploads', fileName);

  const buffer = Buffer.from(await image.arrayBuffer());
  await writeFile(filePath, buffer);

  addPost({ title, image: `/uploads/${fileName}` });

  redirect('/posts');
}
```

---

## 177. Alternative Ways of Using, Configuring & Triggering Server Actions

**What is this:** Triggering Server Actions outside of a form's `action` prop — from event handlers, buttons, and `startTransition`.

**Description:** Server Actions don't have to be tied to a `<form>`. You can call them from `onClick`, wrap them in `startTransition` for non-blocking execution, or bind arguments with `.bind()`. This enables patterns like delete buttons, toggles, and inline edits.

**Examples:**

```jsx
// Calling a Server Action from an onClick (must be in a Client Component)
'use client';

import { deleteMessage } from '@/lib/actions';

export default function DeleteButton({ id }) {
  return (
    <button onClick={() => deleteMessage(id)}>
      Delete
    </button>
  );
}
```

```jsx
// Using .bind() to pre-fill arguments — works in Server AND Client Components
import { deleteMessage } from '@/lib/actions';

export default function MessageItem({ message }) {
  const deleteWithId = deleteMessage.bind(null, message.id);

  return (
    <form action={deleteWithId}>
      <button type="submit">Delete</button>
    </form>
  );
}
```

```jsx
// Using startTransition for non-form triggers (keeps UI responsive)
'use client';

import { useTransition } from 'react';
import { deleteMessage } from '@/lib/actions';

export default function DeleteButton({ id }) {
  const [isPending, startTransition] = useTransition();

  function handleDelete() {
    startTransition(() => deleteMessage(id));
  }

  return (
    <button onClick={handleDelete} disabled={isPending}>
      {isPending ? 'Deleting...' : 'Delete'}
    </button>
  );
}
```

---

## 178. Revalidating Data To Avoid Caching Problems

**What is this:** Using `revalidatePath()` and `revalidateTag()` to bust the cache after a mutation so fresh data is served.

**Description:** NextJS caches fetch results and page renders aggressively. After a Server Action mutates data, the cached version is stale. `revalidatePath()` marks a specific route for regeneration on the next request. `revalidateTag()` invalidates all fetches tagged with a specific string.

**Examples:**

```js
// lib/actions.js
'use server';

import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';
import { addMessage } from './messages';

export async function createMessage(prevState, formData) {
  const text = formData.get('message');
  addMessage(text);

  revalidatePath('/messages'); // bust the /messages page cache
  redirect('/messages');
}
```

```js
// Using revalidateTag — tag fetches, then invalidate by tag
// In a fetch call:
const res = await fetch('/api/messages', { next: { tags: ['messages'] } });

// In the Server Action after mutation:
import { revalidateTag } from 'next/cache';
revalidateTag('messages'); // invalidates all fetches tagged 'messages'
```

---

## 179. Performing Optimistic Updates With NextJS

**What is this:** Using `useOptimistic` to instantly update the UI before the Server Action completes, then reconciling with the real result.

**Description:** Optimistic updates make the UI feel instant — you assume the mutation will succeed and update the UI immediately, then correct it if the server returns an error. React's `useOptimistic` hook handles this pattern. It takes the current state and an update function, returning a temporary optimistic state and a dispatch function.

**Examples:**

```jsx
// components/messages.js
'use client';

import { useOptimistic } from 'react';
import { deleteMessage } from '@/lib/actions';

export default function Messages({ messages }) {
  const [optimisticMessages, setOptimisticMessages] = useOptimistic(
    messages,
    (currentMessages, deletedId) =>
      currentMessages.filter((m) => m.id !== deletedId)
  );

  async function handleDelete(id) {
    setOptimisticMessages(id); // instantly remove from UI
    await deleteMessage(id);   // then actually delete on server
  }

  return (
    <ul>
      {optimisticMessages.map((msg) => (
        <li key={msg.id}>
          {msg.text}
          <button onClick={() => handleDelete(msg.id)}>Delete</button>
        </li>
      ))}
    </ul>
  );
}
```

---

## 180. Caching Differences: Development vs Production

**What is this:** Understanding why caching behaves differently in `npm run dev` vs `npm run build && npm start`.

**Description:** In development (`npm run dev`), NextJS disables most caching so you always see fresh data. In production (`npm start`), caching is aggressive — pages are pre-rendered and frozen at build time unless you explicitly opt out or revalidate. This means bugs that are invisible in dev can appear in production when cached stale data is served.

**Key differences:**

| | Development | Production |
|---|---|---|
| Page caching | Disabled — always fresh | Enabled — pre-rendered at build |
| `fetch` caching | Disabled by default | Enabled — respects `cache` option |
| `revalidatePath` needed? | No — always re-renders | Yes — required after mutations |
| `loading.js` visible? | Yes (real async) | May not appear if page is cached |

**Examples:**

```js
// Force a fetch to always be dynamic (no caching) — same in dev and prod
const res = await fetch('/api/messages', { cache: 'no-store' });

// Allow caching but revalidate every 60 seconds
const res = await fetch('/api/messages', { next: { revalidate: 60 } });

// Default (cache: 'force-cache') — cached until manually revalidated
const res = await fetch('/api/messages');
```

```bash
# Always test caching behavior in production mode before shipping
npm run build
npm start
# Then check if data updates appear correctly after mutations
```

---

# Section 7: Understanding & Configuring Caching

---

## 181. Module Introduction

**What is this:** An overview of NextJS's four caching layers and why understanding them is critical for correct app behavior.

**Description:** NextJS has multiple caching mechanisms that work at different levels. This section breaks each one down — Request Memoization, Data Cache, Full Route Cache, and Router Cache — and shows how to configure, opt out of, and invalidate each one.

---

## 182. Making Sense of NextJS' Caching Types

**What is this:** A map of the four distinct caching layers in NextJS and what each one stores.

**Description:** NextJS applies caching at four levels. Each layer has a different scope, lifetime, and invalidation mechanism. Understanding which layer applies to your situation determines which tool you use to fix it.

**The four caches:**

| Cache | What it stores | Lifetime | Where it lives |
|---|---|---|---|
| **Request Memoization** | `fetch` responses within a single render pass | Per-request (single render) | Server memory |
| **Data Cache** | `fetch` responses across requests and deployments | Persistent (until revalidated) | Server filesystem / CDN |
| **Full Route Cache** | Pre-rendered HTML + RSC payload for static routes | Persistent (until revalidated or redeployed) | Server / CDN |
| **Router Cache** | RSC payload for visited routes | Session (browser tab lifetime) | Client memory |

---

## 183. Project Setup

**What is this:** Bootstrapping the caching demo project provided for this section.

**Description:** A new starter project is provided — a simple app that makes `fetch` calls to an external API endpoint. The project is intentionally minimal so caching behavior is easy to observe without other complexity getting in the way.

---

## 184. Handling Request Memoization

**What is this:** How NextJS automatically deduplicates identical `fetch` calls within a single render pass.

**Description:** If multiple Server Components call `fetch` with the same URL and options during the same request, NextJS only makes the network call once and reuses the result for all of them. This is **Request Memoization** — it's automatic, scoped to a single render, and requires no configuration. It only applies to `fetch` (not DB calls or other async operations).

**Examples:**

```jsx
// Component A — fetches /api/messages
async function ComponentA() {
  const res = await fetch('http://localhost:8080/messages');
  const data = await res.json();
  return <p>Component A: {data.messages.length} messages</p>;
}

// Component B — same fetch URL
async function ComponentB() {
  const res = await fetch('http://localhost:8080/messages'); // NOT a second network call
  const data = await res.json();
  return <p>Component B loaded</p>;
}

// app/page.js — renders both
export default function Page() {
  return (
    <>
      <ComponentA />
      <ComponentB />
    </>
  );
}
// NextJS makes ONE network request — both components share the memoized result
```

---

## 185. Understanding The Data Cache & Cache Settings

**What is this:** How the Data Cache stores `fetch` results persistently across requests and how to configure its behavior.

**Description:** The Data Cache persists `fetch` responses on the server beyond a single request — even across deployments until explicitly invalidated. By default `fetch` is cached (`force-cache`). You control it with the `cache` option or `next.revalidate`.

**Cache options:**

| Option | Behavior |
|---|---|
| `cache: 'force-cache'` | Default — cache indefinitely until revalidated |
| `cache: 'no-store'` | Never cache — always fetch fresh |
| `next: { revalidate: N }` | Cache for N seconds (ISR-style), then refresh |
| `next: { tags: ['tag'] }` | Cache until `revalidateTag('tag')` is called |

**Examples:**

```js
// Cached forever (default) — fastest, may serve stale data
const res = await fetch('http://localhost:8080/messages');

// Never cached — always fresh, slowest
const res = await fetch('http://localhost:8080/messages', {
  cache: 'no-store',
});

// Time-based revalidation — fresh after 60 seconds
const res = await fetch('http://localhost:8080/messages', {
  next: { revalidate: 60 },
});

// Tag-based — stays cached until revalidateTag('messages') is called
const res = await fetch('http://localhost:8080/messages', {
  next: { tags: ['messages'] },
});
```

---

## 186. Controlling Data Caching

**What is this:** Opting individual routes or entire route segments out of caching using segment-level config exports.

**Description:** Instead of setting `cache` on each `fetch`, you can configure caching for an entire route segment with exported constants. `dynamic = 'force-dynamic'` makes the whole page dynamic (no caching). `revalidate = N` sets a time-based revalidation for all fetches in the segment.

**Examples:**

```js
// app/messages/page.js

// Option A: force the entire page to be dynamic — no caching at all
export const dynamic = 'force-dynamic';

// Option B: revalidate all fetches in this segment every 5 seconds
export const revalidate = 5;

export default async function MessagesPage() {
  const res = await fetch('http://localhost:8080/messages');
  // inherits the segment's cache setting — no need for per-fetch options
  const data = await res.json();
  return <ul>{data.messages.map(m => <li key={m.id}>{m.text}</li>)}</ul>;
}
```

```js
// Other segment config options:
export const dynamic = 'auto';           // default — NextJS decides
export const dynamic = 'force-dynamic'; // always dynamic, never cached
export const dynamic = 'force-static';  // always static, errors if dynamic data used
export const dynamic = 'error';         // static, throw if dynamic data is used
```

---

## 187. Making Sense Of The Full Route Cache

**What is this:** How NextJS pre-renders and caches entire route HTML at build time for static routes.

**Description:** The Full Route Cache stores the complete rendered HTML and React Server Component (RSC) payload for routes that NextJS can determine are static at build time. These are served instantly from the CDN without hitting the server. A route becomes dynamic (and skips this cache) if it uses `cookies()`, `headers()`, `searchParams`, `dynamic = 'force-dynamic'`, or `cache: 'no-store'` fetches.

**Examples:**

```bash
# Build output shows which routes are static vs dynamic
npm run build

# Output:
○  /               → static  (pre-rendered, served from Full Route Cache)
●  /messages       → static  (data fetched at build, cached)
ƒ  /messages/new   → dynamic (uses cookies/headers/searchParams)
```

```js
// Making a route dynamic — removes it from the Full Route Cache
export const dynamic = 'force-dynamic';

// Or use a dynamic data source that NextJS detects automatically:
import { cookies } from 'next/headers';
export default function Page() {
  const token = cookies().get('token'); // NextJS sees this → marks route dynamic
  return <div />;
}
```

---

## 188. On-Demand Cache Invalidation with revalidatePath & revalidateTag

**What is this:** Manually busting the Data Cache and Full Route Cache from a Server Action or Route Handler.

**Description:** `revalidatePath()` invalidates the Full Route Cache and Data Cache for a specific path. `revalidateTag()` invalidates all `fetch` calls tagged with a given string. Both are called on-demand — typically after a mutation — and trigger regeneration on the next request.

**Examples:**

```js
// lib/actions.js
'use server';

import { revalidatePath, revalidateTag } from 'next/cache';

export async function addMessage(formData) {
  const text = formData.get('message');
  // ... save to DB or call API ...

  // Option 1: invalidate a specific path
  revalidatePath('/messages');

  // Option 2: invalidate all fetches with a tag
  revalidateTag('messages');
}
```

```js
// Tagging a fetch so revalidateTag can target it
const res = await fetch('http://localhost:8080/messages', {
  next: { tags: ['messages'] },
});

// revalidateTag('messages') will clear this fetch from the Data Cache
```

---

## 189. Setting Up Request Memoization For Custom Data Sources

**What is this:** Manually implementing request memoization for DB calls or other non-`fetch` data sources using React's `cache()`.

**Description:** NextJS's built-in request memoization only works for `fetch`. If you query a database directly (e.g., `better-sqlite3`), the same query can run multiple times per render if multiple components call it. React's `cache()` function wraps any async function and memoizes it per-request — the same arguments produce the same result within a single render pass.

**Examples:**

```js
// lib/messages.js
import { cache } from 'react';
import sql from 'better-sqlite3';

const db = sql('messages.db');

// Without cache() — runs a DB query every time it's called
export function getMessages() {
  return db.prepare('SELECT * FROM messages').all();
}

// With cache() — DB is only queried once per request, result reused
export const getMessages = cache(function getMessages() {
  return db.prepare('SELECT * FROM messages').all();
});
```

```jsx
// Now these two components both call getMessages() but only one DB query runs
async function ComponentA() {
  const messages = getMessages(); // DB query runs here
  return <p>{messages.length} messages</p>;
}

async function ComponentB() {
  const messages = getMessages(); // returns memoized result — no second query
  return <p>Component B</p>;
}
```

---

## 190. Setting Up Data Caching For Custom Data Sources

**What is this:** Using `unstable_cache` to persist DB query results across requests — the Data Cache equivalent for non-`fetch` sources.

**Description:** `fetch` has built-in Data Cache support. For direct DB calls, use `unstable_cache` from `next/cache`. It wraps a function and caches its return value on the server across requests, with the same `revalidate` and `tags` options available to `fetch`.

**Examples:**

```js
// lib/messages.js
import { unstable_cache } from 'next/cache';
import sql from 'better-sqlite3';

const db = sql('messages.db');

// Wrap the DB call — result is cached across requests
export const getMessages = unstable_cache(
  function () {
    return db.prepare('SELECT * FROM messages').all();
  },
  ['messages'],         // cache key — must be unique per function
  {
    revalidate: 60,     // revalidate every 60 seconds
    tags: ['messages'], // allows revalidateTag('messages') to clear it
  }
);
```

```js
// Usage is identical to the non-cached version
const messages = await getMessages();
// First call hits the DB; subsequent calls within the revalidation window
// return the cached result without touching the DB
```

---

## 191. Invalidating Custom Data Source Data

**What is this:** Using `revalidateTag()` to clear the `unstable_cache` entries for custom data sources after a mutation.

**Description:** Because `unstable_cache` supports `tags`, you can invalidate cached DB results the same way you invalidate cached `fetch` results — by calling `revalidateTag()` with the matching tag. This ensures stale data is cleared after a write.

**Examples:**

```js
// lib/actions.js
'use server';

import { revalidateTag } from 'next/cache';
import sql from 'better-sqlite3';

const db = sql('messages.db');

export async function addMessage(prevState, formData) {
  const text = formData.get('message');

  db.prepare('INSERT INTO messages (text) VALUES (?)').run(text);

  revalidateTag('messages'); // clears the unstable_cache entry tagged 'messages'
  // next call to getMessages() will re-run the DB query and cache the fresh result
}
```

```js
// Full flow:
// 1. getMessages() is wrapped in unstable_cache with tag 'messages'
// 2. First request → DB query runs, result cached
// 3. User submits a new message → addMessage() Server Action runs
// 4. revalidateTag('messages') clears the cache
// 5. Next request to any page calling getMessages() → DB query runs again, fresh data
```

---

# Section 8: NextJS App Optimizations

---

## 193. Module Introduction

**What is this:** An overview of the two main optimization topics in this section — the `<Image>` component and page metadata.

**Description:** This section covers how to get the most out of NextJS's built-in `<Image>` component (sizing, priority, fill, remote images, custom loaders, Cloudinary) and how to define static and dynamic metadata for SEO.

---

## 194. Using the NextJS Image Component

**What is this:** Replacing `<img>` with Next's `<Image>` for automatic optimization.

**Description:** The `<Image>` component from `next/image` wraps a native `<img>` but adds: lazy loading by default, automatic WebP conversion, prevention of Cumulative Layout Shift (CLS), and responsive sizing. For local imports, `width` and `height` are inferred automatically.

**Examples:**

```jsx
// app/page.js
import Image from 'next/image';
import heroImg from '@/assets/hero.jpg'; // local import — size inferred

export default function HomePage() {
  return (
    <main>
      <Image src={heroImg} alt="Hero image" />
      {/* lazy loaded, WebP served, no layout shift */}
    </main>
  );
}
```

---

## 195. Understanding the NextJS Image Component

**What is this:** How `<Image>` works under the hood — what it generates, how optimization is applied, and what the browser receives.

**Description:** NextJS intercepts image requests through a built-in image optimization server (`/_next/image`). It resizes, converts to WebP, and serves the correct size based on the device. The original file is never sent as-is unless you opt out. In the DOM, `<Image>` renders as a regular `<img>` with `srcset` and `sizes` attributes generated automatically.

**Examples:**

```html
<!-- What NextJS renders in the browser for a local image -->
<img
  src="/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fhero.abc123.jpg&w=1080&q=75"
  srcset="
    /_next/image?url=...&w=640&q=75  640w,
    /_next/image?url=...&w=1080&q=75 1080w
  "
  sizes="100vw"
  loading="lazy"
  decoding="async"
  width="1920"
  height="1080"
/>
```

```jsx
// quality prop — default is 75 (good balance of size vs quality)
<Image src={heroImg} alt="Hero" quality={90} />
```

---

## 196. Controlling the Image Size

**What is this:** Setting explicit `width` and `height` on `<Image>` for remote or dynamically sourced images.

**Description:** Local imports have their dimensions inferred at build time. For remote URLs or images whose source is determined at runtime, you must provide `width` and `height` (in pixels). These don't crop the image — they tell the browser how much space to reserve, preventing layout shift.

**Examples:**

```jsx
import Image from 'next/image';

// Remote image — width and height are required
export default function MealItem({ meal }) {
  return (
    <Image
      src={meal.image}   // e.g. "https://cdn.example.com/burger.jpg"
      alt={meal.title}
      width={400}
      height={300}
    />
  );
}
```

```jsx
// You can still control the rendered display size with CSS
// width/height just reserve layout space — CSS controls the visual size
<Image
  src={meal.image}
  alt={meal.title}
  width={400}
  height={300}
  style={{ width: '100%', height: 'auto' }} // responsive display
/>
```

---

## 197. Working with Priority Images & More Settings

**What is this:** Using the `priority` prop for above-the-fold images and other useful `<Image>` settings.

**Description:** By default `<Image>` lazy loads. For hero images and anything visible on first paint, add `priority` — this disables lazy loading and adds `<link rel="preload">` to the `<head>`, improving LCP (Largest Contentful Paint). Other useful props: `placeholder`, `blurDataURL`, `sizes`.

**Examples:**

```jsx
// priority — preloads the image, no lazy loading
<Image
  src={heroImg}
  alt="Hero"
  priority
/>
```

```jsx
// placeholder="blur" — shows a blurred version while loading
// blurDataURL is auto-generated for local images
<Image
  src={heroImg}
  alt="Hero"
  placeholder="blur"
/>

// For remote images you must provide blurDataURL manually (a tiny base64 image)
<Image
  src="https://example.com/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  placeholder="blur"
  blurDataURL="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg=="
/>
```

```jsx
// sizes — helps browser pick the right srcset entry for the current viewport
<Image
  src={heroImg}
  alt="Hero"
  sizes="(max-width: 768px) 100vw, 50vw"
/>
```

---

## 198. Loading Unknown Images

**What is this:** Configuring `next.config.js` to allow `<Image>` to optimize images from external domains.

**Description:** By default NextJS only optimizes images from allowed sources for security. To use `<Image>` with a remote URL, you must whitelist the hostname in `next.config.js` via `remotePatterns`. Without this, NextJS throws an error at runtime.

**Examples:**

```js
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'res.cloudinary.com',
        // port and pathname are optional — omit to allow all paths
      },
      {
        protocol: 'https',
        hostname: '**.example.com', // ** wildcard matches any subdomain
      },
    ],
  },
};

module.exports = nextConfig;
```

```jsx
// Now this works without errors
<Image
  src="https://res.cloudinary.com/demo/image/upload/sample.jpg"
  alt="Cloudinary image"
  width={800}
  height={600}
/>
```

---

## 199. Configuring CSS For Images With The "fill" Prop

**What is this:** Using `fill` on `<Image>` to make it stretch to fill its parent container when dimensions are unknown.

**Description:** When you don't know the image's intrinsic dimensions (e.g., user-uploaded content), use the `fill` prop instead of `width`/`height`. The image stretches to fill its nearest positioned parent. The parent must have `position: relative` (or `absolute`/`fixed`) and an explicit size.

**Examples:**

```jsx
// components/meal-item.js
import Image from 'next/image';
import classes from './meal-item.module.css';

export default function MealItem({ meal }) {
  return (
    <article>
      <div className={classes.imageContainer}>
        <Image
          src={meal.image}
          alt={meal.title}
          fill
          sizes="(max-width: 768px) 100vw, 400px"
        />
      </div>
    </article>
  );
}
```

```css
/* meal-item.module.css */
.imageContainer {
  position: relative;  /* required for fill */
  width: 100%;
  height: 200px;       /* explicit height required */
  overflow: hidden;
}

/* object-fit controls how the image fits inside the container */
.imageContainer img {
  object-fit: cover;   /* crop to fill — no distortion */
}
```

---

## 200. Using An Image Loader & Cloudinary Resizing

**What is this:** Replacing NextJS's built-in image optimization server with a custom loader that delegates to a third-party service like Cloudinary.

**Description:** By default `<Image>` uses NextJS's own optimization pipeline (`/_next/image`). A custom `loader` function intercepts the image URL and returns a transformed URL — typically pointing to a CDN like Cloudinary which handles resizing and format conversion on their servers. This offloads optimization from your server.

**Examples:**

```js
// lib/cloudinary-loader.js
export default function cloudinaryLoader({ src, width, quality }) {
  // src is the public ID or full Cloudinary URL
  const params = ['f_auto', 'c_limit', `w_${width}`, `q_${quality || 'auto'}`];
  return `https://res.cloudinary.com/YOUR_CLOUD_NAME/image/upload/${params.join(',')}/${src}`;
}
```

```jsx
// components/meal-item.js
import Image from 'next/image';
import cloudinaryLoader from '@/lib/cloudinary-loader';

export default function MealItem({ meal }) {
  return (
    <Image
      loader={cloudinaryLoader}
      src={meal.cloudinaryPublicId}  // e.g. "meals/burger"
      alt={meal.title}
      width={400}
      height={300}
    />
    // NextJS calls cloudinaryLoader({ src, width, quality })
    // and uses the returned Cloudinary URL as the actual image src
  );
}
```

```js
// Or configure a global loader in next.config.js (applies to all <Image> usage)
const nextConfig = {
  images: {
    loader: 'custom',
    loaderFile: './lib/cloudinary-loader.js',
  },
};
```

---

## 201. Page Metadata — An Introduction

**What is this:** An overview of how NextJS handles `<head>` metadata and why it matters for SEO and social sharing.

**Description:** Metadata (page title, description, Open Graph tags, etc.) lives in the `<head>` of the HTML document. In the App Router, you never write `<head>` manually — instead you export a `metadata` object or a `generateMetadata` function from any `page.js` or `layout.js`. NextJS merges and injects the tags automatically.

**Key metadata fields:**

| Field | HTML output | Purpose |
|---|---|---|
| `title` | `<title>` | Browser tab, search result headline |
| `description` | `<meta name="description">` | Search result snippet |
| `openGraph.title` | `<meta property="og:title">` | Social share preview title |
| `openGraph.images` | `<meta property="og:image">` | Social share thumbnail |
| `robots` | `<meta name="robots">` | Search engine crawl instructions |

---

## 202. Configuring Static Page Metadata

**What is this:** Exporting a `metadata` object from a `page.js` or `layout.js` to set fixed, build-time metadata.

**Description:** Static metadata is defined by exporting a `metadata` constant. It's merged with any parent layout's metadata — child values override parent values for the same field. The root layout's metadata acts as the site-wide default.

**Examples:**

```js
// app/layout.js — root metadata, applies to all pages as defaults
export const metadata = {
  title: {
    template: '%s | Foodies',  // %s is replaced by each page's title
    default: 'Foodies',        // used if a page has no title
  },
  description: 'Delicious meals, shared by a food-loving community.',
};
```

```js
// app/meals/page.js — overrides just the title
export const metadata = {
  title: 'Browse Meals',
  // description falls back to the root layout's value
};
// Final <title>: "Browse Meals | Foodies"
```

```js
// Full metadata example with Open Graph
export const metadata = {
  title: 'Browse Meals',
  description: 'Find and share amazing meals from around the world.',
  openGraph: {
    title: 'Browse Meals on Foodies',
    description: 'Find and share amazing meals.',
    images: [{ url: 'https://example.com/og-image.jpg' }],
  },
};
```

---

## 203. Configuring Dynamic Page Metadata

**What is this:** Using `generateMetadata` to produce metadata at request time for dynamic routes.

**Description:** For routes like `/meals/[slug]`, the title and description depend on the actual data. Export an async `generateMetadata` function instead of a static `metadata` object — it receives `params` and `searchParams`, can fetch data, and returns a metadata object. NextJS calls it before rendering the page.

**Examples:**

```js
// app/meals/[mealSlug]/page.js
import { getMeal } from '@/lib/meals';
import { notFound } from 'next/navigation';

export async function generateMetadata({ params }) {
  const meal = getMeal(params.mealSlug);

  if (!meal) notFound();

  return {
    title: meal.title,
    description: meal.summary,
    openGraph: {
      title: meal.title,
      description: meal.summary,
      images: meal.image ? [{ url: meal.image }] : [],
    },
  };
}

export default function MealDetailPage({ params }) {
  const meal = getMeal(params.mealSlug);
  if (!meal) notFound();
  return <article>{meal.title}</article>;
}

// NextJS deduplicates data fetching — if both generateMetadata and the page
// call getMeal() with the same args, the result is memoized (one DB query)
```

---

## 204. Understanding Layout Metadata

**What is this:** How metadata defined in `layout.js` is inherited and merged across the route hierarchy.

**Description:** Every `layout.js` and `page.js` can export metadata. NextJS merges them from root to leaf — parent values are the defaults, child values override on a per-field basis (not a deep merge of objects — each top-level field is replaced, not merged). The `title.template` pattern is the main exception: it applies the parent's template string around the child's title.

**Examples:**

```
Metadata merge order:
  app/layout.js        → { title: { template: '%s | Foodies', default: 'Foodies' }, description: '...' }
  app/meals/layout.js  → { description: 'Browse our meals.' }  ← overrides description
  app/meals/page.js    → { title: 'All Meals' }                ← fills %s in template

Final <head> for /meals:
  <title>All Meals | Foodies</title>
  <meta name="description" content="Browse our meals." />
```

```js
// app/meals/layout.js — nested layout with its own metadata
export const metadata = {
  description: 'Browse amazing meals shared by our community.',
  // title not set here — inherits the root layout's template
};

export default function MealsLayout({ children }) {
  return <>{children}</>;
}
```

```js
// Fields are replaced, not deep-merged:
// Root:  { openGraph: { title: 'Foodies', images: [...] } }
// Page:  { openGraph: { title: 'Burger' } }
// Final: { openGraph: { title: 'Burger' } }  ← images is GONE
// Fix: always re-specify all openGraph fields in child pages if root has some
```

---

# Section 11: Pages & File-based Routing (Pages Router)

---

## 226. From App Router To Pages Router

**What is this:** A context switch — this section covers the older **Pages Router** (`pages/` directory), not the App Router covered in previous sections.

**Description:** The Pages Router is the original NextJS routing system. It predates the App Router and is still widely used in production codebases. All components are Client Components by default, data fetching uses special exported functions (`getServerSideProps`, `getStaticProps`), and there is no `layout.js` concept — layouts are handled manually. Understanding it is essential for working with legacy NextJS projects.

**Key differences from the App Router:**

| | App Router (`app/`) | Pages Router (`pages/`) |
|---|---|---|
| Default rendering | Server Components | Client Components |
| Data fetching | `async` components + `fetch` | `getServerSideProps` / `getStaticProps` |
| Layouts | `layout.js` per segment | Manual `_app.js` wrapper |
| `<head>` | `metadata` export | `next/head` `<Head>` component |
| Status | Recommended (Next 13+) | Stable, legacy |

---

## 227. Using The Code Snapshots

**What is this:** How to use the provided code snapshots to catch up if you fall behind during the exercises.

**Description:** The instructor provides a folder of snapshots — one per lesson or exercise — so you can always start from a known good state. No new concepts here.

---

## 228. Module Introduction

**What is this:** An overview of what this section covers — Pages Router routing, navigation, dynamic routes, and catch-all routes.

**Description:** This module teaches the Pages Router from scratch: how file paths map to routes, static and dynamic route files, nested routes, catch-all segments, the `<Link>` component, programmatic navigation, and custom 404 pages.

---

## 229. Our Starting Setup

**What is this:** Reviewing the blank Pages Router project that will be built up through the section.

**Description:** The starter is a minimal NextJS project with the `pages/` directory and no routes defined yet. The `pages/_app.js` wrapper is the entry point for all pages — it's where you'd add global styles and persistent layout elements.

**Examples:**

```
pages/
├── _app.js     ← wraps all pages (equivalent to root layout.js in App Router)
└── index.js    ← homepage → /
```

```jsx
// pages/_app.js — global wrapper
import '@/styles/globals.css';

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
  // Component = the active page component
  // pageProps = data passed from getServerSideProps / getStaticProps
}
```

---

## 230. What Is "File-based Routing"? And Why Is It Helpful?

**What is this:** The core concept — every file in `pages/` automatically becomes a route, no router config needed.

**Description:** In the Pages Router, the file system IS the router. A file at `pages/about.js` becomes `/about`. A file at `pages/blog/first-post.js` becomes `/blog/first-post`. No `<Route>` components, no `createBrowserRouter`, no config — just files.

**Examples:**

```
pages/
├── index.js          →  /
├── about.js          →  /about
├── blog/
│   ├── index.js      →  /blog
│   └── first-post.js →  /blog/first-post
└── contact.js        →  /contact
```

```jsx
// pages/about.js — automatically becomes the /about route
export default function AboutPage() {
  return (
    <main>
      <h1>About Us</h1>
      <p>We build great software.</p>
    </main>
  );
}
```

---

## 231. Adding A First Page

**What is this:** Creating the homepage by adding `pages/index.js`.

**Description:** `pages/index.js` maps to the root route `/`. It exports a default React component — that's all that's required. No imports from NextJS are needed for a basic page.

**Examples:**

```jsx
// pages/index.js → route: /
export default function HomePage() {
  return (
    <main>
      <h1>Welcome to My NextJS App!</h1>
      <p>This is the homepage.</p>
    </main>
  );
}
```

---

## 232. Adding a Named / Static Route File

**What is this:** Creating a route for a specific URL by adding a named file to `pages/`.

**Description:** Any `.js` file in `pages/` (other than `_app.js`, `_document.js`) becomes a route. The filename (without extension) becomes the URL path segment.

**Examples:**

```jsx
// pages/news.js → route: /news
export default function NewsPage() {
  return (
    <main>
      <h1>Latest News</h1>
    </main>
  );
}
```

```jsx
// pages/portfolio.js → route: /portfolio
export default function PortfolioPage() {
  return (
    <main>
      <h1>My Portfolio</h1>
    </main>
  );
}
```

---

## 233. Working with Nested Paths & Routes

**What is this:** Creating sub-routes by nesting files inside folders within `pages/`.

**Description:** To create `/blog/first-post`, create `pages/blog/first-post.js`. To create the `/blog` index route, create `pages/blog/index.js`. The folder name becomes the URL segment and the filename becomes the sub-segment.

**Examples:**

```
pages/
└── portfolio/
    ├── index.js          →  /portfolio
    └── my-first-project.js → /portfolio/my-first-project
```

```jsx
// pages/portfolio/index.js → /portfolio
export default function PortfolioPage() {
  return (
    <main>
      <h1>My Portfolio</h1>
      <ul>
        <li>Project 1</li>
        <li>Project 2</li>
      </ul>
    </main>
  );
}
```

```jsx
// pages/portfolio/my-first-project.js → /portfolio/my-first-project
export default function FirstProjectPage() {
  return (
    <main>
      <h1>My First Project</h1>
    </main>
  );
}
```

---

## 234. Adding Dynamic Paths & Routes

**What is this:** Creating routes with variable URL segments using the `[param]` filename convention.

**Description:** Wrapping a filename (or folder name) in square brackets creates a dynamic route. The bracket content is the name of the parameter. `pages/portfolio/[projectId].js` matches `/portfolio/anything` and captures `anything` as `projectId`.

**Examples:**

```
pages/
└── portfolio/
    ├── index.js          →  /portfolio
    └── [projectId].js    →  /portfolio/:projectId
```

```jsx
// pages/portfolio/[projectId].js
// matches /portfolio/react-app, /portfolio/42, /portfolio/my-site, etc.
export default function PortfolioProjectPage() {
  return (
    <main>
      <h1>Project Detail</h1>
    </main>
  );
}
```

---

## 235. Extracting Dynamic Path Segment Data (Dynamic Routes)

**What is this:** Reading the dynamic URL parameter inside a Page component using `useRouter`.

**Description:** The `useRouter` hook from `next/router` gives access to the current route's `query` object, which contains all dynamic parameters. For `[projectId].js`, `router.query.projectId` holds the actual URL value.

**Examples:**

```jsx
// pages/portfolio/[projectId].js
import { useRouter } from 'next/router';

export default function PortfolioProjectPage() {
  const router = useRouter();
  const { projectId } = router.query;
  // visiting /portfolio/react-app → projectId = 'react-app'

  return (
    <main>
      <h1>Project: {projectId}</h1>
    </main>
  );
}
```

---

## 236. Building Nested Dynamic Routes & Paths

**What is this:** Combining nested folders with dynamic segments to create multi-level dynamic routes.

**Description:** You can nest dynamic segments — a folder named `[clientId]` containing a file named `[projectId].js` creates a route like `/clients/:clientId/:projectId`. Both parameters are available in `router.query`.

**Examples:**

```
pages/
└── clients/
    ├── index.js                   →  /clients
    └── [clientId]/
        ├── index.js               →  /clients/:clientId
        └── [projectId].js         →  /clients/:clientId/:projectId
```

```jsx
// pages/clients/[clientId]/[projectId].js
import { useRouter } from 'next/router';

export default function ClientProjectPage() {
  const router = useRouter();
  const { clientId, projectId } = router.query;
  // /clients/john/react-app → { clientId: 'john', projectId: 'react-app' }

  return (
    <main>
      <h1>Client: {clientId}</h1>
      <h2>Project: {projectId}</h2>
    </main>
  );
}
```

---

## 237. Adding Catch-All Routes

**What is this:** Using `[...slug].js` to match any number of path segments with a single route file.

**Description:** A catch-all route `[...slug]` captures all remaining path segments into an array. `pages/blog/[...slug].js` matches `/blog/2024`, `/blog/2024/05`, `/blog/2024/05/hello-world`, etc. The captured segments are available as an array in `router.query.slug`.

**Examples:**

```
pages/
└── blog/
    └── [...slug].js    → /blog/*, /blog/*/*, /blog/*/*/*, etc.
```

```jsx
// pages/blog/[...slug].js
import { useRouter } from 'next/router';

export default function BlogPage() {
  const router = useRouter();
  const { slug } = router.query;
  // /blog/2024/05/hello → slug = ['2024', '05', 'hello']
  // /blog/2024          → slug = ['2024']

  return (
    <main>
      <h1>Blog</h1>
      <p>Segments: {slug ? slug.join(' / ') : 'loading...'}</p>
    </main>
  );
}
```

```
// Optional catch-all — also matches the base route
// pages/blog/[[...slug]].js → matches /blog AND /blog/2024/05/hello
// slug = undefined when visiting /blog with no segments
```

---

## 238. Navigating with the "Link" Component

**What is this:** Using `<Link>` from `next/link` for client-side navigation between pages.

**Description:** Same concept as in the App Router — `<Link>` prevents full page reloads and keeps the SPA experience. In the Pages Router, `<Link>` works identically: import from `next/link`, pass an `href` string, wrap whatever content you like.

**Examples:**

```jsx
// pages/index.js
import Link from 'next/link';

export default function HomePage() {
  return (
    <main>
      <h1>Welcome</h1>
      <nav>
        <ul>
          <li><Link href="/portfolio">Portfolio</Link></li>
          <li><Link href="/clients">Clients</Link></li>
          <li><Link href="/blog">Blog</Link></li>
        </ul>
      </nav>
    </main>
  );
}
```

---

## 239. Navigating To Dynamic Routes

**What is this:** Building `href` strings for dynamic routes when linking to a specific parameter value.

**Description:** For dynamic routes, construct the `href` string with the actual value interpolated. For `/portfolio/[projectId]`, the link's `href` would be `/portfolio/react-app` (the real value, not the bracket syntax).

**Examples:**

```jsx
// pages/portfolio/index.js — linking to individual project pages
import Link from 'next/link';

const projects = [
  { id: 'react-app', title: 'React App' },
  { id: 'nextjs-site', title: 'NextJS Site' },
];

export default function PortfolioPage() {
  return (
    <main>
      <h1>Portfolio</h1>
      <ul>
        {projects.map((project) => (
          <li key={project.id}>
            <Link href={`/portfolio/${project.id}`}>
              {project.title}
            </Link>
          </li>
        ))}
      </ul>
    </main>
  );
}
```

---

## 240. A Different Way Of Setting Link Hrefs

**What is this:** Using an object as the `href` prop on `<Link>` instead of a plain string.

**Description:** `<Link>` accepts an object with `pathname` and `query` fields. This is useful for dynamic routes — you pass the route pattern as `pathname` and the parameter values as `query`. NextJS builds the final URL for you. It's more explicit and easier to maintain than string interpolation.

**Examples:**

```jsx
// String href — works, but manual string building
<Link href={`/clients/${client.id}/${project.id}`}>
  View Project
</Link>

// Object href — more explicit, NextJS builds the URL
<Link
  href={{
    pathname: '/clients/[clientId]/[projectId]',
    query: { clientId: client.id, projectId: project.id },
  }}
>
  View Project
</Link>
// Both produce the same final URL: /clients/john/react-app
```

---

## 241. Navigating Programmatically

**What is this:** Using `router.push()` to navigate without a `<Link>` component — from event handlers or after async operations.

**Description:** `useRouter` from `next/router` provides `push()`, `replace()`, and `back()` for imperative navigation. This is how you navigate after form submissions, button clicks that aren't anchor tags, or any non-link trigger.

**Examples:**

```jsx
// pages/clients/index.js
import { useRouter } from 'next/router';

export default function ClientsPage() {
  const router = useRouter();

  function loadClientHandler(clientId) {
    // navigate after some logic (e.g. after an API call)
    router.push(`/clients/${clientId}`);

    // Or with object form:
    router.push({
      pathname: '/clients/[clientId]',
      query: { clientId },
    });
  }

  return (
    <main>
      <h1>Clients</h1>
      <button onClick={() => loadClientHandler('max')}>
        Load Max's Portfolio
      </button>
    </main>
  );
}
```

```js
router.push('/about');       // navigate, add to history
router.replace('/about');    // navigate, replace current history entry
router.back();               // go back one step
```

---

## 242. Adding a Custom 404 Page

**What is this:** Creating a custom 404 page by adding `pages/404.js`.

**Description:** NextJS automatically serves `pages/404.js` for any URL that doesn't match a known route. This is a special reserved filename — you just create the file and export a component. No configuration needed.

**Examples:**

```jsx
// pages/404.js — shown for any unmatched URL
export default function NotFoundPage() {
  return (
    <main>
      <h1>Page Not Found</h1>
      <p>The page you are looking for does not exist.</p>
    </main>
  );
}
```

---

## 243. Module Summary

**What is this:** A recap of everything covered in Section 11 — the Pages Router fundamentals.

**Description:** This lesson consolidates the key concepts before moving on. No new code.

**What was covered:**
- The Pages Router uses the `pages/` directory — file paths map directly to URL routes
- `pages/index.js` → `/`, `pages/about.js` → `/about`, `pages/blog/index.js` → `/blog`
- Dynamic routes use bracket filenames: `[param].js` captures one segment, `[...slug].js` catches all
- `useRouter` from `next/router` gives access to `router.query` for dynamic params
- Nested dynamic routes: folder `[clientId]/` + file `[projectId].js` → `/clients/:clientId/:projectId`
- `<Link href="...">` for client-side navigation (no full page reload)
- Object-form `href={{ pathname, query }}` as a cleaner alternative to string interpolation
- `router.push()` / `router.replace()` for programmatic navigation
- `pages/404.js` for a custom not-found page
- `pages/_app.js` wraps all pages (global layout, global styles)
