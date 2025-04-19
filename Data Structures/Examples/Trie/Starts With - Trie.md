# Trie - Starts With

This is an example implementation of a `startsWith()` method within a `Trie` class. 

### Dependencies
* [TrieNode](Trie%20-%20Node.md) - The node class
* [BaseTrie](Trie%20-%20Basic.md) - Base Trie class with common functions (insert, find, print, etc.)

## Breakdown
- Checks for empty input string
- Iterates over the input word and check's that each letter in sequential order
- Unlike `find()` this method does not return `false` if the node representing the final letter is not `terminal` as we aren't looking for completed words.

## TypeScript Implementation

![trie-basic-example.svg](../_images/trie-basic-example.svg)

```ts
import { TrieNode } from "./trie-node";
import { BaseTrie } from "./trie";

class Trie extends BaseTrie {
  /**
   * Returns `true` if the given `prefix` exists in this trie. This differs from
   * the `find` function in that the node at the final letter of the word does
   * NOT need to represent a complete word.
   * @param prefix The prefix to search for in the trie
   * @returns True if the prefix exists in the trie, false if not.
   */
  public startsWith(prefix: string): boolean {
    // First, check if the string is empty
    if (prefix.length === 0) return false;
    
    // Create a variable to track the current node in the iteration
    let current = this.root;
    for (let i = 0; i < prefix.length; i++) {
      if (!current.hasChild(prefix[i])) {
        return false;
      }
      current = current.getChild(prefix[i]);
    }
    // If we got here, then all letters of the prefix are in the trie
    return true;
  }
}

const myTrie: Trie = Trie.fromWords(
  "a", "apple", "ate", "apt", "app",
  "bat", "bath", "batch", "baton",
  "dog", "doll", "dollar",
  "lie"
) as Trie;

myTrie.print();
// Output:
// a
// app
// apple
// apt
// ate
// bat
// bath
// batch
// baton
// dog
// doll
// dollar
// lie

console.log(myTrie.startsWith('batc')); // true
console.log(myTrie.find('batc')); // false
console.log(myTrie.startsWith('batch')); // true
console.log(myTrie.startsWith('lies')); // false
```