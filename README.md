# 🛒 বাজার দর (BazarDor) 

## তুমি নিজে বানাও! Step-by-Step Guided Walkthrough

> **এই গাইডে full code দেওয়া নেই।** এখানে তোমাকে ধাপে ধাপে বলা হবে কী করতে হবে, কেন করতে হবে, এবং crucial code snippet দেওয়া হবে। বাকিটা তোমাকে নিজে implement করতে হবে। এটাই শেখার সবচেয়ে ভালো উপায়! 💪

> **টেকনোলজি:** Next.js 16 | TypeScript | Tailwind CSS v4 | DaisyUI | BetterAuth | MongoDB Atlas

---

## 📑 সূচিপত্র

| ধাপ | বিষয় | ক্যাটাগরি |
|-----|-------|-----------|
| ১ | [প্রজেক্ট সেটআপ ও ইনস্টলেশন](#ধাপ-১-প্রজেক্ট-সেটআপ-ও-ইনস্টলেশন) | 🔧 Setup |
| ২ | [ফোল্ডার স্ট্রাকচার বুঝে নাও](#ধাপ-২-ফোল্ডার-স্ট্রাকচার-বুঝে-নাও) | 📁 Structure |
| ৩ | [Environment Variables ও External Services](#ধাপ-৩-environment-variables-ও-external-services) | 🔑 Config |
| ৪ | [BetterAuth — Authentication সেটআপ](#ধাপ-৪-betterauth--authentication-সেটআপ) | 🔐 Auth |
| ৫ | [Middleware দিয়ে Route Protection](#ধাপ-৫-middleware-দিয়ে-route-protection) | 🛡️ Security |
| ৬ | [Utility Functions ও Type Definitions](#ধাপ-৬-utility-functions-ও-type-definitions) | 🧰 Utils |
| ৭ | [Layout, Navbar ও Footer বানাও](#ধাপ-৭-layout-navbar-ও-footer-বানাও) | 🧱 Layout |
| ৮ | [Home Page — Hero, Price Sections, All Products](#ধাপ-৮-home-page) | 🏠 Pages |
| ৯ | [Category Page — Dynamic Route + Sorting](#ধাপ-৯-category-page) | 📂 Pages |
| ১০ | [Product Details Page — Protected Route](#ধাপ-১০-product-details-page) | 📦 Pages |
| ১১ | [Sign In ও Sign Up Page](#ধাপ-১১-sign-in-ও-sign-up-page) | 🔑 Auth Pages |
| ১২ | [Profile ও Update Profile Page](#ধাপ-১২-profile-ও-update-profile-page) | 👤 Pages |
| ১৩ | [404 Page ও Loading Skeletons](#ধাপ-১৩-404-page-ও-loading-skeletons) | ⚡ UX |
| ১৪ | [Deployment — Vercel এ Deploy করো](#ধাপ-১৪-deployment) | 🚀 Deploy |

---

## ধাপ ১: প্রজেক্ট সেটআপ ও ইনস্টলেশন

### 🎯 এই ধাপে কী করবে:
Next.js প্রজেক্ট তৈরি করবে, প্রয়োজনীয় package install করবে, এবং DaisyUI configure করবে।

### 📝 কাজের তালিকা:

**1. Next.js প্রজেক্ট তৈরি করো:**
```bash
npx create-next-app@latest ./
```
- TypeScript → **Yes**
- ESLint → **Yes**  
- Tailwind CSS → **Yes**
- `src/` directory → **Yes**
- App Router → **Yes**
- Turbopack → **Yes**
- Import alias → **@/***

**2. প্রয়োজনীয় packages install করো:**
```bash
npm install better-auth @better-auth/mongo-adapter mongodb react-hot-toast
npm install daisyui@latest
```

**3. DaisyUI enable করো — `src/app/globals.css` এ:**

> ⚠️ **গুরুত্বপূর্ণ:** Tailwind CSS v4 এ আর `tailwind.config.ts` এ plugin হিসেবে DaisyUI যোগ করা লাগে না। CSS ফাইলে `@plugin` directive ব্যবহার করতে হয়।

```css
@import "tailwindcss";
@plugin "daisyui";
```

**4. Google Font যোগ করো:**

`layout.tsx` এ `next/font/google` থেকে তোমার পছন্দের font import করো। যেমন Bangla text এর জন্য `Hind_Siliguri` ভালো কাজ করে:

```tsx
import { Hind_Siliguri } from "next/font/google";

const hindSiliguri = Hind_Siliguri({
  subsets: ["bengali", "latin"],
  weight: ["300", "400", "500", "600", "700"],
});
```

> 💡 **হিন্ট:** এই font instance কে `<body>` এর `className` এ apply করবে।

**5. Git Initialize ও প্রথম কমিট:**
```bash
git init
git add .
git commit -m "feat: initialize next.js project with tailwind, daisyui, better-auth"
```

---

## ধাপ ২: ফোল্ডার স্ট্রাকচার বুঝে নাও

### 🎯 এই ধাপে কী শিখবে:
কোন ফাইল কোথায় তৈরি করতে হবে তার পরিকল্পনা।

### 📝 তোমাকে যে ফোল্ডার/ফাইলগুলো তৈরি করতে হবে:

```
src/
├── app/
│   ├── api/auth/[...all]/route.ts     ← Auth API handler
│   ├── category/[slug]/page.tsx       ← Category page (Dynamic Route)
│   ├── product/[slug]/page.tsx        ← Product Details (Protected)
│   ├── signin/page.tsx                ← Sign In
│   ├── signup/page.tsx                ← Sign Up
│   ├── profile/page.tsx               ← Profile (Protected)
│   ├── profile/update/page.tsx        ← Update Profile (Protected)
│   ├── not-found.tsx                  ← 404 Page
│   ├── layout.tsx                     ← Root Layout
│   ├── page.tsx                       ← Home Page
│   └── loading.tsx                    ← Home Loading Skeleton
├── components/                        ← তোমার সব reusable components
├── lib/
│   ├── auth.ts                        ← BetterAuth Server Config
│   ├── auth-client.ts                 ← BetterAuth Client Config
│   └── utils.ts                       ← Helper functions + Types
└── proxy.ts                      ← Route Protection
```

> 💡 **হিন্ট:** Next.js App Router এ `[slug]` মানে dynamic route। `[...all]` মানে catch-all route যেটা `/api/auth/` এর পরে যেকোনো path handle করবে।

> 💡 **হিন্ট:** `components/` ফোল্ডারে তোমার প্রজেক্টের প্রয়োজন অনুযায়ী component বানাবে — Navbar, Footer, ProductCard, Hero ইত্যাদি।

---

## ধাপ ৩: Environment Variables ও External Services

### 🎯 এই ধাপে কী করবে:
MongoDB Atlas, Google OAuth, GitHub OAuth সেটআপ করবে এবং `.env.local` ফাইল তৈরি করবে।

### 📝 `.env.local` ফাইলে এগুলো থাকবে:

```env
# MongoDB
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/<db-name>

# BetterAuth
BETTER_AUTH_SECRET=<random-32-char-string>
BETTER_AUTH_URL=http://localhost:3000

# Google OAuth
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...

# GitHub OAuth (optional)
GITHUB_CLIENT_ID=...
GITHUB_CLIENT_SECRET=...

# Client-side
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

### 🔧 External Services সেটআপ:

**MongoDB Atlas:**
1. https://cloud.mongodb.com → Free Cluster তৈরি করো
2. Database Access → User তৈরি করো
3. Network Access → `0.0.0.0/0` allow করো
4. Connect → Connection String কপি করো

**Google OAuth:**
1. https://console.cloud.google.com → নতুন project
2. APIs & Services → Credentials → OAuth 2.0 Client ID
3. Redirect URI: `http://localhost:3000/api/auth/callback/google`

**GitHub OAuth:**
1. https://github.com/settings/developers → New OAuth App
2. Callback URL: `http://localhost:3000/api/auth/callback/github`

---

## ধাপ ৪: BetterAuth — Authentication সেটআপ

### 🎯 এই ধাপে কী করবে:
Server-side auth instance, client-side auth hooks, এবং API route handler সেটআপ করবে।

### 📝 তিনটা ফাইল তৈরি করতে হবে:

---

### ফাইল ১: `src/lib/auth.ts` — Server Instance

**ধারণা:** এই ফাইলে BetterAuth এর server instance তৈরি হবে, যেটা MongoDB এর সাথে connect করবে।

**Crucial Code:**
```ts
import { betterAuth } from "better-auth";
import { MongoClient } from "mongodb";
import { mongodbAdapter } from "better-auth/adapters/mongodb";

const client = new MongoClient(process.env.MONGODB_URI as string);
const db = client.db("your-db-name"); // তোমার database নাম দাও

export const auth = betterAuth({
  emailAndPassword: { enabled: true },
  socialProviders: {
    google: {
      clientId: process.env.GOOGLE_CLIENT_ID as string,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET as string,
    },
    github: {
      clientId: process.env.GITHUB_CLIENT_ID as string,
      clientSecret: process.env.GITHUB_CLIENT_SECRET as string,
    },
  },
  database: mongodbAdapter(db, { client }),
});
```

> 💡 **কী হচ্ছে এখানে?**
> - `MongoClient` দিয়ে MongoDB তে connect হচ্ছে
> - `betterAuth()` call করে auth instance তৈরি হচ্ছে
> - `emailAndPassword` enabled মানে email/password দিয়ে login হবে
> - `socialProviders` এ Google ও GitHub login দেওয়া হচ্ছে
> - `mongodbAdapter` দিয়ে user/session data MongoDB তে store হবে

---

### ফাইল ২: `src/lib/auth-client.ts` — Client Instance

**ধারণা:** Client-side এ (React components এ) auth ব্যবহার করার জন্য এই ফাইল লাগবে।

**Crucial Code:**
```ts
"use client";

import { createAuthClient } from "better-auth/react";

export const authClient = createAuthClient({
  baseURL: process.env.NEXT_PUBLIC_APP_URL || "http://localhost:3000",
});

export const { signIn, signUp, signOut, useSession } = authClient;
```

> 💡 **কী হচ্ছে এখানে?**
> - `"use client"` — এটা client component মার্ক করছে
> - `createAuthClient` — BetterAuth এর React hooks তৈরি করছে
> - `signIn`, `signUp`, `signOut`, `useSession` — এগুলো তুমি যেকোনো component এ import করে ব্যবহার করবে

---

### ফাইল ৩: `src/app/api/auth/[...all]/route.ts` — API Handler

**ধারণা:** BetterAuth এর সব auth API endpoint এই single route handler handle করবে।

**Crucial Code:**
```ts
import { auth } from "@/lib/auth";
import { toNextJsHandler } from "better-auth/next-js";

export const { POST, GET } = toNextJsHandler(auth);
```

> 💡 **কী হচ্ছে?** `[...all]` catch-all route — `/api/auth/signin`, `/api/auth/signup`, `/api/auth/callback/google` ইত্যাদি সব endpoint এখানে handle হবে।

---

## ধাপ ৫: Middleware দিয়ে Route Protection

### 🎯 এই ধাপে কী করবে:
নির্দিষ্ট route গুলোতে logged-in না থাকলে signin page এ redirect করবে।

### 📝 `src/proxy.ts` তৈরি করো

**ধারণা:** Middleware হলো এমন একটা function যেটা প্রতিটা request এর আগে চলে। তুমি এখানে check করবে user logged in কিনা।

**তোমাকে যা করতে হবে:**
1. কোন routes protect করবে define করো (array হিসেবে)
2. BetterAuth এর session cookie check করো
3. Cookie না থাকলে `/signin` এ redirect করো, সাথে `callbackUrl` query param দাও





**Crucial Code — Matcher Config:**
```ts
export const config = {
  matcher: ["/product/:path*", "/profile/:path*"],
};
```

> 💡 **কেন `callbackUrl`?** ধরো user `/product/rice` এ যেতে চাইছে কিন্তু logged in না। তখন signin page এ redirect হবে। Login করার পর user কে আবার `/product/rice` এ নিয়ে যাওয়ার জন্য `callbackUrl` save করা হচ্ছে।

> ⚠️ **নিজে করো:** উপরের hints দেখে পুরো middleware function টা নিজে লেখো। `NextRequest`, `NextResponse` import করো `next/server` থেকে।

---

## ধাপ ৬: Utility Functions ও Type Definitions

### 🎯 এই ধাপে কী করবে:
Helper functions বানাবে যেগুলো পুরো project জুড়ে ব্যবহার হবে, এবং TypeScript type/interface define করবে।

### 📝 `src/lib/utils.ts` তৈরি করো

**তোমাকে যে functions বানাতে হবে:**

#### 1. `toBengaliNumber(num)` — ইংরেজি সংখ্যাকে বাংলায় রূপান্তর

**ধারণা:** `"123"` → `"১২৩"` করতে হবে

**Crucial Code — Mapping:**
```ts
const bengaliDigits = ["০", "১", "২", "৩", "৪", "৫", "৬", "৭", "৮", "৯"];
```

> 💡 **হিন্ট:** `String(num).replace(/[0-9]/g, callback)` ব্যবহার করো। Callback এ digit কে bengaliDigits array তে index হিসেবে use করো।

#### 2. `formatPrice(price)` — দাম কমা দিয়ে ফরম্যাট + বাংলায়

**ধারণা:** `1850` → `"১,৮৫০"`

> 💡 **হিন্ট:** `price.toLocaleString("en-IN")` Indian-style comma দেবে, তারপর `toBengaliNumber()` call করো।

#### 3. `getUnitBn(unit)` — ইউনিট বাংলায়

**ধারণা:** `"kg"` → `"প্রতি কেজি"`, `"litre"` → `"প্রতি লিটার"` ইত্যাদি।

> 💡 **হিন্ট:** একটা `Record<string, string>` object বানাও key-value mapping দিয়ে।

#### 4. `getBanglaDate()` — আজকের তারিখ বাংলায়

**ধারণা:** `"শনিবার, ১০ অক্টোবর, ২০২৬"` format এ return করবে

> 💡 **হিন্ট:** `new Date()` থেকে `getDay()`, `getDate()`, `getMonth()`, `getFullYear()` নিয়ে বাংলা দিন ও মাসের array থেকে match করো।

#### 5. TypeScript Interfaces

**তোমাকে দুটো interface define করতে হবে:**

```ts
export interface Product {
  id: number;
  slug: string;
  nameBn: string;
  category: string;
  categoryNameBn: string;
  categoryIcon: string;
  unit: string;
  image: string;
  today: number;
  yesterday: number;
  lastWeek: number;
  lastMonth: number;
  change: {
    dir: "up" | "down" | "flat";
    pct: number;
  };
  markets: {
    market: string;
    division: string;
    min: number;
    max: number;
  }[];
}

export interface Category {
  id: string;
  slug: string;
  nameBn: string;
  icon: string;
}
```

> 💡 **কেন Type define করছি?** TypeScript এ type define করলে তুমি কোনো property ভুলে miss করলে বা wrong type দিলে editor ই তোমাকে ধরিয়ে দেবে।

---

## ধাপ ৭: Layout, Navbar ও Footer বানাও

### 🎯 এই ধাপে কী করবে:
Root Layout, Navbar (categories + auth + price ticker), এবং Footer component তৈরি করবে।

---

### ৭.১ Root Layout — `src/app/layout.tsx`

**তোমাকে যা করতে হবে:**
1. Google Font import ও apply করো
2. `Navbar` ও `Footer` component import করে layout এ বসাও
3. `react-hot-toast` এর `Toaster` component যোগ করো
4. `metadata` export করো (SEO এর জন্য)

**Structure হবে এরকম:**
```tsx
<html lang="bn" data-theme="light">
  <body className={`${font.className} antialiased`}>
    <Navbar />
    <main className="min-h-screen">{children}</main>
    <Footer />
    <Toaster position="top-center" />
  </body>
</html>
```

> 💡 **কেন `data-theme="light"`?** DaisyUI theme system ব্যবহার করে — এটা দিলে DaisyUI components সঠিক color পাবে।

---

### ৭.২ Navbar Component — `src/components/Navbar.tsx`

**এটা `"use client"` component হবে কারণ:**
- `useState` লাগবে (mobile menu toggle, categories data)
- `useSession` লাগবে (logged in কিনা check)
- `usePathname` লাগবে (active category highlight)

**তোমাকে যে features implement করতে হবে:**

**Feature 1: Categories API থেকে fetch করো**
```
API: https://api.api-store.workers.dev/api/bazardor/categories
```
> 💡 `useEffect` এ fetch করে `useState` এ store করো। তারপর map করে Link হিসেবে render করো।

**Feature 2: Active Category Highlight**
```ts
const pathname = usePathname();
const isActive = pathname === `/category/${cat.slug}`;
// isActive হলে different styling দাও
```

**Feature 3: Auth State দেখাও**
```tsx
const { data: session, isPending } = useSession();
// isPending → spinner দেখাও
// session আছে → user name + dropdown (profile link, sign out)
// session নেই → sign in / sign up buttons
```

**Feature 4: Sign Out Handler**
```ts
const handleSignOut = async () => {
  await signOut({
    fetchOptions: {
      onSuccess: () => {
        toast.success("সাইন আউট সফল!");
        router.push("/");
      },
    },
  });
};
```

**Feature 5: Mobile Responsive Menu**
> 💡 **হিন্ট:** `useState` দিয়ে `mobileMenuOpen` state রাখো। hamburger icon click এ toggle করো। `lg:hidden` ও `lg:block` দিয়ে responsive করো।

---

### ৭.৩ Price Ticker Component — `src/components/PriceTicker.tsx`

**ধারণা:** Stock market এর মতো scrolling ticker — সব product এর নাম, দাম, এবং পরিবর্তন দেখাবে।

**তোমাকে যা করতে হবে:**
1. Products API থেকে data fetch করো (`useEffect`)
2. CSS marquee animation দিয়ে horizontal scroll করাও
3. দাম বেড়েছে → লাল (▲), কমেছে → সবুজ (▼), flat → ধূসর (—)

**Crucial Code — Infinite scroll trick:**
```ts
// দুইবার repeat করো smooth infinite scroll এর জন্য
const tickerItems = [...products, ...products];
```

**Crucial Code — CSS Animation (globals.css এ যোগ করো):**
```css
@keyframes marquee {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

.animate-marquee {
  animation: marquee 30s linear infinite;
}

.animate-marquee:hover {
  animation-play-state: paused;
}
```

> 💡 **কেন `-50%`?** কারণ তুমি items দুইবার repeat করেছো। যখন scroll অর্ধেক যাবে, তখন দ্বিতীয় copy শুরু হবে — আর seamless loop হবে।

---

### ৭.৪ Footer Component — `src/components/Footer.tsx`

> এটা সহজ — একটা simple footer বানাও। Logo, tagline, এবং disclaimer text রাখো। Dark background (`bg-gray-900`) ভালো দেখায়।

---

## ধাপ ৮: Home Page

### 🎯 এই ধাপে কী করবে:
Home page বানাবে — Hero section, দাম বেড়েছে/কমেছে section, এবং সব পণ্যের grid।

### 📝 API Information:
```
Products API: https://api.api-store.workers.dev/api/bazardor/products
```

---

### ৮.১ Home Page — `src/app/page.tsx` (Server Component)

**ধারণা:** এটা Server Component — সরাসরি `async` function হিসেবে data fetch করবে।

**Crucial Code — Server-side Data Fetching:**
```ts
async function getProducts(): Promise<Product[]> {
  const res = await fetch(
    "https://api.api-store.workers.dev/api/bazardor/products",
    { next: { revalidate: 60 } } // ৬০ সেকেন্ড cache
  );
  if (!res.ok) throw new Error("Failed to fetch products");
  return res.json();
}
```

> 💡 **কেন `{ next: { revalidate: 60 } }`?** এটা Next.js এর ISR (Incremental Static Regeneration) — ৬০ সেকেন্ড পর পর data fresh হবে। প্রতিটা request এ API call হবে না।

**Page এর Structure:**
```tsx
export default async function HomePage() {
  const products = await getProducts();
  return (
    <>
      <Hero />
      <PriceRisers products={products} />
      <PriceFallers products={products} />
      <AllProducts products={products} />
    </>
  );
}
```

> ⚠️ **নিজে করো:** নিচের ৪টা component নিজে বানাও!

---

### ৮.২ Hero Component

**কী থাকবে:**
- আজকের বাংলা তারিখ (badge হিসেবে)
- একটা heading — "আজকের বাজারের দাম এক নজরে"
- একটা description paragraph
- "সব পণ্য দেখুন" anchor link (`href="#সব-পণ্য"`)
- ডান পাশে একটা বড় emoji (🧺) বা illustration

> 💡 **হিন্ট:** `bg-gradient-to-r from-green-50 to-green-100` দিয়ে gradient background দিলে সুন্দর দেখাবে।

---

### ৮.৩ ProductCard Component (Reusable)

**ধারণা:** একটা card component যেটা একটা product এর তথ্য দেখাবে — emoji, নাম, unit, আজকের দাম, পরিবর্তনের badge।

**তোমাকে যা implement করতে হবে:**
1. পুরো card টাকে `<Link href={/product/${product.slug}}>` এ wrap করো
2. Product এর emoji image, নাম, unit দেখাও
3. আজকের দাম `formatPrice()` দিয়ে format করে দেখাও
4. `change.dir` অনুযায়ী badge color দাও:
   - `"up"` → লাল badge (`badge-error`) — ▲
   - `"down"` → সবুজ badge (`badge-success`) — ▼
   - `"flat"` → ধূসর badge (`badge-ghost`) — —

> 💡 **হিন্ট:** DaisyUI এর `card`, `badge` classes ব্যবহার করো। Hover effect দিলে interactive লাগবে।

---

### ৮.৪ PriceRisers Component

**ধারণা:** দাম বেড়েছে এমন top ৬টা product দেখাবে।

**Crucial Logic:**
```ts
const risers = products
  .filter((p) => p.change.dir === "up")
  .sort((a, b) => b.change.pct - a.change.pct) // বেশি থেকে কম
  .slice(0, 6);
```

> ⚠️ **নিজে করো:** filter → sort → slice → map করে ProductCard render করো grid layout এ।

---

### ৮.৫ PriceFallers Component

> একই pattern — শুধু `"down"` filter করো। ৬টা দেখাও।

---

### ৮.৬ AllProducts Component

> সব products grid এ দেখাও। `id="সব-পণ্য"` দাও section এ যাতে Hero এর anchor link কাজ করে।

---

## ধাপ ৯: Category Page

### 🎯 এই ধাপে কী করবে:
Dynamic route দিয়ে category-wise product list দেখাবে, সাথে sorting feature থাকবে।

### 📝 ফাইল: `src/app/category/[slug]/page.tsx`

**এটা `"use client"` component হবে** কারণ sorting এর জন্য `useState` লাগবে।

### APIs:
```
Products: https://api.api-store.workers.dev/api/bazardor/products?category={slug}
Category: https://api.api-store.workers.dev/api/bazardor/categories/{slug}
```

### তোমাকে যে features implement করতে হবে:

**Feature 1: Dynamic route থেকে slug নাও**
```ts
const params = useParams();
const slug = params.slug as string;
```

**Feature 2: দুটো API একসাথে fetch করো**
> 💡 **হিন্ট:** `Promise.all()` ব্যবহার করো দুটো fetch একসাথে চালাতে।

**Feature 3: Sorting (Challenge C1)**

SortDropdown নামে আলাদা component বানাও — একটা `<select>` element:
- ডিফল্ট
- দাম: কম থেকে বেশি
- দাম: বেশি থেকে কম

**Crucial Code — Sort Logic:**
```ts
const [sortBy, setSortBy] = useState("default");

const sortedProducts = [...products].sort((a, b) => {
  if (sortBy === "price-asc") return a.today - b.today;
  if (sortBy === "price-desc") return b.today - a.today;
  return 0;
});
```

> ⚠️ **সাবধান!** `products.sort()` না করে `[...products].sort()` করো — কারণ React এ direct state mutation করা যায় না।

**Feature 4: তিনটা State Handle করো:**
1. **Loading** → Skeleton দেখাও
2. **Error/Empty** → "কোনো পণ্য পাওয়া যায়নি" message দেখাও + home link
3. **Success** → Products grid + sort dropdown

---

## ধাপ ১০: Product Details Page

### 🎯 এই ধাপে কী করবে:
একটা single product এর detailed view বানাবে — breadcrumb, price summary, market-wise prices, historical comparison।

### 📝 ফাইল: `src/app/product/[slug]/page.tsx`

**এটা Server Component হবে** — `async function` দিয়ে data fetch করবে।

### তোমাকে যে features implement করতে হবে:

**Feature 1: Slug দিয়ে Product খোঁজো**

API তে direct slug endpoint নেই, তাই সব product fetch করে find করো:

```ts
async function getProduct(slug: string): Promise<Product | null> {
  const res = await fetch("...products API...", { next: { revalidate: 60 } });
  const products: Product[] = await res.json();
  return products.find((p) => p.slug === slug) || null;
}
```

**Feature 2: Product না পেলে 404 দেখাও**
```ts
import { notFound } from "next/navigation";
// ...
if (!product) notFound();
```

**Feature 3: Next.js 16 এ `params` হলো Promise**
```ts
export default async function ProductPage({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  // ...
}
```

**Feature 4: Markets কে Division অনুযায়ী গ্রুপ করো**

**Crucial Code — Grouping with reduce:**
```ts
const marketsByDivision = product.markets.reduce(
  (acc, market) => {
    if (!acc[market.division]) acc[market.division] = [];
    acc[market.division].push(market);
    return acc;
  },
  {} as Record<string, typeof product.markets>
);
```

**Feature 5: Price Statistics Calculate করো**
```ts
const allMins = product.markets.map((m) => m.min);
const allMaxs = product.markets.map((m) => m.max);
const minPrice = Math.min(...allMins);
const maxPrice = Math.max(...allMaxs);
const avgPrice = Math.round(
  product.markets.reduce((sum, m) => sum + (m.min + m.max) / 2, 0) /
    product.markets.length
);
```

**Feature 6: Page এর UI Sections:**
1. **Breadcrumb** — হোম > ক্যাটাগরি > প্রোডাক্ট নাম
2. **Summary Card** — emoji, নাম, tags (category, unit, change badge)
3. **Price Cards (4টা grid)** — সর্বনিম্ন, সর্বোচ্চ, গড়, আজকের দাম
4. **Market Table** — Division-wise grouping, প্রতিটা market এ min/max
5. **Historical Comparison** — আজ, গতকাল, গত সপ্তাহ, গত মাস

> 💡 **হিন্ট:** DaisyUI এর `breadcrumbs`, `table table-zebra`, `badge` classes কাজে দেবে।

---

## ধাপ ১১: Sign In ও Sign Up Page

### 🎯 এই ধাপে কী করবে:
Email/Password login, Google ও GitHub social login সহ authentication pages বানাবে।

---

### ১১.১ Sign In Page — `src/app/signin/page.tsx`

**এটা `"use client"` component।**

**তোমাকে যে features implement করতে হবে:**

**Feature 1: callbackUrl handle করো**
```ts
const searchParams = useSearchParams();
const callbackUrl = searchParams.get("callbackUrl") || "/";
```
> এটা middleware থেকে আসা redirect URL — login এর পর এখানে নিয়ে যাবে।

**Feature 2: Email Sign In**
```ts
const { data, error } = await signIn.email({
  email,
  password,
  callbackURL: callbackUrl,
});

if (error) {
  toast.error(error.message || "সাইন ইন ব্যর্থ!");
} else {
  toast.success("সাইন ইন সফল! 🎉");
  router.push(callbackUrl);
}
```

**Feature 3: Social Sign In**
```ts
// Google
await signIn.social({ provider: "google", callbackURL: callbackUrl });

// GitHub
await signIn.social({ provider: "github", callbackURL: callbackUrl });
```

**Feature 4: UI Structure:**
- Social login buttons (Google, GitHub) — উপরে
- Divider — "অথবা ইমেইল দিয়ে"
- Email/Password form
- Loading state — button এ spinner
- নিচে link — "অ্যাকাউন্ট নেই? রেজিস্টার করুন"

---

### ১১.২ Sign Up Page — `src/app/signup/page.tsx`

**Feature 1: Email Sign Up**
```ts
const { data, error } = await signUp.email({
  name,
  email,
  password,
});

if (error) {
  toast.error(error.message || "রেজিস্ট্রেশন ব্যর্থ!");
} else {
  toast.success("রেজিস্ট্রেশন সফল!");
  router.push("/signin");
}
```

> 💡 **হিন্ট:** Sign Up page Almost Sign In page এর মতোই — শুধু name field extra আছে, এবং `signUp.email()` ব্যবহার হচ্ছে `signIn.email()` এর বদলে।

---

## ধাপ ১২: Profile ও Update Profile Page

### 🎯 এই ধাপে কী করবে:
User profile দেখানো এবং name update করার functionality বানাবে।

---

### ১২.১ Profile Page — `src/app/profile/page.tsx`

**তোমাকে যা handle করতে হবে:**
1. **isPending** → Loading spinner দেখাও
2. **session নেই** → "আপনি লগইন করেননি" + sign in link
3. **session আছে** → User এর avatar, নাম, email দেখাও + "তথ্য আপডেট করুন" button

**Avatar logic:**
```tsx
{session.user.image ? (
  <img src={session.user.image} alt={session.user.name} className="rounded-full" />
) : (
  session.user.name?.charAt(0) || "U"
)}
```

---

### ১২.২ Update Profile Page — `src/app/profile/update/page.tsx` (Challenge C3)

**ধারণা:** BetterAuth এর `updateUser` API ব্যবহার করে user name update করবে।

**Crucial Code — Session থেকে name pre-fill:**
```ts
useEffect(() => {
  if (session?.user?.name) {
    setName(session.user.name);
  }
}, [session]);
```

**Crucial Code — Update API call:**
```ts
// BetterAuth docs: https://better-auth.com/docs/concepts/users-accounts#update-user
const { data, error } = await authClient.updateUser({
  name: name,
});

if (error) {
  toast.error("আপডেট ব্যর্থ!");
} else {
  toast.success("প্রোফাইল আপডেট হয়েছে! 🎉");
  router.push("/profile");
}
```

> 💡 **গুরুত্বপূর্ণ:** `authClient.updateUser()` ব্যবহার করতে হবে — এটা `auth-client.ts` থেকে import হবে।

---

## ধাপ ১৩: 404 Page ও Loading Skeletons

### 🎯 এই ধাপে কী করবে:
Custom 404 page এবং loading skeleton components বানাবে।

---

### ১৩.১ 404 Page — `src/app/not-found.tsx`

**একটা সুন্দর 404 page বানাও:**
- বড় emoji (🔍)
- Heading — "পেজটি পাওয়া যায়নি"
- Description
- "হোম পেজে ফিরে যান" button

> 💡 **কেন `not-found.tsx`?** Next.js App Router এ এই filename convention — auto 404 page হিসেবে কাজ করে। আর code থেকে `notFound()` call করলেও এই page দেখাবে।

---

### ১৩.২ Loading Skeletons

**ধারণা:** Next.js App Router এ কোনো folder এ `loading.tsx` রাখলে সেই route এর data fetch হওয়ার সময় automatically এই component দেখাবে।

**তোমাকে ৩টা loading file বানাতে হবে:**
1. `src/app/loading.tsx` — Home page skeleton
2. `src/app/category/[slug]/loading.tsx` — Category page skeleton
3. `src/app/product/[slug]/loading.tsx` — Product details skeleton

**DaisyUI Skeleton ব্যবহার:**
```tsx
<div className="skeleton h-8 w-48"></div>
<div className="skeleton h-4 w-32"></div>
```

> 💡 **হিন্ট:** আসল page এর layout অনুযায়ী skeleton বানাও — card shapes, text line shapes ইত্যাদি। এতে user experience ভালো হয় কারণ user বুঝতে পারে content আসছে।

---

## ধাপ ১৪: Deployment

### 🎯 এই ধাপে কী করবে:
GitHub এ push ও Vercel এ deploy করবে।

### 📝 Steps:

**1. GitHub এ Push করো:**
```bash
git remote add origin https://github.com/<username>/<repo>.git
git branch -M main
git push -u origin main
```

**2. Vercel Deploy:**
1. https://vercel.com → New Project → GitHub repo select
2. Environment Variables যোগ করো (সব `.env.local` variables)
3. **গুরুত্বপূর্ণ:** `BETTER_AUTH_URL` ও `NEXT_PUBLIC_APP_URL` → তোমার Vercel URL দাও
4. Deploy!

**3. OAuth Redirect URIs Update করো:**
- Google Console → Redirect URI: `https://your-app.vercel.app/api/auth/callback/google`
- GitHub OAuth App → Callback URL: `https://your-app.vercel.app/api/auth/callback/github`

---

## ⚠️ Common Mistakes ও Solutions

| সমস্যা | সমাধান |
|---------|--------|
| DaisyUI classes কাজ করছে না | `globals.css` এ `@plugin "daisyui";` আছে কিনা check করো |
| `Module not found: mongodb` | `npm install mongodb` করো |
| Session cookie পাচ্ছে না Vercel এ | `BETTER_AUTH_URL` সঠিক Vercel URL দিয়েছো কিনা দেখো |
| Google OAuth redirect error | Google Console এ production URL add করেছো কিনা check করো |
| `useSearchParams()` error | Page কে `<Suspense>` দিয়ে wrap করতে হতে পারে |
| Sort করলে original array change হচ্ছে | `[...array].sort()` ব্যবহার করো, `array.sort()` নয় |
| Middleware infinite loop | `config.matcher` সঠিকভাবে set করো |

---

## 📋 Git Commit Checklist (মোট ৮+ লাগবে)

তোমার প্রতিটা major feature add করার পর commit করো:

```
✅ 1. feat: initialize project
✅ 2. feat: setup auth with MongoDB
✅ 3. feat: add middleware and utilities
✅ 4. feat: add Navbar, Footer, PriceTicker
✅ 5. feat: add Home page sections
✅ 6. feat: add Category page with sort
✅ 7. feat: add Product Details page
✅ 8. feat: add SignIn and SignUp pages
✅ 9. feat: add Profile and Update Profile
✅ 10. feat: add 404 page
✅ 11. docs: add README
✅ 12. chore: deployment config
```

---

## 🎯 Final Checklist — Submit করার আগে চেক করো

- [ ] Home page — Hero, দাম বেড়েছে, কমেছে, সব পণ্য sections আছে
- [ ] Category page — filter + sort কাজ করছে
- [ ] Product Details — market prices, breadcrumb, price comparison আছে
- [ ] Sign In — email + Google/GitHub login কাজ করছে
- [ ] Sign Up — registration কাজ করছে
- [ ] Profile — logged in user এর info দেখাচ্ছে
- [ ] Update Profile — name update হচ্ছে
- [ ] Protected routes — `/product/*`, `/profile/*` → redirect to signin
- [ ] 404 page — কাজ করছে
- [ ] Loading skeletons — দেখাচ্ছে
- [ ] Responsive — mobile এ ঠিক আছে
- [ ] ৮+ meaningful git commits আছে
- [ ] README.md আছে
- [ ] Vercel এ deploy হয়েছে এবং error-free কাজ করছে

---

> 🎉 **শুভ কামনা!** এই গাইড follow করে তুমি নিজে নিজে পুরো project টা solve করতে পারবে। কোনো step এ আটকে গেলে সেই specific topic নিয়ে Google/BetterAuth docs পড়ো। Happy coding! 💻
