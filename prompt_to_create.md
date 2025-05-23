You are a deep thinking AI, you may use extremely long chains of thought to deeply consider the problem and deliberate with yourself via systematic reasoning processes to help come to a correct solution prior to answering. You will think deeply and thoroughly to explore various implementation options before choosing the most optimal one. You will double-check and validate any code changes before implementing. You should enclose your thoughts and internal monologue inside <think> </think> tags, and then provide your solution or response to the problem.

create a modern e-commerce website using the current website at https://balancecleanse.com/ as your starting design to produce a Next.js website project using TypeScript, Tailwind CSS, Next-UI to produce attractive website with user-friendly, intuitive and dynamic UI with dynamic visual feedback as the user mouse over the web pages.


Now you will outline the project structure, key components, and the development process for creating a modern e-commerce website inspired by balancecleanse.com, using Next.js, TypeScript, Tailwind CSS, and NextUI.  Then follow the guide follow to build the *entire* website here by using the framework, code snippets, and explanations as guide.

**1. Project Setup and Initialization**

   *   **Create a Next.js Project:**
       ```bash
       npx create-next-app@latest balancecleanse-clone --typescript
       cd balancecleanse-clone
       ```

   *   **Install Dependencies:**
       ```bash
       npm install @nextui-org/react tailwindcss postcss autoprefixer
       ```

   *   **Configure Tailwind CSS:**
       1.  **`tailwind.config.js`:**
           ```javascript
           /** @type {import('tailwindcss').Config} */
           module.exports = {
             content: [
               "./pages/**/*.{js,ts,jsx,tsx,mdx}",
               "./components/**/*.{js,ts,jsx,tsx,mdx}",
               "./app/**/*.{js,ts,jsx,tsx,mdx}",
               "./node_modules/@nextui-org/theme/dist/**/*.{js,ts,jsx,tsx}", // Important for NextUI
             ],
             theme: {
               extend: {
                 // Add custom colors, fonts, etc. inspired by balancecleanse.com
                 colors: {
                   'balance-green': '#your-green-color-code', // Replace with actual color
                   'balance-beige': '#your-beige-color-code',
                   // ... other colors
                 },
                 fontFamily: {
                   'balance-sans': ['YourFont', 'sans-serif'], // Replace with actual font
                   // ... other fonts
                 }
               },
             },
             darkMode: "class", // Important for NextUI dark mode
             plugins: [],
           };
           ```
       2.  **`postcss.config.js`:** (This should be created automatically, but double-check)
           ```javascript
           module.exports = {
             plugins: {
               tailwindcss: {},
               autoprefixer: {},
             },
           };
           ```
       3.  **`./styles/globals.css` (or `app/globals.css` if using the `app` directory):**
           ```css
           @tailwind base;
           @tailwind components;
           @tailwind utilities;

           /* Add any global styles here */
           body {
             @apply font-balance-sans; /* Use your custom font */
           }
           ```

   * **Configure NextUI:**
     1. In `app/layout.tsx` (or `pages/_app.tsx` if using the `pages` directory):

        ```typescript
        // app/layout.tsx (or pages/_app.tsx)
        import type { Metadata } from 'next'
        import { Inter } from 'next/font/google'
        import './globals.css'
        import { NextUIProvider } from '@nextui-org/react'

        const inter = Inter({ subsets: ['latin'] })

        export const metadata: Metadata = {
          title: 'Create Next App',
          description: 'Generated by create next app',
        }

        export default function RootLayout({
          children,
        }: {
          children: React.ReactNode
        }) {
          return (
            <html lang="en">
              <body className={inter.className}>
                <NextUIProvider>
                  {children}
                </NextUIProvider>
              </body>
            </html>
          )
        }
        ```

**2. Project Structure (using the `app` directory - recommended for Next.js 13+)**

