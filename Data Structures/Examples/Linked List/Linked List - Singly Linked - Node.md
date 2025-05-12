# Singly Linked List - Node Implementation

![singly-linked-list-node.svg](_images/singly-linked-list-node.svg)

This is a simple implementation of a `ListNode` class for a `Singly Linked List` using a generic data type for the `value`. This `ListNode` class is can be used as a self-contained `Linked List` or with a `LinkedList` wrapper class.

## Breakdown

* **Node Structure**:
  * The class defines a `value` property (of type `T`) and a `next` property (which is either another `ListNode` or `null`), which is standard for a singly linked list node.
* **Constructor**:
  * The constructor initializes the node with a given `value` and an optional `next` parameter (defaulting to `null`).

## TypeScript Implementation

```ts
/**
 * Represents a node in a Singly Linked List.
 */
export class ListNode<T> {
  /** The value or "data" contained in this node */
  public value: T;
  /** Reference to the next node in the list (or null) */
  public next: ListNode<T> | null;
  constructor(value: T, next: ListNode<T> | null = null) {
    this.value = value;
    this.next = next;
  }
}
```