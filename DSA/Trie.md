
A **Trie** (also called **Prefix Tree**) is a **tree-based data structure** used for **fast prefix-based searching**.  

It is mainly used in **autocomplete, spell check, dictionary search**, and **search suggestions**.


Both binary search trees and tries are trees, but each node in binary search trees always has two children, whereas tries’ nodes, on the other hand, can have more.

In a trie, every node (except the root node) stores one character or a digit. By traversing the trie down from the root node to a particular node _n_, a common prefix of characters or digits can be formed which is shared by other branches of the trie as well.

By traversing up the trie from a leaf node to the root node, a _String_ or a sequence of digits can be formed.
## ✅ Basic Trie Node

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord;
}
```

---

## ✅ Trie Implementation

```java
class Trie {

    private TrieNode root = new TrieNode();

    // Insert word into Trie
    public void insert(String word) {
        TrieNode node = root;

        for (char ch : word.toLowerCase().toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new TrieNode();
            }
            node = node.children[idx];
        }
        node.isEndOfWord = true;
    }

    // Check if prefix exists
    public boolean startsWith(String prefix) {
        TrieNode node = root;

        for (char ch : prefix.toLowerCase().toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                return false;
            }
            node = node.children[idx];
        }
        return true;
    }

    // Get autocomplete suggestions
    public List<String> autoComplete(String prefix) {
        List<String> result = new ArrayList<>();
        TrieNode node = root;

        // Move to prefix node
        for (char ch : prefix.toLowerCase().toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                return result;
            }
            node = node.children[idx];
        }

        dfs(node, prefix.toLowerCase(), result);
        return result;
    }

    private void dfs(TrieNode node, String word, List<String> result) {
        if (node.isEndOfWord) {
            result.add(word);
        }

        for (int i = 0; i < 26; i++) {
            if (node.children[i] != null) {
                dfs(node.children[i], word + (char)(i + 'a'), result);
            }
        }
    }
}
```

---

## ✅ Usage Example (Restaurant Autocomplete)

```java
public class Main {
    public static void main(String[] args) {

        Trie trie = new Trie();

        trie.insert("burgerking");
        trie.insert("burgerhut");
        trie.insert("pizzahut");
        trie.insert("pizzapalace");

        System.out.println(trie.autoComplete("bur"));
        // Output: [burgerking, burgerhut]

        System.out.println(trie.autoComplete("pizza"));
        // Output: [pizzahut, pizzapalace]
    }
}
```

---

## ⏱️ Time Complexity

|Operation|Complexity|
|---|---|
|Insert|O(word length)|
|Search|O(prefix length)|
|Autocomplete|O(prefix + results)|

---

## 🎯 Interview Tip (Say This)

> “Trie stores characters in a tree structure and allows prefix search in O(length of prefix) time, which makes it ideal for autocomplete features.”

---

## 🔥 Follow-up Topics (Very Common Interview)

- Trie + **location filtering**
    
- Trie vs **Elasticsearch**
    
- Memory optimization using **Map instead of array**
    
- Trie with **top-K ranking**
    

If you want next → **Trie + Location search design** 🚀