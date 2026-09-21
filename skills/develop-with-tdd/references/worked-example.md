# Worked example: unique IDs in arrival order

This original example concerns an exported collection utility. Its contract is explicit: accept an array of case-sensitive string IDs, return each distinct ID once in first-appearance order, and leave the input unchanged. An empty input produces an empty result. Validation of other input types and performance targets are outside this example's contract.

The function's users need those observations; they do not need to know which collection or helper implements them.

## Scenario list and increments

List empty input, repeated IDs, interleaved IDs, case distinctions, and input preservation. Develop one scenario at a time; the complete tests below show the resulting artifact, not a requirement to write the entire suite before implementing anything.

An illustrative sequence:

| Next scenario | Why the previous implementation fails | Small next implementation |
| --- | --- | --- |
| Empty input returns an empty array | An initial placeholder returns `undefined` | Return an empty array. |
| Repeated `item-b` appears once | Always returning an empty array loses the ID | Return the first ID when present. |
| `item-b, item-a, item-b, item-c` preserves distinct arrival order | Returning only the first ID drops later distinct IDs | Keep the first occurrence of every ID. |

At each row, run the new test against the previous implementation, then run all tests accumulated so far after the change. The temporary implementations are steps toward satisfying the list; they are insufficient once later scenarios are required.

The final implementation also covers case distinctions and input preservation. When those checks are added, they can legitimately pass immediately. Inspect that they exercise their intended observations and retain them as protection.

## Final implementation and tests

Save as `unique-in-order.mjs`:

```js
export function uniqueInOrder(ids) {
  return [...new Set(ids)];
}
```

Save alongside it as `unique-in-order.test.mjs`:

```js
import assert from 'node:assert/strict';
import test from 'node:test';
import {uniqueInOrder} from './unique-in-order.mjs';

test('empty input produces an empty result', () => {
  assert.deepEqual(uniqueInOrder([]), []);
});

test('repeated copies of one ID produce one entry', () => {
  assert.deepEqual(uniqueInOrder(['item-b', 'item-b']), ['item-b']);
});

test('interleaved IDs retain their first-appearance order', () => {
  const input = ['item-b', 'item-a', 'item-b', 'item-c'];

  const result = uniqueInOrder(input);

  assert.deepEqual(result, ['item-b', 'item-a', 'item-c']);
});

test('IDs differing only in case remain distinct', () => {
  assert.deepEqual(uniqueInOrder(['A', 'a', 'A']), ['A', 'a']);
});

test('selecting unique IDs leaves the input unchanged', () => {
  const input = ['item-b', 'item-a', 'item-b'];

  uniqueInOrder(input);

  assert.deepEqual(input, ['item-b', 'item-a', 'item-b']);
});
```

Run with a Node.js version supporting its built-in test runner:

```sh
node --test unique-in-order.test.mjs
```

## What the checks establish

The expected arrays come directly from the declared examples. Computing expectations with another copy of the deduplication algorithm would weaken their independence. The tests observe the exported API without spying on `Set` or requiring a private helper.

An explicit iteration that accumulates unseen IDs in order can satisfy the same tests. That freedom is useful: internal restructuring should preserve the examples without changing their expectations.

To assess the tests' sensitivity, use an isolated scratch copy and try plausible faults: return all IDs without deduplication, sort the unique IDs, or deduplicate by mutating the input. Each violates a different declared behavior and should be rejected. Restore the valid implementation after the experiment. This is an optional check of the example, not a required mutation-testing stage for every task.

This utility has no external dependencies. These tests establish nothing about UI wiring, storage, delivery guarantees, invalid input types, or large-input performance. Choose an additional boundary only when the actual task introduces such a requirement.
