# DSA Technical Journal — Python-Based Garden Simulator (v0.6.0)

## 1. Project Overview
GardenSim is a browser-based garden simulation project developed for our Data Structures and Algorithms subject. The project allows users to interact with a virtual garden while demonstrating different data structures using Python. The project uses HTML, CSS, JavaScript, and Python integration. The main goal of this project is to apply the concepts of Python OOP, Stack, Queue, 2D List/Array, Hierarchical Tree, and Tree Traversal into a simple and interactive garden environment. 

## 2. Current Project Progress
| Requirement                         | Project Implementation                                                        | Current Status | Purpose                                            |
| ----------------------------------- | ----------------------------------------------------------------------------- | -------------- | -------------------------------------------------- |
| Python OOP                          | `Crop`, `ResourceStorage`, `Plant`, `TreeNode`, `GardenTree`                  | Implemented    | Group data + behavior (water, coins, plant growth) |
| 2D List / Array                     | `GardenGrid.cells[row][col]` (5x5)                                            | Implemented    | Garden plots for planting/harvesting               |
| Stack                               | `ActionHistoryStack`                                                          | Implemented    | Undo the most recent planting (LIFO)               |
| Queue                               | `ClimateQueue` (deque)                                                        | Implemented    | Climate events happen in arrival order (FIFO)      |
| Hierarchical Tree                   | `TreeNode`, `BinaryTree`, `GardenTree` (root Garden, each plant a node)       | Implemented    | Show how plants are organized in a hierarchy       |
| BFS                                 | `bfs(root)` using `deque` (queue)                                             | Implemented    | Visit plants level by level                        |
| DFS: Preorder / Inorder / Postorder | `preorder`, `inorder`, `postorder` (recursive) + `dfs_stack` (explicit stack) | Implemented    | Visit plants subtree by subtree                    |

## 3. Implemented Data Structures
### 3.1 Python OOP
`ResourceStorage` owns water/seeds/energy/hope/coins and validates changes (`can_afford`, `change`). `Plant` knows its age/stage. `GardenGrid`, `ActionHistoryStack`, `ClimateQueue`, `GardenTree` each have one responsibility. OOP fits because each game entity has its own state and rules.
### 3.2 2D List / Array
`cells = [[None for _ in range(5)] for _ in range(5)]`. Row = vertical index, column = horizontal. Example concept: `cells[row][column]`. List comprehension avoids shared rows from `[[None]*5]*5`.
### 3.3 Stack
LIFO. `push` records each planting; `pop` returns the newest and `undo_last()` reverses it (removes plant, refunds coins/seed/water). Interface: Action History panel + Undo button.
### 3.4 Queue
FIFO. `enqueue` adds a random future event; `dequeue` (`deque.popleft`) removes the oldest on Next Day and applies its effect. Interface: Climate Queue panel (front marked "Next out").
### 3.5 Hierarchical Tree
A binary tree: every `TreeNode` has a `parent`, `left`, and `right`. The root is `Garden` (🌳); each planted crop becomes a node placed in the first free slot (level order), so the tree stays compact. When a plant is harvested or undone, the last node moves into its gap so the tree has no holes. The UI draws it as circles and lines (root at the top, leaves at the bottom). Terms: root, branch, parent, child, siblings, leaf, subtree, size (node count), depth (steps from root), height (longest path down to a leaf).
Lesson example used for testing (`Lesson Example A–F` button):
```
        A            root
      /   \
     B     C         branches
    / \     \
   D   E     F       leaves
```
### 3.6 Tree Traversal
| Traversal         | Order                         | Uses                                    | Result on example |
| ----------------- | ----------------------------- | --------------------------------------- | ----------------- |
| BFS (level order) | level by level, left to right | Queue (FIFO)                            | A B C D E F       |
| DFS Preorder      | Root → Left → Right           | recursion (call stack) / explicit Stack | A B D E C F       |
| DFS Inorder       | Left → Root → Right           | recursion                               | D B E A C F       |
| DFS Postorder     | Left → Right → Root           | recursion                               | D E B F C A       |
`dfs_stack()` is an iterative preorder with a Python list as a stack (push right first so left is popped first). The program checks it equals `preorder()`. In the game, the four buttons animate the visiting order on the tree; on the lesson example they also check the result against the expected answer.

