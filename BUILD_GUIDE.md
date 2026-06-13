# Comprehensive Next.js 15 Boilerplate Build Guide

## Complete Step-by-Step Process with Technical Depth

This document provides a professional-grade, detailed guide to building with this modern Next.js 15 boilerplate—designed to impress technical judges with architectural understanding and implementation best practices.

---

## Phase 1: Environment Setup & Project Initialization

### Step 1.1: Prerequisites Installation

#### Node.js Runtime Setup
- **Requirement**: Node.js v18 LTS or higher
- **Why**: Provides modern JavaScript runtime with built-in support for ES2024 features
- **Installation**:
  ```bash
  # Download from nodejs.org or use nvm (recommended)
  nvm install 18
  nvm use 18
  node --version  # Verify v18+
  ```

#### Package Manager Configuration
- **Primary**: npm v10+ (bundled with Node.js)
- **Alternatives**: yarn, pnpm, or bun (all compatible)
- **Verify**: `npm --version`

#### Git Version Control
- **Requirement**: Git 2.30+
- **Purpose**: Version tracking, collaboration, CI/CD integration
- **Initialize**: `git init` (if not already cloned)

#### Code Editor Setup
- **Recommended**: Visual Studio Code
- **Essential Extensions**:
  - "ES7+ React/Redux/React-Native snippets"
  - "TypeScript Vue Plugin"
  - "Tailwind CSS IntelliSense"
  - "Next.js Official" (Vercel)
  - "Thunder Client" or "REST Client" (for API testing)

### Step 1.2: Project Setup and Dependency Installation

```bash
# 1. Clone repository (if not already done)
git clone <repository-url>
cd nextjs-boilerplate

# 2. Install all dependencies
npm install

# 3. Verify installation
npm list
```

#### Installed Dependencies Breakdown

**Production Dependencies**:
- **React 19.0.0**
  - Latest React with concurrent rendering
  - Server Components (RSC) support
  - Automatic batching for better performance
  - Built-in hooks optimizations

- **React-DOM 19.0.0**
  - React rendering engine for web
  - Hydration for Server-Side Rendering (SSR)
  - Streaming response support

- **Next.js 15.3.5**
  - App Router (not Pages Router) for modern file-based routing
  - Built-in TypeScript support
  - Automatic code splitting per route
  - Middleware execution layer
  - API Routes as backend alternative
  - Image and Font optimization built-in

**Development Dependencies**:
- **TypeScript 5.x**
  - Strict type checking (recommended: `strict: true`)
  - Latest ECMAScript 2024 support
  - Improved type inference
  - Path aliases for cleaner imports

- **@types packages**
  - @types/node: Node.js API types for backend code
  - @types/react: React component and hook types
  - @types/react-dom: ReactDOM API types

- **Tailwind CSS 4**
  - Utility-first CSS framework
  - Just-In-Time (JIT) compilation
  - No CSS file size bloat
  - Built-in dark mode support
  - Mobile-first responsive design
  - 70+ CSS utilities for rapid prototyping

- **@tailwindcss/postcss 4**
  - Tailwind CSS v4 PostCSS plugin
  - Enables CSS variable customization
  - Custom selector support
  - Advanced responsive design features

- **PostCSS 4**
  - CSS transformation pipeline
  - Autoprefixing for browser compatibility
  - CSS nesting support
  - CSS variable processing
  - Future CSS features polyfilling

---

## Phase 2: Architecture & Technical Foundation

### Step 2.1: App Router Architecture (Next.js 15 Modern Pattern)

The project uses **App Router** instead of the deprecated Pages Router.

#### Directory Structure:
```
app/
├── layout.tsx              # Root layout (wraps all routes)
├── page.tsx                # Home page (/)
├── api/
│   └── [resource]/
│       └── route.ts        # API endpoint handlers
├── [dynamic]/
│   └── page.tsx            # Dynamic routes with parameters
└── [...slug]/
    └── page.tsx            # Catch-all routes
```

#### Key App Router Features:

**1. Server Components (Default)**
```typescript
// app/page.tsx - Automatically runs on server
export default function Page() {
  // Can fetch data directly
  // No JavaScript sent to browser
  return <h1>Server rendered HTML</h1>
}
```

**2. Nested Layouts with Automatic Code Splitting**
```typescript
// app/layout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>{children}</body>
    </html>
  )
}
```

