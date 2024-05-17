# COMMAND REPOSITORY
 * This repository stores all(most of them, lets say the ones i always i forget 🥹) the commands I use as a developer.



## COMMAND LIST
Here is a list of them

 * npx create-vite@latest  - Used for a new react/angular / svelte project
 * npm i  - Used for installing all the node modules along with packages.json
 * 



### Required Scripts Commands in package.json
Here is a list of scripts to run in package.json(both frontend and backend)

* "predeploy" : "npm run build",
    "deploy": "gh-pages -d build",  - used to build projects to production and deploy them, install gh-pages first before using the deploy scripts
 * 

#### Adding tailwindcss to project
How to add tailwindcss to your project

• Install the following in your project directory
 * npm install -D tailwindcss postcss autoprefixer
 * npx tailwindcss init -p

• Change your content inside your tailwind config to
 *    content: [
        "./index.html",
        "./src/**/*.{js,ts,jsx,tsx}",
    ],

• Add this inside your index.css file
 *  @tailwind base;
    @tailwind components;
    @tailwind utilities;

• Add this to your scripts object in your package.json
 * "css": "npx tailwindcss -i ./src/index.css -o ./public/output.css --watch" 



#### Setting up vitest (Tools for testing your Apps) in your Project
Here is a list of install commands used to set up your vitest, jsdom for application

 * npm i -D vitest - Use this to install vitest
 * "test":"vitest" - add this to your scripts object in your package.json
 * npm i -D jsdom @testing-library/react @testing-library/jest-dom - Use this install jest dom and some other packages
 * Make sure your vite config looks exactly like this
   /// <reference types="vitest" />
   /// <reference types="vite/client" />

    import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'

    // https://vitejs.dev/config/
    export default defineConfig({
    plugins: [react()],
    test: {
        globals: true,
        environment: 'jsdom',
        css: true,
        setupFiles:'./src/test/setup.ts'
    }
    })
 * Add the types key to your compiler options in your tsconfig.json
    "types": ["vitest/globals"],
 * Now create a folder in your src folder and and name it test and add a file named setup.ts in it, inside the file paste this 
 import '@testing-library/jest-dom'
 * npm i -D @vitest/ui - You can use this to install the ui version of the test results (gives out the clear version)
 * "test:ui" : "vitest --ui" - Add this to your package.json script objects to run your test





