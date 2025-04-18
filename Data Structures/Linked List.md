# Linked List

![linked-list.svg](_images/linked-list.svg)

A `linked list` is a data structure that represents a sequence of nodes.  It is a linear collection of data elements whose order is not given by their physical placement in memory, as opposed to arrays, where data is stored in contiguous blocks of memory. Instead, each element contains an address of the next element. It is a data structure consisting of a collection of nodes which together represent a sequence.

![linked-list-in-memory.svg](_images/linked-list-in-memory.svg)

In its most basic form, each node contains: data, and a reference (in other words, a link) to the next node in the sequence.

### Key Terms
- **Head**: The first node in the linked list
- **Tail**: The last node in the linked list. In Singly Linked Lists, the tail's `next` node will be `null`

![linked-list-head-tail.svg](_images/linked-list-head-tail.svg)

___

## Types of Linked List

### Singly Linked List

In a **singly linked list**, each node points to the `next` node in the linked list. The `tail` node in the list points to `null`.

![singly-linked-list.svg](_images/singly-linked-list.svg)

> For most interviews, the **singly linked list** is the primary focus unless specified otherwise.

### Doubly Linked List

A **doubly linked list** gives each node pointers to both the `next` and `prev` node. The `prev` pointer of the `head` node and the `next` pointer of the `tail` node point to `null`.

![doubly-linked-list.svg](_images/doubly-linked-list.svg)

### Circular Linked List

#### Circular Singly Linked List
A **singly linked list** where the `tail` node points back to the `head` node.

![circular-linked-list.svg](_images/circular-linked-list.svg)

#### Circular Doubly Linked List

There is a **circular doubly linked list** variant where the `prev` pointer of the `head` node points to the `tail` node and the `next` pointer of the `tail` node points to the `head` node. Useful for applications that require a circular traversal (e.g., round-robin scheduling).

![circular-linked-list-dbl.svg](_images/circular-linked-list-dbl.svg)
___

## Time/Space Complexity

### Time Complexity

- **Read**: $O(n)$
- **Search**: $O(n)$ - You need to iterate through every element to find the `kᵗʰ` element
- **Insert**: $O(1)$
- **Delete**: $O(1)$

### Space Complexity

$O(n)$ because each element requires extra memory for the pointer in addition to the data.
___

## Edge Cases

### Empty List

Operations like deletion or search should handle cases where the list is empty.

### Single-Element List

Removing or inserting must appropriately update the head (and tail if applicable).

### Insertion at Head and Tail

Ensure that if you’re inserting at the beginning or end, your head (or tail) pointer is updated.

### Deletion of a Specific Node

When deleting, especially in a singly linked list, you need to update the pointer from the previous node; careful handling is required if the node to delete is the head.

### Cycle Detection

Although not always part of a basic implementation, be aware of the potential for cyclic references (e.g., in corrupted lists) and algorithms like Floyd’s Cycle Detection (fast/slow pointers) that address this.

### Memory Leaks (in languages without garbage collection)

Ensure that nodes are properly de-referenced, though this is less of a concern in languages like TypeScript/JavaScript.
___

## When to Use

### Frequent Insertions/Deletions

When you need to frequently add or remove elements (especially in the middle of the list) where shifting array elements would be costly.

### Implementing Stacks and Queues

Their structure lends itself well to LIFO (stack) and FIFO (queue) operations.

### Memory Management Situations

When you want to allocate memory on-the-go and reduce the overhead of contiguous memory allocation (arrays).

### Dynamic Data Structures

When the size of the dataset frequently changes, using a linked list can be more efficient in terms of reallocation.

### Problems Involving Sequential Access

Interview questions that ask you to reverse a list, merge sorted lists, or remove the nth element from the end are all based on linked list manipulations.
___

## Example Implementation

