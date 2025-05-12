# Linked List - Search for Node

## Breakdown

This function searches for a node with a specific value within a `Singly Linked List`. The function assumes that the list has distinct values. Otherwise, the first occurrence of the value is returned.

1. **Initialize Pointer**
    - Let `current` point to the `head` of the list.
2. **Traversal Loop**
    - While `current` is not `null`:
        - Compare `current.value` to the target `value`.
        - If they match, **return** `current` immediately.
        - Otherwise, set `current = current.next` to advance.
3. **Miss Case**
    - If the loop ends without finding a match, **return** `null`.

## TypeScript Implementation

### Dependencies
* [Linked List - Singly Linked - Node.md](Linked%20List%20-%20Singly%20Linked%20-%20Node.md) - The generic node class for the `LinkedList`

### Edge Cases
* Empty list
* Value does not exist
* Duplicate values

### Complexity
* **Time**: $O(n)$ - where $n$ is the number of nodes in the list
* **Space**: $O(1)$

```ts
import { ListNode } from "./dsa/linked-lists/singly-linked-list";
class LinkedListNode extends ListNode<number> { }

/**
 * Returns the first occurrence of the LinkedListNode with the given
 * `value` within the provided Linked List.
 * @param head The current head node of the list
 * @param value The value to get the node for
 * @returns The node representing the first occurrence of the value
 */
function findNode(head: LinkedListNode | null, value: number): LinkedListNode | null {
  // Iterate through the list and find the node
  let node = head;
  while (node !== null) {
    // Check if this is the node we're looking for
    if (node.value === value) {
      return node;
    }
    // Otherwise, move to the next node
    node = node.next;
  }
  // If we reach this point, there is no such node
  return null;
}
```