# Linked List - Delete Node

![delete-node.svg](_images/delete-node.svg)

## Breakdown
This function deletes a node from a `LinkedList` that has a specific value.

1. **Dummy Head**
    - Create a new node (`dummy`) whose `next` points to the original `head`.
    - Ensures deletion at the head is handled like any other node.
2. **Initialize Pointers**
    - `prev` starts at `dummy`.
    - `current` starts at the real `head`.
3. **Traverse & Delete**
    - While `current` is not `null`:
        - If `current.value === value`
            - Bypass `current` by setting `prev.next = current.next`.
        - Else
            - Advance `prev = current` to “keep” the node.
        - In both cases, advance `current = current.next`.
4. **Return New Head**
    - After the loop, `dummy.next` is the head of the modified list.

## TypeScript Implementation

### **Dependencies**
* [Linked List - Singly Linked - Node.md](Linked%20List%20-%20Singly%20Linked%20-%20Node.md) - The generic node class for the `LinkedList`

### Edge Cases
* **Empty List** - Handles an empty list (returns `null`)
* **Value does not exist**
* **Duplicate values** - Deletes any number of matching nodes in a row
* **Deleting the `head` node** - Correctly deletes the original head if it matches

### Complexity
* **Time**: $O(n)$ - one pass through the list of length `n`
* **Space**: $O(1)$ - only a couple of extra pointers and a `dummy` node

```ts
import { ListNode } from "./dsa/linked-lists/singly-linked-list";
class LinkedListNode extends ListNode<number> {}

/**
 * Deletes all occurrences the LinkedListNode with the given `value`
 * from the Linked List . The resulting head node of the list is
 * returned after the operation is complete.
 * @param head The current head node of the list
 * @param value The value to delete
 * @returns The resulting head node of the list
 */
function deleteNode(head: LinkedListNode | null, value: number): LinkedListNode | null {
  // Create dummy node at the head of the list to easily account for 
  // head node updates.
  const dummy = new LinkedListNode(0, head);
  
  // Store reference to previous iterated node
  let prev = dummy;
  // Store reference to the current iterated node
  let current = head;
  
  // Traverse the list and remove all nodes that match the value
  while (current !== null) {
    // Check if the current node needs to be removed
    if (current.value === value) {
      // Update the previous node's next pointer to remove the node
      prev.next = current.next;
    } else {
      // If not a match, update the prev pointer
      prev = current;
    }
    // Update the current pointer to the next node
    current = current.next;
  }
  
  // Return the dummy head node's next pointer as the head node
  return dummy.next;
}
```