## 4. Complexity Summary
| Structure / Operation          | Complexity            | Reason                                                   |
| ------------------------------ | --------------------- | -------------------------------------------------------- |
| Grid access `cells[r][c]`      | O(1)                  | Direct indexes                                           |
| Grid grow_all / count          | O(rows × cols)        | Visits every cell                                        |
| Stack push / pop / peek        | O(1)                  | Operates on the end of a list                            |
| Queue enqueue / dequeue        | O(1)                  | `deque` is efficient at both ends                        |
| Tree insert (add_plant)        | O(n)                  | BFS to find the first free slot                          |
| Tree remove_pos                | O(n)                  | BFS list to find the node and the last node              |
| BFS                            | O(n) time, O(w) space | Each node once; queue holds one level (w = widest level) |
| Preorder / Inorder / Postorder | O(n) time, O(h) space | Each node once; recursion depth = height h               |
| `height()` / `size()`          | O(n)                  | Visits whole subtree                                     |

## 5. Testing and Demonstration
- Run All 1–5, then plant on empty tiles: resources drop, tree and stack update.
- Plant on an occupied tile / without resources: message shown, nothing changes.
- Undo repeatedly until "Stack is empty."
- Click BFS, Preorder, Inorder, Postorder: nodes light up in visit order with numbers.
- Click `Lesson Example A–F`: all four traversals show ✔ (A B C D E F / A B D E C F / D B E A C F / D E B F C A).
- Plant, harvest, and undo: tree shape updates and stays valid.
- Next Day ×3: events leave the queue in the order Rain → Drought → Heatwave.
- Crops ripen after N days (glowing tile); harvest gives coins + hope.
- Refresh page: code in editor stays, unlocked panels are restored.

Suggested evidence: [insert screenshot(s)]

## 6. Approved Technical Environment
- HTML5 / DOM / Vanilla JavaScript
- Tailwind CSS (CDN)
- Lucide Icons (CDN)
- Pyodide (Python runtime)
- External CSS (`css/style.css`)

## 7. Project Version and Git Development (Optional)
Current Version: v0.6.0 (adds BFS and DFS traversals) · Repository: garden-simulator · Git: one commit per version tag (v0.1.0 … v0.6.0), see README.md · GitHub Repository: [PASTE YOUR LINK HERE]

## 8. AI Reflection
During the development of the Python-Based Garden Simulator, I used AI as a guide to help me understand the project and its programming concepts. Since I am still a beginner, AI helped me learn how to organize the project and understand how different data structures work.
AI helped me with the implementation of the required data structures, such as the 2D List for the garden grid, Stack for action history, Queue for climate events, and Hierarchical Tree for organizing the garden and planted crops. It also helped me understand Tree Traversal, such as BFS, Preorder, Inorder, and Postorder, which are used to visit or go through the nodes in a tree in different ways.
Whenever I had difficulty understanding the code or encountered errors, I used AI to explain the problem and suggest possible solutions. However, I still needed to check and test the suggestions to make sure they worked properly in my project.
Through this experience, I learned that AI is a helpful tool for beginners, especially when learning new programming concepts. It made some parts of the development easier, but I also realized that I should understand the code and not just copy the answers.


## 9. Current Limitations and Next Steps
- Game state (plants, day) resets on refresh; only code and unlocked panels persist.
- The tree is a binary tree but not a Binary Search Tree, so inorder is not sorted.
- Traversals are shown for the garden and the lesson example only.
- Next: BST (insert/search/delete), hash table for crop lookup, graph of zones with BFS/DFS..

## 10. Submission Summary
This version integrates Python OOP, a 2D list garden grid, an action-history stack, a climate queue, a visible binary garden tree, and tree traversals (BFS and DFS preorder, inorder, postorder) in one interactive simulator. It satisfies the Lesson 2 submission: updated journal, GitHub repository link, BFS, and the three DFS traversals.

## 11. Screenshots
[insert]

