# Reproduction for `create-next-app` Missing Next-Steps Commands

This repository demonstrates that `create-next-app` omits next-steps command instructions when creating projects from built-in templates.

## Steps to Reproduce

1. Clone this repository or run in any terminal:
   ```bash
   npx create-next-app@canary test-app --ts --app --skip-install
   ```
2. Observe the terminal output upon completion.

## Actual Output
```text
Creating a new Next.js app in .../test-app.

Using npm.

Initializing project with template: app 

Initialized a git repository.

Success! Created test-app at .../test-app
```
*(Process exits immediately with no further output)*

## Expected Output
```text
Success! Created test-app at .../test-app
Inside that directory, you can run several commands:

  npm run dev
    Starts the development server.

  npm run build
    Builds the app for production.

  npm start
    Runs the built app in production mode.

We suggest that you begin by typing:

  cd test-app
  npm run dev
```

## Root Cause
In `packages/create-next-app/create-app.ts`:
- `hasPackageJson` is initialized to `false`.
- It is only updated inside `if (example)`.
- In the `else` branch (where templates are installed), `hasPackageJson` remains `false`.
- Therefore, `if (hasPackageJson)` evaluates to `false` and skips printing the commands.
