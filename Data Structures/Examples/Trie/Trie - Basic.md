# Basic Trie Implementation

![trie-basic-example.svg](../_images/trie-basic-example.svg)

This is a basic implementation of a `Trie` data structure. We define a `TrieNode` class to represent the nodes of the trie and a wrapper class `BaseTrie` to represent the actual trie structure. This implementation contains minimal methods and a print function for debugging.

## Breakdown

- **Root Node**: The `root` node is initialized when the trie itself is instantiated
- **Insert**: Inserts a new word into the trie
  - Checks for null inputs
  - Iterates over the input word and adds each character (if it doesn't already exist)
  - Marks the final node as `terminal`
- **Find**: Returns the `terminal` node of the input word within the trie (or null if no such word is in the trie)
  - Checks for null inputs
  - Iterates over the input word and for each character checks that each character exists
  - Returns true if the node representing the final letter in the input word is `terminal`
- **Print**: Logs all the **valid** words currently in the trie to the console
  - Uses a recursive inner function to iterate over each node and log its value
- **Create From Words**: A `static` method that creates and returns a new instance of the `BaseTrie` class (or any class that derives from it) with a pre-inserted set of words. This is mainly built to save time when creating test cases.
  - Creates a new instance of the current class
  - Iterates over all the provided words and calls `insert()` on each of them to add them to the trie

## TypeScript Implementation

### Dependencies
* [TrieNode](Trie%20-%20Node.md) - The node class

```ts
import { TrieNode } from "./trie-node";

class BaseTrie {
  private _root: TrieNode | null;
  /** The root {@link TrieNode} of this trie */
  public get root(): TrieNode | null { return this._root; }
  public set root(value: TrieNode) { this._root = value; }
  
  constructor() {
    this._root = new TrieNode();
  }
  
  /**
   * Inserts the given `word` into the trie
   * @param word The word to add into the trie
   */
  public insert(word: string): void {
    // Make sure the input word isn't empty
    if (word.length === 0) return;
    // Create a holder variable for the last visited node
    let current = this._root;
    for (let i = 0; i < word.length; i++) {
      if (!current.hasChild(word[i])) {
        // Create a new node if it doesn't exist
        current.setChild(word[i], new TrieNode());
      }
      // Update the current pointer
      current = current.getChild(word[i]);
    }
    // At this point, `current` points to the last letter in the word
    current.isTerminal = true;
  }
  
  /**
   * Searches the trie for the given word and either returns the terminal
   * node representing the last character of the word if found, or null if
   * no such word exists within the current trie.
   * @param word The word to search for
   * @returns Terminal node of the last character in the word, or null
   */
  public find(word: string): TrieNode | null {
    // Make sure the input word isn't empty
    if (word.length === 0) return null;
    // Create a holder variable for the last visited node
    let current = this._root;
    for (let i = 0; i < word.length; i++) {
      if (!current.hasChild(word[i])) {
        return null;
      }
      current = current.getChild(word[i]);
    }
    // At this point, `current` points to the last letter in the word
    // Check that the last letter is terminal
    if (current.isTerminal) {
      return current;
    } else {
      // The word doesn't exist in this trie
      return null;
    }
  }
  
  /**
   * Logs all the valid words currently in the trie to the console on
   * separate lines. Useful for debugging.
   */
  public print(): void {
    printHelper(this._root, "");
    
    /**
     * Helper function that traverses the trie recursively. The
     * `word` parameter accumulates the characters for the current
     * path.
     * @param node The current node being traversed
     * @param word The current accumulated prefix/word
     */
    function printHelper(node: TrieNode, prefix: string): void {
      // If this node marks the end of a valid word, output it.
      if (node.isTerminal) {
        console.log(prefix);
      }
      // Iterate over the children. Each key is a character code.
      // Append the corresponding character to the current word and continue.
      for (const child in node.children) {
        printHelper(node.getChild(child), prefix + child);
      }
    }
  }
  
  /**
   * Returns a new Trie with the given `words`.
   * @param words Words to be inserted into the new trie
   * @returns A new trie with the given `words`
   */
  public static fromWords<T extends typeof BaseTrie>(...words: string[]): InstanceType<T> {
    const trie = new this() as InstanceType<T>;
    // Insert all the words into the trie
    for (const word of words) {
      trie.insert(word);
    }
    return trie;
  }
}

const myTrie = BaseTrie.fromWords(
  "a", "apple", "ate", "apt", "app",
  "bat", "bath", "batch", "baton",
  "dog", "doll", "dollar",
  "lie"
);

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
```