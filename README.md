# tsdown Watch Mode Issue Reproduction

This repository reproduces an issue where TypeScript declaration files (.d.ts) are not updated in tsdown watch mode.

## Issue Overview

When using tsdown with `--watch --dts` options to monitor file changes, JavaScript files (.js) are properly regenerated when TypeScript files are modified, but TypeScript declaration files (.d.ts) are not updated.

## Environment

- tsdown: `0.14.2`
  - (rolldown-plugin-dts: `0.15.9`)
- typescript: `5.9.2`
- Node.js: `24.1.0`

## Project Structure

```
tsdown-watch-repro/
├── src/
│   └── index.ts         # Test TypeScript file
├── dist/
│   ├── index.js         # Generated JavaScript file
│   └── index.d.ts       # Generated TypeScript declaration file
├── package.json
└── README.md
```

## Reproduction Steps

### 1. Install Dependencies

```bash
pnpm install
```

### 2. Start Watch Mode

```bash
pnpm watch
```

This command executes `tsdown --watch --dts`.

### 3. Reproduce the Issue

1. While watch mode is running, edit `src/index.ts`
2. For example, add a new property to the `ExampleType` interface:
   ```typescript
   export interface ExampleType {
     aaa: number;
     bbb: number;
     ccc: string;
     ddd: boolean; // Newly added
   }
   ```
3. Save the file

### 4. Check Results

- ✅ `dist/index.js` is properly updated
- ❌ `dist/index.d.ts` is not updated (remains with old type definitions)

## Expected Behavior

When TypeScript files are modified, both JavaScript files and TypeScript declaration files should be automatically updated.

## Actual Behavior

Only JavaScript files are updated, while TypeScript declaration files remain unchanged from their initial generation.

