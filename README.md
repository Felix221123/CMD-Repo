# Command Repository

This repository stores all the commands I frequently use as a developer (the ones I always forget 🥹).

## Command List

Here is a list of them:

* `npx create-vite@latest` - Used for a new React/Angular/Svelte project.
* `npm i` - Used for installing all the node modules along with `packages.json`.

### Required Scripts Commands in package.json

Here is a list of scripts to run in `package.json` (both frontend and backend):

```json
"predeploy": "npm run build",
"deploy": "gh-pages -d build"
```
These scripts are used to build projects to production and deploy them. Install `gh-pages` first before using the deploy scripts.

### Adding Tailwind CSS to a Project

How to add Tailwind CSS to your project:

1. Install the following in your project directory:

    ```bash
    npm install -D tailwindcss postcss autoprefixer
    npx tailwindcss init -p
    ```

2. Change your content inside your Tailwind config to:

    ```javascript
    content: [
        "./index.html",
        "./src/**/*.{js,ts,jsx,tsx}",
    ],
    ```

3. Add this inside your `index.css` file:

    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

4. Add this to your scripts object in your `package.json`:

    ```json
    "css": "npx tailwindcss -i ./src/index.css -o ./public/output.css --watch"
    ```

### Setting up Vitest (Tools for Testing React Apps) in Your Project

#### Installation

This module should be installed as one of your project's `devDependencies`:

```shell
# with npm
npm install --save-dev vitest-dom
# yarn
yarn add --dev vitest-dom
# pnpm
pnpm add --dev vitest-dom
```

Install jsdom and other necessary packages:

```shell
npm install --save-dev jsdom @testing-library/react @testing-library/jest-dom
```

Import the matchers from `vitest-dom/matchers` once (preferably in your [tests setup file](#)), then pass them to Vitest's `expect.extend` method:

```typescript
import * as matchers from "vitest-dom/matchers";
import { expect, afterEach } from "vitest";
import { cleanup } from "@testing-library/react";

expect.extend(matchers);

// vitest-setup.ts
import "vitest-dom/extend-expect";

// runs a cleanup after each test case (e.g. clearing jsdom)
afterEach(() => {
  cleanup();
});
```

You can also configure Vitest to use `vitest-dom` in your test files or `tsconfig.json`:

1. In your test file via a reference directive:

   ```typescript
   /// <reference types="vitest-dom/extend-expect" />
   ```

2. In your `tsconfig.json` via the `types` compiler option:

   ```json
   {
     "compilerOptions": {
       "types": ["vitest-dom/extend-expect"]
     }
   }
   ```

Ensure your Vite config is set up correctly:

```javascript
/// <reference types="vitest" />
/// <reference types="vite/client" />

import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    css: true,
    setupFiles: './src/test/setup.ts',
  },
});
```

Install the UI version of the test results for a clear visual output:

```shell
npm install --save-dev @vitest/ui
```

Add this to your `package.json` script objects to run your tests:

```json
"scripts": {
  "test:ui": "vitest --ui"
}
```

Import these from the React Testing Library to begin your tests:

```javascript
import { screen, render } from "@testing-library/react";
```

Now you're all set to use `vitest-dom` in your project. Happy testing!



### Setting up Cypress Testing (Tools for React Apps) in Your Project

1. Install Cypress:

    ```bash
    npm install cypress --save-dev
    ```

2. Add this to your scripts object in your `package.json`:

    ```json
    "cy:open": "cypress open"
    ```

3. Choose Component testing and you be asked to choose your bundler, Cypress will automatically detect the bundler you are using for your react application and you can continue from there

4. Create a new component from the create new component and choose a new component to start with in your project

### Cypress TypeScript Configuration

To configure Cypress with TypeScript, follow these steps:

5. **Update `tsconfig.json`**:
   Include the Cypress folder in your TypeScript configuration to eliminate errors:
   ```json
   "include": ["src", "cypress"],
   ```

6. **Add Cypress Commands**:
   If the above step doesn't resolve the errors, add the mount to the Cypress interface by modifying the `commands.ts` file:
   ```typescript
   import { mount } from 'cypress/react';

   declare global {
     namespace Cypress {
       interface Chainable {
         mount: typeof mount;
       }
     }
   }

   Cypress.Commands.add('mount', mount);
   ```

7. **Handle Custom Commands**:
   To resolve similar errors for custom commands, update the `commands.ts` file:
   ```typescript
   import { mount } from 'cypress/react';

   declare global {
     namespace Cypress {
       interface Chainable {
         mount: typeof mount;
         dataCy(value: string): Chainable<JQuery<HTMLElement>>;
       }
     }
   }

   Cypress.Commands.add('mount', mount);

   Cypress.Commands.add('dataCy', (value) => {
     return cy.get(`[data-cy=${value}]`);
   });
   ```

8. **Fix Augmentations Errors**:
   To fix errors related to global scope augmentations, create a `cypress.d.ts` file in the root directory (or preferred location) of your project:
   ```typescript
   import { mount } from 'cypress/react';

   declare global {
     namespace Cypress {
       interface Chainable {
         mount: typeof mount;
         dataCy(value: string): Chainable<JQuery<HTMLElement>>;
       }
     }
   }
   ```

9. **Update `tsconfig.json`**:
   Point to the new typing file by updating the includes:
   ```json
   "include": ["src", "cypress", "./cypress.d.ts"],
   ```