**3. Dynamic Routes**
```typescript
// app/blog/[id]/page.tsx
export default function BlogPost({ params }: { params: { id: string } }) {
  return <h1>Post {params.id}</h1>
}

// Generates static pages at build time for known params
export async function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }]
}
```

**4. API Routes (Backend-like functionality)**
```typescript
// app/api/posts/route.ts
export async function GET(request: Request) {
  return Response.json({ posts: [] })
}

export async function POST(request: Request) {
  const body = await request.json()
  // Process data
  return Response.json({ success: true })
}
```

### Step 2.2: TypeScript Configuration

**tsconfig.json** configuration ensures:
```json
{
  "compilerOptions": {
    "target": "ES2020",           // Modern JavaScript target
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "preserve",             // Preserve JSX for Next.js
    "module": "ESNext",            // Modern module format
    "moduleResolution": "bundler", // Optimized for bundlers
    "strict": true,                // Enable all strict type checks
    "declaration": true,           // Generate .d.ts files
    "sourceMap": true,             // Enable debugging
    "forceConsistentCasingInFileNames": true,
    "paths": {
      "@/*": ["./*"]               // Absolute imports support
    }
  }
}
```

### Step 2.3: Build Optimization Pipeline

#### Turbopack Configuration
```bash
npm run dev -- --turbopack
```

**Why Turbopack**:
- **10-100x faster** than Webpack for development
- Written in Rust for near-native performance
- Incremental computation (only re-compiles changed files)
- Streaming response handling
- Automatic Hot Module Replacement (HMR)

#### Build Optimizations:
1. **Automatic Code Splitting**
   - One JavaScript bundle per route
   - Unused code never shipped to browser
   - Dynamic imports trigger separate chunks

2. **Static Site Generation (SSG)**
   - Pre-renders pages at build time
   - Serves as static HTML (fastest possible)
   - Generates .html files in .next/static

3. **Incremental Static Regeneration (ISR)**
   ```typescript
   export const revalidate = 60 // Revalidate every 60 seconds
   ```

4. **Server-Side Rendering (SSR)**
   - Dynamic routes rendered on-demand
   - Fresh data every request
   - Streaming HTML for faster perceived load time

5. **Image Optimization**
   - Automatic WebP format generation
   - Responsive image variants
   - Lazy loading by default
   - Serves from CDN with caching

6. **Font Optimization**
   - Geist font auto-loaded
   - Subset generation (only used characters)
   - No layout shift (font-display: swap)

---

## Phase 3: Development Environment Setup

### Step 3.1: Starting Development Server

```bash
npm run dev
```

**What this command does**:

1. **Launches Turbopack Compiler**
   - Monitors all files in `app/` directory
   - Watches `next.config.ts` and `tsconfig.json` changes
   - Compiles TypeScript → JavaScript

2. **Starts Dev Server**
   - Listens on http://localhost:3000
   - Supports hot module replacement
   - Real-time error overlay

3. **Enables Fast Refresh**
   - Edit React components → Instant update
   - Component state preserved during edits
   - No full page refresh required

4. **Creates Development Environment**
   ```
   .next/              # Build artifacts (gitignored)
   ├── server/         # Server-side bundles
   ├── static/         # Client-side bundles
   └── cache/          # Build cache (reusable)
   ```

### Step 3.2: Local Development Workflow

#### File Editing Example:
```typescript
// app/page.tsx
export default function Home() {
  const [count, setCount] = useState(0)
  
  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  )
}
```

**Development Flow**:
1. Edit file and save
2. Turbopack recompiles in milliseconds
3. Browser updates without page reload
4. Component state (count value) persists

#### Accessing Environment Variables:
```typescript
// app/page.tsx
const apiUrl = process.env.NEXT_PUBLIC_API_URL // Client-accessible
const secret = process.env.SECRET_KEY           // Server-only
```

#### Error Handling:
- **TypeScript Errors**: Shown in terminal and overlay
- **Runtime Errors**: Overlay with stack trace and code context
- **Styling Issues**: Tailwind validation errors displayed

---

## Phase 4: Frontend Development with React 19 & Tailwind CSS

### Step 4.1: React 19 Component Architecture

