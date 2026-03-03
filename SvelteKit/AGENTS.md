# SvelteKit App
You are working in a SvelteKit app with typescript. Below are common anti-patterns that agents tend to follow, and what you should do instead. Make sure to generate code in correspondence with examples listed as GOOD or OK, and avoid generating code listed as a BAD example.

## Always Use Svelte 5 Runes Syntax (Never Use Svelte 4 Patterns)

| Category          | ✅ Svelte 5                            | ❌ Svelte 4 (DO NOT USE)  |
| ----------------- | -------------------------------------- | ------------------------- |
| **State**         | `let count = $state(0);`               | `let count = 0;`          |
| **Derived**       | `const doubled = $derived(count * 2);` | `$: doubled = count * 2;` |
| **Effects**       | `$effect(() => { ... });`              | `$: { ... }`              |
| **Props**         | `let { foo, bar } = $props();`         | `export let foo;`         |
| **Events**        | `onclick={handler}`                    | `on:click={handler}`      |
| **Custom events** | Pass callback props: `onsave={fn}`     | `createEventDispatcher`   |
| **Slots**         | `{@render children()}`                 | `<slot />`                |

## Use `$derived.by` For Multi-Line Derived Values

If a `$derived` value takes more than a single expression to compute, then use `$derived.by` instead.

```ts
// BAD
const value = $derived(expression());
const value2 = $derived(value + something);

// GOOD
const value = $derived.by(() => {
	const value = expression();
	return value + something;
});
```
