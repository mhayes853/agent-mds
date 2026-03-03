# TypeScript
You are working within a typescript project. Below are common anti-patterns that agents tend to follow, and what you should do instead. Make sure to generate code in correspondence with examples listed as GOOD or OK, and avoid generating code listed as a BAD example.

## Prefer `AbortController` to `removeEventListener`

Most `addEventListener` APIs allow you to pass an `AbortSignal` to cancel the event listener, instead of the old way of cancelling via the function reference to `removeEventListener`. The `AbortSignal` way enables you to cancel multiple event listeners at once as well.

```ts
// BAD

window.addEventListener('my-event', func);
window.addEventListener('my-event2', func);
window.addEventListener('my-event3', func);
window.removeEventListener('my-event', func);
window.removeEventListener('my-event2', func);
window.removeEventListener('my-event3', func);

// GOOD
const controller = new AbortController();
window.addEventListener('my-event', func, { signal: controller.signal });
window.addEventListener('my-event2', func, { signal: controller.signal });
window.addEventListener('my-event3', func, { signal: controller.signal });

controller.abort();
```

## Don't Prefix Functions That Only Retrieve Values With Verbs Like "get":

```typescript
// BAD
const getUserEmail = (): string => user.email;
const getProductName = (): string => product.name;

// GOOD
const userEmail = (): string => user.email;
const productName = (): string => product.name;
```

## Prefer Arrow Functions

Prefer arrow functions over the `function` keyword for all function declarations. Use the pattern:

```typescript
// BAD
function myFunction() { 
  // ...
}

// GOOD
const myFunction = () => {
  // ...
};
```

This applies to both exported functions and internal helper functions.

## No Re-exports

Re-exporting code is **only permitted in barrel files** (`index.ts`). Do not re-export functions from any other non-barrel files.

This keeps the code modular and makes import paths predictable.