#### Server Components (Default)
```typescript
// app/users/page.tsx
import { db } from '@/lib/db'

export default async function UsersPage() {
  // This runs on the server only
  const users = await db.query('SELECT * FROM users')
  
  return (
    <div>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  )
}

// Tiny UserCard component (no JavaScript shipped)
function UserCard({ user }) {
  return <div>{user.name}</div>
}
```

#### Client Components (Interactive)
```typescript
'use client'

import { useState } from 'react'

export default function Counter() {
  // 'use client' directive makes this client-side
  const [count, setCount] = useState(0)
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  )
}
```

#### Mixing Server and Client
```typescript
// app/page.tsx (Server Component)
import Counter from '@/components/Counter'  // Client component
import UserList from '@/components/UserList' // Server component

export default async function Home() {
  const users = await fetchUsers()
  
  return (
    <div>
      <Counter />              {/* Interactivity */}
      <UserList users={users} /> {/* Pre-rendered data */}
    </div>
  )
}
```

### Step 4.2: Tailwind CSS Styling System

#### Utility-First CSS Approach
```typescript
// No custom CSS needed
export default function Card() {
  return (
    <div className="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition-shadow">
      <h2 className="text-xl font-bold text-gray-900">Title</h2>
      <p className="text-gray-600 mt-2">Description</p>
    </div>
  )
}
```

#### Responsive Design
```typescript
<div className="
  w-full md:w-1/2 lg:w-1/3
  text-sm md:text-base lg:text-lg
  grid-cols-1 md:grid-cols-2 lg:grid-cols-3
">
  Mobile-first responsive design
</div>
```

#### Dark Mode Support
```typescript
// tailwind.config.ts
export default {
  darkMode: 'class', // or 'media'
  theme: {
    extend: {},
  },
}

// Usage
<div className="bg-white dark:bg-gray-900 text-black dark:text-white">
  Adapts to dark mode
</div>
```

#### CSS Variables Integration
```typescript
// app/globals.css
@theme {
  --color-primary: #3b82f6;
  --color-secondary: #10b981;
}

// Usage
<div className="bg-[--color-primary]">Using CSS variables</div>
```

### Step 4.3: Performance Optimization Techniques

#### Image Optimization
```typescript
import Image from 'next/image'

export default function Hero() {
  return (
    <Image
      src="/hero.jpg"
      alt="Hero"
      width={1200}
      height={600}
      priority  // LCP optimization
      className="w-full h-auto"
    />
  )
}
```

#### Dynamic Imports (Code Splitting)
```typescript
import dynamic from 'next/dynamic'

const HeavyChart = dynamic(() => import('@/components/Chart'), {
  loading: () => <p>Loading chart...</p>,
})

export default function Dashboard() {
  return <HeavyChart /> // Loaded only when needed
}
```

#### Suspense Boundaries (Streaming)
```typescript
import { Suspense } from 'react'

function UserList() {
  // Suspends rendering until data is ready
  const users = use(fetchUsers())
  return users.map(u => <UserCard key={u.id} user={u} />)
}

export default function Page() {
  return (
    <Suspense fallback={<div>Loading users...</div>}>
      <UserList />
    </Suspense>
  )
}
```

---

## Phase 5: Building for Production

### Step 5.1: Production Build Process

```bash
npm run build
```

**Build Steps**:

1. **Type Checking**
   - TypeScript compiler runs in full mode
   - All type errors must be resolved
   - Fails if types are invalid

2. **Next.js Compilation**
   - Analyzes all routes in `app/` directory
   - Identifies static routes vs. dynamic routes
   - Pre-renders static pages to `.html`
   - Creates server functions for dynamic routes

3. **Dependency Analysis**
   - Identifies all imports
   - Tree-shaking unused exports
   - Dead code elimination

4. **Bundle Optimization**
   - Minification (removes whitespace, renames variables)
   - Compression (gzip, brotli ready)
   - CSS purging (removes unused Tailwind classes)
   - Code splitting per route

5. **Asset Generation**
   - Generates images in multiple formats (WebP, avif)
   - Creates responsive image variants
   - Optimizes font subsets
   - Hashes assets for cache busting

6. **Output Report**
   ```
   ✓ Compiled successfully
   
   Route (app)                                   Size     First Load JS
   ┌ ○ /                                        X KB           X KB
   ├ ○ /api/posts                               -              -
   ├ ○ /_not-found                              X KB           X KB
   └ ○ /layout                                  X KB           X KB
   ```