#### **Singly Linked List**
```ts
// Very basic implementation of a singly linked list
class Node {
  public next: Node | null = null;
  public data: number;
  
  constructor(d: number) {
    this.data = d;
  }
  
  appendToTail(d: number): void {
    const end: Node = new Node(d);
    let n = this;
    while (n != null) {
      n = n.next;
    }
    n.next = end;
  }
}

// Adds a new node to the head of the linked list
function insertAtHead(head: Node, d: number): Node {
  const node: Node = new Node(d);
  node.next = head;
  return node; // Node becomes the new head
}

// Removes a node from a singly linked list and returns the new head
function deleteNode(head: Node, d: number): Node {
  if (head === null || head === undefined) {
    return null;
  }
  // Start our search off at the head
  let n = head;
  
  // Check if the head contains the data we are trying to delete
  if (n.data === d) {
    return head.next; // Head has moved
  }
  
  // Move up the list until we find the node we are looking for
  while (n.next !== null) {
    if (n.next.data === d) {
      n.next = n.next.next; // Update the `next` node to the node after the deleted node
      return head; // Head has not changed
    }
    n = n.next;
  }
  
  // If we reached this point, there isn't a node with the given data
  return head;
}

// Checks to see if a node with the given data exists
function hasNode(head: Node, d: number) {
  if (head === null) {
    return false;
  }
  
  let n: Node = head;
  if (n.data === d) {
    return true; // The head node is the one we are looking for
  }
  
  while (n.next !== null) {
    if (n.next.data === d) {
      return true;
    }
    n = n.next;
  }
  
  return false;
}

// Returns the node with the given data
function getNode(head: Node, d: number): Node {
  if (head === null) {
    return null;
  }
  
  let n: Node = head;
  if (n.data === d) {
    return head;
  }
  
  while (n.next !== null) {
    if (n.next.data === d) {
      return n.next;
    }
    n = n.next;
  }
  // If we reached this point, then the node doesn't exist
  return null;
}

// Reverses a singly linked list using an iterative approach
function reverseLinkedList(head: Node): Node {
  let prev: Node = null;
  let current: Node = head;
  while (current !== null) {
    const next = current.next; // Hold ref to next node
    current.next = prev; // Set the current nodes' next to the prev node
    prev = current; // Set prev node to current node
    current = next; // Set current node to next node
  }
  // `prev` is now the head so we return that
  return prev;
}

// Reverses a singly linked list using a recursive approach
function reverseLinkedListRecursive(current: Node, prev: Node | null = null): Node {
  if (current === null) {
    return prev; // This will happen when we reach the end, prev is the new head
  }
  const next = current.next;
  current.next = prev;
  return reverseLinkedListRecursive(next, current);
}
```

This is the most basic way to implement a linked list in that it doesn't declare an actual `LinkedList` data structure, but rather accesses the linked list through a reference to the `head` node. This can be a dangerous approach, however, as multiple objects may need to reference the linked list and could fall out of sync with what the `head` node is. For interview questions, this is often the best approach because it takes less time to develop, and you know your “world” of operations never needs to know or change the `head` reference from multiple points.

___

## LeetCode Questions

- **Reverse Linked List** (LeetCode 206)
    - **Prerequisites:** Basic linked list manipulation; understanding of pointers.
    - **Topics:** Iterative and recursive solutions.

- **Linked List Cycle** (LeetCode 141)
    - **Prerequisites:** Knowledge of fast/slow pointer technique.
    - **Topics:** Cycle detection (Floyd’s Tortoise and Hare).

- **Merge Two Sorted Lists** (LeetCode 21)
    - **Prerequisites:** Familiarity with linked list traversal and merging techniques.
    - **Topics:** Merging two sorted linked lists, often utilizing dummy nodes.

- **Remove Nth Node From End of List** (LeetCode 19)
    - **Prerequisites:** Two pointers (fast/slow pointer) technique.
    - **Topics:** Handling edge cases (removing head, one-node list).

- **Intersection of Two Linked Lists** (LeetCode 160)
    - **Prerequisites:** Understanding of linked list traversal and handling where lists converge.
    - **Topics:** Two-pointer technique, potentially using extra data structures (hash set) in alternate solutions.

- **Swap Nodes in Pairs** (LeetCode 24)
    - **Prerequisites:** Solid grasp of pointer manipulation.
    - **Topics:** Reordering nodes in pairs.

- **Add Two Numbers** (LeetCode 2)
    - **Prerequisites:** Linked list traversal, handling carry over in addition.
    - **Topics:** Summing numbers represented in reverse order.
