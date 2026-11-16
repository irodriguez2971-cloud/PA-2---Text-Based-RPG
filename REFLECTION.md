1. For every function you wrote (TODO 1 to 19), explain in your own words what it does and
   give its time complexity in terms of n, the number of scenes. One short paragraph each.

   1) The Scene constructor creates exactly one pointer slot for each choice and initializes every slot to nullptr. 
   Since the assignment limits a scene to three choices, this takes O(1) time, or O(k) if k represents the number of choices.

   2) The Scene destructor deletes every child scene, deletes the choices pointer array, and deletes the scene’s data. Each owned object is deleted once, so the destructor takes O(m) time for a subtree containing m scenes.

   3) isEnding checks whether the scene has zero choices. It performs one comparison, so it takes O(1) time.

   4) getChoice checks whether an index is valid and returns the pointer stored at that index. It takes O(1) time.

   5) setChoice checks the index and stores a child pointer in that slot. It takes O(1) time.

   6) countScenes recursively visits every scene and adds one for each scene. Since every scene is visited once, 
   it takes O(n) time.

   7) countEndings recursively checks every scene and counts the scenes with zero choices. It takes O(n) time.

   8) height recursively calculates the height of every child and keeps the largest value. It visits every scene once, 
   so it takes O(n) time.

   9) clear deletes the root scene. The Scene destructor recursively deletes the rest of the tree, so clearing the entire 
   tree takes O(n) time.

   10) load opens the file, reads the starting HP, builds the root scene, and stores it in the tree. Reading the complete 
   story takes O(n) time, ignoring the length of individual text strings.

   11) readScene reads one scene, creates it, and recursively reads all of its children. For the entire story, each scene 
   is read once, so the total time is O(n).

   12) askChoice repeatedly reads input until the player enters a valid choice. Each individual input attempt takes O(1) 
   time. The total depends on how many invalid attempts the player makes.

   13) play follows one path from the root to an ending or death. If the path has height h, it visits h scenes. The tree 
   traversal is O(h), at most O(n), although inventory searches can make the worst case O(n²).

   14) printStats calls the scene-count, ending-count, and height functions. Each is O(n), so together they are still O(n).

   15) printSpoilers starts the recursive ending guide. It takes O(n) time because it may visit every scene.

   16) printEndings recursively visits the tree and prints each ending path. It takes O(n) time.

   17) greedyRun follows one path and checks the immediate loot of each scene’s choices. Since there are at most three 
   choices per scene, it takes O(h) time, which is at most O(n).

   18) bestLoot recursively checks every scene and calculates the best surviving loot from each subtree. Because each scene
   is visited once, it takes O(n) time.

   19) ptimalRun calls bestLoot once and prints its result. Its time complexity is O(n).

2. Why does each scene allocate exactly numChoices slots instead of declaring Scene<T>*
   choices[3]? What does an ending hold in choices_, and why?

   Each scene will allocate only the number of child slots it actually needs. This helps avoid wasting any memory on slots
that are not used. A fixed array such as choices[3] was not allowed because the assignment wanted dynamic allocation. 
An ending has zero choices, so choices_ is a nullptr, and it has no child scenes. The destructor can safely delete nullptr.

3. Walk through your Scene destructor. What gets deleted, and in what order? What would
   go wrong if choices_ were deleted first?

   The destructor first deletes every child scene, then each child destructor deletes its own children recursively. 
After all children are deleted, the destructor deletes the choices_ pointer array and finally deletes data_. If it was
deleted first, the program would lose access to the child pointers, causing memory leaks.

4. Show the path greedy took and the path optimal took on your story, with the loot for each.
   Explain why greedy lost. If greedy did not lose, change your story until it does, and explain
   what you changed.
5. bestLoot tries every path, yet it runs in O(n) time. Explain why it is not exponential, using
   what you know about paths and leaves in a tree.

    Although bestLoot thinks every possible branch, it won't repeatedly copy or separately explore the same subtree. 
Each scene belongs to only one part of this tree, and the function visits each scene once while combining the best result 
from each child. The total work is proportional to the number of scenes, which is O(n).

6. Which List methods does your game use, and why? What does one search cost as the bag
   grows?

   The game uses addFront to add the newest collected item to the inventory. It uses search to check whether the player 
already owns an item. It uses size to print the number of inventory items, and then it uses print to display the inventory
at the end. One search takes roughly O(n).