### Step 5.2: Understanding Build Output

#### .next Directory Structure
```
.next/
├── static/
│   ├── chunks/
│   │   ├── app.js              # Common chunk
│   │   ├── app/page.js         # Per-route bundle
│   │   └── _app-*.js           # App wrapper
│   ├── media/                  # Optimized images
│   └── css/                    # Extracted CSS
├── server/
│   └── app/                    # Server functions
└── cache/
    └── fetch-cache/            # Revalidated data
```

#### Understanding File Sizes

**Good Metrics**:
- Core chunk (runtime): < 50 KB
- Per-page bundle: < 100 KB
- Total initial JS: < 200 KB
- CSS: < 50 KB

**Optimization Opportunities**:
- If bundle > 200 KB: use dynamic imports
- If CSS > 50 KB: check for unused Tailwind classes
- If image > 100 KB: use Image component with quality setting

---

## Phase 6: Advanced Technicalities & Best Practices

### Step 6.1: Routing Patterns

#### Basic Route (Static)
```typescript
// app/about/page.tsx
export default function About() {
  return <h1>About Us</h1>
}
// Route: /about
```

#### Dynamic Single Parameter
```typescript
// app/blog/[id]/page.tsx
export default function BlogPost({ params }: { params: { id: string } }) {
  return <h1>Post {params.id}</h1>
}
// Routes: /blog/1, /blog/2, etc.
```

#### Catch-All Route (Multiple Segments)
```typescript
// app/docs/[...slug]/page.tsx
export default function Docs({ params }: { params: { slug: string[] } }) {
  // params.slug = ['getting-started', 'installation']
  // for route: /docs/getting-started/installation
  return <h1>Docs: {params.slug.join(' / ')}</h1>
}
```

#### Optional Catch-All
```typescript
// app/[[...slug]]/page.tsx
export default function Page({ params }: { params?: { slug?: string[] } }) {
  // Matches: /, /a, /a/b/c
  return <h1>Route: {params?.slug?.join('/') || 'home'}</h1>
}
```

#### Route Groups (Organizing Without URL)
```typescript
app/
├── (marketing)/
│   ├── layout.tsx    # Marketing layout
│   ├── page.tsx      # /
│   └── about.tsx     # /about
└── (dashboard)/
    ├── layout.tsx    # Dashboard layout
    ├── page.tsx      # /page (different layout)
    └── settings.tsx  # /settings
```

### Step 6.2: Data Fetching Patterns

#### Server-Side Fetching (Recommended)
```typescript
// app/posts/page.tsx
async function getPosts() {
  const res = await fetch('https://api.example.com/posts', {
    next: { revalidate: 60 } // Cache for 60 seconds (ISR)
  })
  return res.json()
}

export default async function Posts() {
  const posts = await getPosts()
  return (
    <ul>
      {posts.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

#### Client-Side Fetching (For Dynamic Updates)
```typescript
'use client'

import { useState, useEffect } from 'react'

export default function UserProfile() {
  const [user, setUser] = useState(null)

  useEffect(() => {
    fetch('/api/user')
      .then(res => res.json())
      .then(data => setUser(data))
  }, [])

  return user ? <h1>{user.name}</h1> : <p>Loading...</p>
}
```

#### API Routes (Backend Endpoints)
```typescript
// app/api/posts/route.ts
export async function GET(request: Request) {
  const posts = await db.query('SELECT * FROM posts')
  return Response.json(posts)
}

export async function POST(request: Request) {
  const body = await request.json()
  const post = await db.insert('posts', body)
  return Response.json(post, { status: 201 })
}
```

#### Form Submission with Server Actions
```typescript
'use client'

export default function SignupForm() {
  async function handleSubmit(formData: FormData) {
    'use server'
    const email = formData.get('email')
    // Save to database
    return { success: true }
  }

  return (
    <form action={handleSubmit}>
      <input name="email" type="email" required />
      <button type="submit">Sign Up</button>
    </form>
  )
}
```

### Step 6.3: Environmental Configuration

#### Environment Variables

**Development (.env.local)**
```env
NEXT_PUBLIC_API_URL=http://localhost:3000
DATABASE_URL=postgresql://localhost/dev_db
SECRET_KEY=dev_secret_key
```

**Production**
```env
NEXT_PUBLIC_API_URL=https://api.example.com
DATABASE_URL=postgresql://prod-server/prod_db
SECRET_KEY=<long-secure-random-string>
```

**Usage**:
```typescript
// Client-side (NEXT_PUBLIC_ prefix required)
const apiUrl = process.env.NEXT_PUBLIC_API_URL

