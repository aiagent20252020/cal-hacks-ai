# Ultracite Code Standards

This project uses **Ultracite**, a zero-config preset that enforces strict code quality standards through automated formatting and linting.

## Quick Reference

- **Format code**: `pnpm dlx ultracite fix`
- **Check for issues**: `pnpm dlx ultracite check`
- **Diagnose setup**: `pnpm dlx ultracite doctor`

Biome (the underlying engine) provides robust linting and formatting. Most issues are automatically fixable.

---

## Core Principles

Write code that is **accessible, performant, type-safe, and maintainable**. Focus on clarity and explicit intent over brevity.

### Type Safety & Explicitness

- Use explicit types for function parameters and return values when they enhance clarity
- Prefer `unknown` over `any` when the type is genuinely unknown
- Use const assertions (`as const`) for immutable values and literal types
- Leverage TypeScript's type narrowing instead of type assertions
- Use meaningful variable names instead of magic numbers - extract constants with descriptive names

### Modern JavaScript/TypeScript

- Use arrow functions for callbacks and short functions
- Prefer `for...of` loops over `.forEach()` and indexed `for` loops
- Use optional chaining (`?.`) and nullish coalescing (`??`) for safer property access
- Prefer template literals over string concatenation
- Use destructuring for object and array assignments
- Use `const` by default, `let` only when reassignment is needed, never `var`

### Async & Promises

- Always `await` promises in async functions - don't forget to use the return value
- Use `async/await` syntax instead of promise chains for better readability
- Handle errors appropriately in async code with try-catch blocks
- Don't use async functions as Promise executors

### React & JSX

- Use function components over class components
- Call hooks at the top level only, never conditionally
- Specify all dependencies in hook dependency arrays correctly
- Use the `key` prop for elements in iterables (prefer unique IDs over array indices)
- Nest children between opening and closing tags instead of passing as props
- Don't define components inside other components
- Use semantic HTML and ARIA attributes for accessibility:
  - Provide meaningful alt text for images
  - Use proper heading hierarchy
  - Add labels for form inputs
  - Include keyboard event handlers alongside mouse events
  - Use semantic elements (`<button>`, `<nav>`, etc.) instead of divs with roles

### Error Handling & Debugging

- Remove `console.log`, `debugger`, and `alert` statements from production code
- Throw `Error` objects with descriptive messages, not strings or other values
- Use `try-catch` blocks meaningfully - don't catch errors just to rethrow them
- Prefer early returns over nested conditionals for error cases

### Code Organization

- Keep functions focused and under reasonable cognitive complexity limits
- Extract complex conditions into well-named boolean variables
- Use early returns to reduce nesting
- Prefer simple conditionals over nested ternary operators
- Group related code together and separate concerns

### Security

- Add `rel="noopener"` when using `target="_blank"` on links
- Avoid `dangerouslySetInnerHTML` unless absolutely necessary
- Don't use `eval()` or assign directly to `document.cookie`
- Validate and sanitize user input

### Performance

- Avoid spread syntax in accumulators within loops
- Use top-level regex literals instead of creating them in loops
- Prefer specific imports over namespace imports
- Avoid barrel files (index files that re-export everything)
- Use proper image components (e.g., Next.js `<Image>`) over `<img>` tags

### Framework-Specific Guidance

**Next.js:**
- Use Next.js `<Image>` component for images
- Use `next/head` or App Router metadata API for head elements
- Use Server Components for async data fetching instead of async Client Components

**React 19+:**
- Use ref as a prop instead of `React.forwardRef`

**Solid/Svelte/Vue/Qwik:**
- Use `class` and `for` attributes (not `className` or `htmlFor`)

---

## Testing

- Write assertions inside `it()` or `test()` blocks
- Avoid done callbacks in async tests - use async/await instead
- Don't use `.only` or `.skip` in committed code
- Keep test suites reasonably flat - avoid excessive `describe` nesting

## When Biome Can't Help

Biome's linter will catch most issues automatically. Focus your attention on:

1. **Business logic correctness** - Biome can't validate your algorithms
2. **Meaningful naming** - Use descriptive names for functions, variables, and types
3. **Architecture decisions** - Component structure, data flow, and API design
4. **Edge cases** - Handle boundary conditions and error states
5. **User experience** - Accessibility, performance, and usability considerations
6. **Documentation** - Add comments for complex logic, but prefer self-documenting code

---

Most formatting and common issues are automatically fixed by Biome. Run `pnpm dlx ultracite fix` before committing to ensure compliance.

<<<<<<< HEAD
## Cloud Agent

This repo requires Node.js `>=24`. The Cloud Agent image keeps Node 24 at `/usr/local/lib/nodejs/bin`. Put that directory first on `PATH` before `pnpm`, `vite`, or `wrangler`. An older Node binary is also on `PATH` and will not satisfy `engines`.

The environment starts these local processes on boot:

- Control plane (`wrangler dev`): `http://127.0.0.1:8787`
- Dashboard: `http://127.0.0.1:5173`
- Landing: `http://127.0.0.1:5174`
- Docs: `http://127.0.0.1:3000`

`packages/control-plane/.dev.vars` is created from `.dev.vars.example` when it is missing. An empty `JWT_SECRET` is filled with the local value `local-dev-jwt-secret`, and an empty `ALLOWED_ORIGINS` is filled with `http://127.0.0.1:5173,http://localhost:5173` so the dashboard can call the worker. Mint a dashboard token with `pnpm dev:token`. Model, GitHub, Devin, and Modal keys in that file are optional for health checks, session lists, and benchmark task lists.

`pnpm install` runs `lefthook install`. Cloud agents set `core.hooksPath`, and lefthook exits if that value is present. The environment install unsets it only for the install, then restores it. Do not leave `core.hooksPath` unset.
=======
## Local development

The repo requires Node.js 24 (`engines.node`) and pnpm 10.33.4. pnpm 10 skips dependency build scripts unless they are approved; set `dangerously-allow-all-builds` so `esbuild` and `workerd` can install:

```bash
pnpm config set --global dangerously-allow-all-builds true
pnpm install --frozen-lockfile
```

`pnpm install` runs `lefthook install`. If `core.hooksPath` points at the agent hook directory, unset that local git config for the install and restore it afterward.

Copy `packages/control-plane/.dev.vars.example` to `packages/control-plane/.dev.vars` and set `JWT_SECRET`. Set `ALLOWED_ORIGINS` to `http://localhost:5173` so the dashboard can call the worker. Apply the local D1 schema with `pnpm db:apply:local`, then mint a dashboard token with `pnpm dev:token`.

| Process | Command | Port |
| --- | --- | --- |
| Control plane | `pnpm dev:worker` | 8787 |
| Dashboard | `pnpm dev:dashboard` | 5173 |
| Landing | `pnpm dev:landing` | 5174 |
| Docs | `pnpm dev:docs` | 3000 |
| Docker shim | `pnpm dev:docker-shim` | 8000 |

The docker shim needs a running Docker daemon and `SHIM_SECRET` equal to `MODAL_SHIM_SECRET` in `.dev.vars`. Point `MODAL_SHIM_URL` at `http://127.0.0.1:8000`. `pnpm dev:modal` needs a Modal account and is not part of the default local stack. Model, GitHub, and Devin keys in `.dev.vars` are only required for live agent runs.
>>>>>>> origin/main


