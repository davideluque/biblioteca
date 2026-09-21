# Declaration order in TypeScript and JavaScript

Use this reference when choosing or changing file order in TypeScript or JavaScript. TypeScript allows several readable layouts; runtime initialization and configured lint rules impose separate constraints.

## A flexible module layout

After required directives and imports, expose the contract and main operation before its implementation details:

```ts
export interface Person {
  firstName: string;
  lastName: string;
}

export function displayName(person: Person): string {
  return normalizeWhitespace(`${person.firstName} ${person.lastName}`);
}

function normalizeWhitespace(value: string): string {
  return value.trim().replace(/\s+/g, " ");
}
```

Here the reader encounters the public operation before the normalization algorithm. The helper could appear above the caller in a project that requires definitions before use. Neither arrangement requires extracting more functions or changing the public API.

Keep types used only by a helper near that helper. Export types because they belong to the public contract, not to justify placing them at the top. Small pure constants can go near their users; module-level initializers that call functions or create objects must also respect execution dependencies.

## Distinguish source position from execution time

| Construct | Ordering constraint |
| --- | --- |
| `type` and `interface` | References may precede declarations within their scope. These declarations are erased and have no runtime initialization. |
| Function declaration | Its binding is initialized before ordinary statements in its scope run. It can be called above its declaration, but values read by its body must already be ready. |
| `const` or `let`, including an assigned arrow function | Access before initialization is invalid. A function body higher in the file may refer to the binding if that body executes after initialization. |
| Class | The runtime value must be initialized before construction or use as a base class. Referring to a class as a type is a different operation. |
| Enum | A regular enum also creates a runtime value; do not treat all TypeScript declarations as erased types. Preserve runtime dependency order. |

This example works, including the forward reference to an arrow function:

```ts
function statusMessage(): string {
  return decorate("ready");
}

const decorate = (value: string): string => `[${value}]`;

export const message = statusMessage(); // "[ready]"
```

The following standalone counterexample fails at runtime even though `statusMessage` is a function declaration:

```ts
export const message = statusMessage(); // ReferenceError

function statusMessage(): string {
  return decorate("ready");
}

const decorate = (value: string): string => `[${value}]`;
```

The first call reaches `decorate` before its initializer runs. Moving the invocation after initialization fixes this; moving only the function declaration does not. A compiler may accept an indirect early access like this, so follow the actual call path. Circular imports can also invoke exported functions before a module finishes initializing.

## Preserve execution-sensitive order

- Keep required directives in their required positions. Imports-first is a convention, not permission to move a framework directive below an import.
- Follow existing import grouping. Imported modules can execute effects, including through named imports; do not alphabetize dependency evaluation blindly. Preserve intentional setup order and verify import sorting tools respect it.
- Keep executable registration, startup, and initialization calls in dependency order. Putting a script's startup call near the end can help, but does not by itself solve dependencies between modules.
- Within classes, arrange ordinary methods for reading while preserving field initializer and static initialization order. Inspect computed names or decorators before treating a member move as inert. Keep overload signatures with their implementation and preserve their order.
- Do not convert an arrow function to a function declaration solely to bypass ordering constraints. That can change lexical `this`, binding behavior, or typing. Use the repository's existing declaration style where practical.

Check the active lint configuration before changing layout. A `no-use-before-define` rule can intentionally reject a legal function forward reference. Its options distinguish functions, runtime values, and type references; use the version and configuration present in the project. Do not add or relax lint rules as an incidental part of reordering code.

## References

- [TypeScript: Variable Declaration](https://www.typescriptlang.org/docs/handbook/variable-declarations.html#block-scoping): initialization timing and captures of later bindings.
- [TypeScript: Declaration Merging](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#basic-concepts): which declarations create types and runtime values.
- [TypeScript: Modules](https://www.typescriptlang.org/docs/handbook/2/modules.html): runtime imports and type-only imports.
- [TypeScript: Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html): fields, constructors, overloads, and initialization order.
- [ESLint: no-use-before-define](https://eslint.org/docs/latest/rules/no-use-before-define): hoisting and configurable declaration-order checks.