// Server-side (any variable)
const dbUrl = process.env.DATABASE_URL

// Cannot access server variables on client side
// process.env.SECRET_KEY // ❌ Returns undefined
```

#### Configuration Files

**next.config.ts**
```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'cdn.example.com',
      },
    ],
  },
  rewrites: async () => {
    return {
      beforeFiles: [
        {
          source: '/api/:path*',
          destination: 'https://api.example.com/:path*',
        },
      ],
    }
  },
};

export default nextConfig;
```

**tsconfig.json Path Aliases**
```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"],
      "@/components/*": ["./app/components/*"],
      "@/lib/*": ["./lib/*"],
      "@/types/*": ["./types/*"]
    }
  }
}
```

### Step 6.4: Security Best Practices

#### Input Validation
```typescript
import { z } from 'zod'

const formSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
})

export async function createUser(formData: unknown) {
  const validated = formSchema.parse(formData)
  // Process validated data
}
```

#### CORS Headers
```typescript
// app/api/users/route.ts
export async function GET(request: Request) {
  return Response.json(
    { users: [] },
    {
      headers: {
        'Access-Control-Allow-Origin': process.env.FRONTEND_URL || '*',
      },
    }
  )
}
```

#### Content Security Policy
```typescript
// next.config.ts
const nextConfig: NextConfig = {
  headers: async () => [
    {
      source: '/:path*',
      headers: [
        {
          key: 'Content-Security-Policy',
          value: "default-src 'self'; script-src 'self' 'unsafe-inline'",
        },
      ],
    },
  ],
};
```

#### Secure Cookies
```typescript
const response = new Response(/* ... */)
response.headers.set(
  'Set-Cookie',
  'session=token; HttpOnly; Secure; SameSite=Strict; Path=/'
)
return response
```

---

## Phase 7: Code Quality & Linting

### Step 7.1: ESLint Configuration

```bash
npm run lint
```

**ESLint Rules Cover**:
- **Next.js Best Practices**: Image optimization, dynamic imports
- **React Rules**: Hook dependencies, key props, component patterns
- **TypeScript**: Type safety violations, unused variables
- **Accessibility**: ARIA attributes, semantic HTML

### Step 7.2: Auto-Fixing Code
```bash
npm run lint -- --fix
```

**Auto-Fixed Issues**:
- Unused imports
- Missing dependencies in useEffect
- Incorrect hook usage
- Formatting inconsistencies

---

## Phase 8: Production Server Execution

### Step 8.1: Starting Production Server

```bash
npm start
```

**What happens**:
1. Reads `.next/` build artifacts
2. Starts Node.js server on port 3000
3. Serves pre-rendered static pages instantly
4. Renders dynamic routes on-demand
5. Streams responses for better perceived performance

### Step 8.2: Production Characteristics

**Performance Metrics**:
- First Contentful Paint (FCP): < 1s
- Largest Contentful Paint (LCP): < 2.5s
- Cumulative Layout Shift (CLS): < 0.1
- Time to Interactive (TTI): < 3s

**Caching Strategy**:
- Static pages: `Cache-Control: public, max-age=31536000, immutable`
- Dynamic pages: `Cache-Control: private, no-cache`
- Assets: Content-hashed for permanent caching

**Compression**:
- Gzip: Automatically applied to responses > 1 KB
- Brotli: Supported by most modern browsers
- Images: WebP, AVIF formats for 30-50% size reduction

---

## Phase 9: Deployment Strategy

### Step 9.1: Vercel Deployment (Recommended)

**Advantages**:
- Zero-configuration Next.js hosting
- Automatic HTTPS with SSL certificate
- Global CDN for edge computing
- Serverless functions for API routes
- Environment variable management
- Automatic rollback on deployment failure

**Deployment Steps**:
1. Push code to GitHub repository
2. Connect repository to Vercel dashboard
3. Configure environment variables
4. Click "Deploy"
5. Automatic deployment on git push

### Step 9.2: Self-Hosted Deployment

**Docker Setup**:
```dockerfile
FROM node:18-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