```
balancecleanse-clone/
├── app/
│   ├── (products)/       # Group related routes (optional, good for organization)
│   │   ├── [slug]/      # Dynamic route for individual product pages
│   │   │   └── page.tsx  # Product detail page
│   │   └── page.tsx      # Product listing page
│   ├── about/
│   │   └── page.tsx      # About Us page
│   ├── contact/
│   │   └── page.tsx      # Contact Us page
│   ├── cart/
│   │    └── page.tsx      # Cart page
│   ├── checkout/
│   │    └── page.tsx      # Checkout page
│   ├── globals.css      # Global styles
│   ├── layout.tsx       # Main layout (includes NextUIProvider)
│   └── page.tsx         # Homepage
├── components/
│   ├── Header.tsx       # Navigation bar
│   ├── Footer.tsx       # Footer
│   ├── ProductCard.tsx  # Reusable component for displaying products
│   ├── Button.tsx        # Custom button component (with hover effects)
│   ├── ...              # Other reusable components
├── lib/                 # Utility functions, API calls, etc.
│   ├── api.ts          # Example: Fetching product data
│   └── ...
├── public/              # Static assets (images, etc.)
├── styles/
│   └── globals.css      # (If not using the app directory)
├── .env.local           # Environment variables (API keys, etc. - keep secret!)
├── next.config.js
├── package.json
├── postcss.config.js
├── tailwind.config.js
└── tsconfig.json
```

**3. Key Components and Code Examples**

   *   **`components/Header.tsx` (Navigation Bar):**

       ```typescript
       import Link from 'next/link';
       import { Navbar, NavbarBrand, NavbarContent, NavbarItem, Button } from "@nextui-org/react";
       import { CartIcon } from './CartIcon'; //  Create a simple CartIcon component

       export default function Header() {
         return (
           <Navbar isBordered>
             <NavbarBrand>
               <Link href="/" className="font-bold text-inherit">
                 Balance Cleanse
               </Link>
             </NavbarBrand>
             <NavbarContent className="hidden sm:flex gap-4" justify="center">
               <NavbarItem>
                 <Link color="foreground" href="/products">
                   Products
                 </Link>
               </NavbarItem>
               <NavbarItem>
                 <Link href="/about" color="foreground">
                   About
                 </Link>
               </NavbarItem>
               <NavbarItem>
                 <Link color="foreground" href="/contact">
                   Contact
                 </Link>
               </NavbarItem>
             </NavbarContent>
             <NavbarContent justify="end">
               <NavbarItem>
                 <Link href="/cart">
                    <CartIcon /> {/* Your cart icon component */}
                 </Link>
               </NavbarItem>
               <NavbarItem>
                  <Button as={Link} color="primary" href="/checkout" variant="flat">
                      Checkout
                  </Button>
               </NavbarItem>
             </NavbarContent>
           </Navbar>
         );
       }
       ```

   *   **`components/ProductCard.tsx`:**

       ```typescript
       import Link from 'next/link';
       import { Card, CardHeader, CardBody, CardFooter, Image, Button } from "@nextui-org/react";
       import { Product } from '@/lib/types'; // Define a Product type

       interface ProductCardProps {
         product: Product;
       }

       export default function ProductCard({ product }: ProductCardProps) {
         return (
           <Card className="w-[300px] h-[400px] hover:scale-105 transition-transform">
             <CardHeader className="absolute z-10 top-1 flex-col items-start">
                <p className="text-tiny text-white/60 uppercase font-bold">{product.category}</p> {/*Example category*/}
                <h4 className="text-white font-medium text-large">{product.name}</h4>
             </CardHeader>
             <Image
              removeWrapper
              alt="Card background"
              className="z-0 w-full h-full object-cover"
              src={product.image}
              width={300}
              height={300}
             />
             <CardFooter className="justify-between before:bg-white/10 border-white/20 border-1 overflow-hidden py-1 absolute before:rounded-xl rounded-large bottom-1 w-[calc(100%_-_8px)] shadow-small ml-1 z-10">
                <p className="text-tiny text-white/80">{product.description.substring(0, 50)}...</p> {/* Shorten description */}
                <Button as={Link} href={`/products/${product.slug}`} className="text-tiny text-white bg-black/20
