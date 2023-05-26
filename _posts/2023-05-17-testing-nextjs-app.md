---
layout: post
title: "Testing Next.js App with Jest and RTL"
date: 2023-05-17 20:00:00 +0700
categories: Next.js
---
Next.js is like the Swiss army knife of front-end app creation. With the release of Next.js v13, it got even better, introducing server components and other thrilling features.

Tutorials, courses, and docs are floating around everywhere. But one question still lingers, like that last piece of popcorn stuck in your teeth: "How do I test Next.js components or pages, especially considering that they are server components by default?"

But one thing I have never seen is how do I test the Next.js component or page? What makes it different since it is a server component by default.

The only documentation I could find was from the [Next.js official site](https://nextjs.org/docs/pages/building-your-application/optimizing/testin). Let's do this.

## Setup

I'm currently running the most recent version of Next.js, v13.4.2. The first step is to install the required packages.

```sh
yarn add -D jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
```

It's a bit baffling that testing isn't first class in Next.js. It's an integral part of development, yet it's not included by default.

Next, create a config file for jest, and put it in the root directory. The name should be `jest.config.mjs`; you can use `jest.config.js` if you are not using the `import` statement.

```mjs
import nextJest from 'next/jest.js';
 
const createJestConfig = nextJest({
  dir: './',
});
 
/** @type {import('jest').Config} */
const config = {
  testEnvironment: 'jest-environment-jsdom',
};
 
export default createJestConfig(config);
```

Just it. Under the hood `next/js` do the other setup for you.

## First Test

To run the test, use the command:

```sh
yarn jest
```

But, since I'm using package json, I aliases it to `yarn test`

```js
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:watch": "jest --watch"
  },
```

My app's directory structure looks like this:

```
src
  |_ app
     |_ page.tsx
     |_ layout.tsx    
```

So, I created a test directory to mimic the same structure:

```
src ...
tests
  |_ app
     |_ page.spec.tsx
     |_ layout.spec.tsx
```

Since I'm using aliases for importing, we also need to adjust that:

```ts
// tsconfig.json
...
"compilerOptions": {
  ...
  "baseUrl": ".",
  "paths": {
    "@app/*": ["./src/app/*"]
  }
  ...
}
...
```

```ts
// jest.config.mjs
const config = {
  ...
  moduleNameMapper: {
    '^@app(.*)$': '<rootDir>/src/app/$1',
  }
  ...
};
```

Now, let's dive into our first test:

```tsx
import { render, screen } from '@testing-library/react';
import Home from '@app/page';
import '@testing-library/jest-dom';
 
describe('Home', () => {
  it('renders a heading', () => {
    render(<Home />);
 
    const heading = screen.getByRole('heading', {
      name: /Hello world!/i,
    });
 
    expect(heading).toBeInTheDocument();
  });
});
```

This test instructs Jest to find the text "Hello world!" in any header tag.

Now let's run `yarn test`. It should fail.

```sh
Home
    ✕ renders a heading (176 ms)

  ● Home › renders a heading

    TestingLibraryElementError: Unable to find an accessible element with the role "heading" and name `/Hello world!/i`

    Here are the accessible roles:
...
```

As expected, the test failed. Don't worry, let's fix `app/page.tsx`:

```tsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-between p-24">
      <h1>Hello world!</h1>
    </main>
  )
}
```

Now, let's run yarn test again:

```sh
 PASS  tests/app/page.spec.tsx
  Home
    ✓ renders a heading (47 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
```

Perfect. We can now tesing the page with jest and [react testing library](https://testing-library.com/docs/react-testing-library/intro/).