EXPOSE 3000
CMD ["npm", "start"]
```

**Deployment**:
```bash
# Build Docker image
docker build -t nextjs-app .

# Run container
docker run -p 3000:3000 nextjs-app
```

**Load Balancing**:
- Run multiple containers
- Place behind NGINX or HAProxy
- Health checks every 30 seconds
- Auto-restart on failure

---

## Phase 10: Advanced Features Implementation

### Step 10.1: Database Integration

#### With Prisma ORM
```typescript
// lib/db.ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()
export default prisma

// app/api/users/route.ts
import prisma from '@/lib/db'

export async function GET() {
  const users = await prisma.user.findMany()
  return Response.json(users)
}
```

#### Connection Pooling
```typescript
// DATABASE_URL="******host/db?schema=public"
// Prisma automatically handles connection pooling
```

### Step 10.2: Authentication Implementation

#### JWT-Based Auth
```typescript
import { jwtVerify } from 'jose'

export async function verifyAuth(token: string) {
  const secret = new TextEncoder().encode(process.env.JWT_SECRET!)
  const { payload } = await jwtVerify(token, secret)
  return payload
}

// middleware.ts
import { NextRequest, NextResponse } from 'next/server'

export async function middleware(request: NextRequest) {
  const token = request.cookies.get('auth-token')?.value
  
  if (!token) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  try {
    await verifyAuth(token)
    return NextResponse.next()
  } catch (err) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
}

export const config = {
  matcher: ['/dashboard/:path*'],
}
```

### Step 10.3: Real-Time Features

#### Server-Sent Events (SSE)
```typescript
// app/api/events/route.ts
export async function GET(request: Request) {
  const encoder = new TextEncoder()
  
  return new Response(
    new ReadableStream({
      start(controller) {
        setInterval(() => {
          const message = `data: ${JSON.stringify({ time: new Date() })}\n\n`
          controller.enqueue(encoder.encode(message))
        }, 1000)
      },
    }),
    {
      headers: {
        'Content-Type': 'text/event-stream',
        'Cache-Control': 'no-cache',
      },
    }
  )
}
```

---

## Phase 11: Monitoring & Maintenance

### Step 11.1: Web Vitals Monitoring
```typescript
// app/layout.tsx
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function RootLayout({ children }) {
  useReportWebVitals((metric) => {
    console.log(metric)
    // Send to analytics service
    fetch('/api/metrics', { method: 'POST', body: JSON.stringify(metric) })
  })

  return (
    <html>
      <body>{children}</body>
    </html>
  )
}
```

### Step 11.2: Error Tracking
```typescript
// app/error.tsx
'use client'

import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // Send to Sentry
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  )
}
```

---

## Quick Start Command Reference

```bash
# Setup Phase
npm install                    # Install dependencies
npm run dev                    # Start dev server

# Development
# Edit files in app/ directory
# Browser auto-updates with Fast Refresh

# Production Build Phase
npm run build                  # Build for production
npm start                      # Run production server

# Quality Assurance
npm run lint                   # Run ESLint
npm run lint -- --fix         # Auto-fix issues

# Monitoring
npm run build                  # Check build warnings
```

---

## Key Differentiators (Why This Boilerplate Excels)

✅ **Server Components First**: Modern React 19 with automatic client/server splitting  
✅ **Turbopack Performance**: 10-100x faster builds than traditional Webpack  
✅ **Type Safety**: Full TypeScript integration across entire stack  
✅ **Zero Runtime Overhead**: Server components eliminate unnecessary JavaScript  
✅ **Built-in Optimization**: Images, fonts, CSS automatically optimized  
✅ **Streaming Ready**: Suspense boundaries for progressive enhancement  
✅ **Production Ready**: Security, caching, compression configured out-of-the-box  
✅ **Developer Experience**: Hot reload, instant feedback, excellent error messages  

---

## Conclusion

This Next.js 15 boilerplate represents modern full-stack web development best practices:

- **Architecture**: App Router with Server Components
- **Performance**: Turbopack, automatic code splitting, image optimization
- **Type Safety**: Strict TypeScript across frontend and backend
- **Developer Experience**: Fast refresh, excellent tooling integration
- **Production Ready**: Security hardened, performance optimized

Following this guide enables building enterprise-grade applications that impress technical judges with solid architectural understanding, optimal performance, and adherence to modern web development standards.
