```markdown
# my-skills Development Patterns

> Auto-generated skill from repository analysis

## Overview
This skill outlines the core development patterns and conventions used in the `my-skills` TypeScript repository. It covers file organization, coding style, import/export patterns, and testing practices to ensure consistency and maintainability across the codebase.

## Coding Conventions

### File Naming
- Use **kebab-case** for all file names.
  - Example:  
    ```
    user-profile.ts
    data-utils.ts
    ```

### Import Style
- Use **relative imports** for modules within the project.
  - Example:
    ```typescript
    import { fetchData } from './data-utils';
    ```

### Export Style
- Use **named exports** for all exported functions, types, or constants.
  - Example:
    ```typescript
    // In data-utils.ts
    export function fetchData() { ... }
    export const API_URL = '...';
    ```

### Commit Patterns
- Commit messages are freeform, with no strict prefixing.
- Average commit message length is about 56 characters.

## Workflows

_No automated workflows detected in this repository._

## Testing Patterns

- Test files follow the `*.test.*` naming convention.
  - Example:  
    ```
    user-profile.test.ts
    ```
- The specific testing framework is not detected, but tests are likely colocated with the code they test or in the same directory.

#### Example Test File
```typescript
// user-profile.test.ts
import { getUserProfile } from './user-profile';

describe('getUserProfile', () => {
  it('returns user profile data', () => {
    // test implementation
  });
});
```

## Commands

| Command | Purpose |
|---------|---------|
| /test   | Run all test files matching `*.test.*` |
```