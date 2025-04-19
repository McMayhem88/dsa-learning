# Trie Node Implementation

This is a simple class implementation to represent a node within a `Trie`. The class uses a `Record` to store the `string` character and `TrieNode` pairs. It includes helper functions to easily handle common functions on the child nodes.

## Breakdown

- Properties
  - **Children**: `Record<string, TrieNode>` - Child nodes stored using characters as keys.
  - **Is Terminal**: `boolean` - Determines if this node marks the end of a valid word within the trie.
  - **Count**: `number` - The number of child nodes for this node
- Methods
  - **Get/Set Child**: Gets/Sets the value of the `TrieNode` child for the given character
  - **Has Child: Returns** `true` if the node has a child node for the given character
  - **Remove Child**: Removes the `TrieNode` child for the given character

### TypeScript Implementation

```ts
/**
 * Simple class for trie nodes
 */
class TrieNode {
  private _children: Record<string, TrieNode> = {};
  /** Child nodes stored using character  */
  public get children(): Record<string, TrieNode> { return this._children; }

  private _count = 0;
  /** The number of child nodes for this node */
  public get count(): number {
    return this._count;
  }

  /** Determines if this node marks the end of a valid word */
  public isTerminal = false;

  /**
   * Returns the {@link TrieNode} child for the given `char`
   * @param char Char to get the TrieNode for
   * @returns The TrieNode of the char if found, null if not
   */
  public getChild(char: string): TrieNode | null {
    if (char in this._children) {
      return this.children[char];
    }
    return null;
  }

  /**
   * Set's the {@link TrieNode} at the given `char` to the given 
   * `node` value.
   * @param char Char to set
   * @param node TrieNode to set at the `char`
   */
  public setChild(char: string, node: TrieNode): void {
    if (!(char in this._children)) {
      this._count++;
    }
    this.children[char] = node;
  }

  /**
   * Returns true if the given `char` exists as a child of this node.
   * @param char Char to check for
   * @returns True if the char exists as a child in this node.
   */
  public hasChild(char: string): boolean {
    return char in this.children;
  }

  /**
   * Removes the {@link TrieNode} child for the given `char`.
   * @param char Char to remove
   */
  public removeChild(char: string): void {
    delete this.children[char];
    this._count--;
  }
}
```