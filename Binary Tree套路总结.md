<h3>Binary Tree套路总结</h3>

<strong>Traversal of Binary Tree</strong>

<em>Pre-order Traversal</em>

1. Current node
2. Traverse left subnodes
3. Traverse right subnodes

```javascript
preOrder(root) {
  if root
  doSomething(root)
  preOrder(root.left)
  preOrder(root.right)
}
```
<em>In-order Traversal</em>

1. Traverse left subnodes

2. Current node

3. Traverse right subnodes

   ```javascript
   inOrder(root) {
     if root
     inOrder(root.left)
     doSomething(root)
     inOrder(root.right)
   }
   ```

<em>Post-order Traversal</em>

1. Traverse left subnodes

2. Traverse right subnodes

3. Current node

   ```javascript
   postOrder(root) {
     if root
     postOrder(root.left)
     postOrder(root.right)
     doSomething(root)
   }
   ```

<em>Level-order Traversal(BFS)</em>

```javascript
levelOrder(root) {
  queue = []
  queue.push(root)
  while queue.length {
    curLevel = queue
    queue = []
    for i = 0 to curLevel.length {
      doSomething(curLevel[i])
      if(curLevel[i].left) queue.push(curLevel[i].left)
      if(curLevel[i].right) queue.push(curLevel[i].right)
    }
  }
}
```



<strong>Construction of Binary Tree</strong>

You can determine a binary tree by the combination of inOrder traversal & (preOrder, postOrder, levelOrder traversal)

inOrder traversal can be used to seperate left subtree and right subtree

Example: 105. Construct Binary Tree from Preorder and Inorder Traversal



<strong>Binary Search Tree</strong>

Characteristics:

1. left subtree < root
2. right subtree > root
3. inOrder traversal of BST is a sorted list

Example: 1008. Construct Binary Search Tree from Preorder Traversal



<strong>Stack</strong>

Stack can be seen as complete binary tree

<em>complete binary tree: depth = k, #node = 2^k -1</em>

Given an node A[i]

Parent node: A[Math.floor(i/2)]

Left child node: A[2i]

Right child node: A[2i+1]

If A[parent[i]] >= A[i] --> max stack

If A[parent[i]] <= A[i] --> min stack

Example: Build a max stack

```javascript
// build the stack from bottom to up
BUILD-MAX-HEAP(A)
	A.heap-size = A.length
	for i = Math.floor(A.length / 2) downto 1
		MAX-HEAPIFY(A, i)

// maintain max-heap
MAX-HEAPIFY(A, i)
	l = LEFT(i)
	r = RIGHT(i)
	// find max(cur node, left child node, right child node) and exchange
	if l <= A.heap-size and A[l] > A[i]
		largest = l
	else largest = i
	if r <= A.heap-size and A[r] > A[largest]
		largest = r
	if largest != i 
		exchange A[i] with A[largest]
		// recursively maintain map heap after exchange
		MAX-HEAPIFY(A, largest)
```



<strong>Morris traversal</strong>

**Morris (InOrder) traversal** is a tree traversal algorithm that does *not* employ the use of recursion or a stack. 

<em>Algorithm</em>

- Initialize the `root` as the current node `curr`.
- While `curr` is *not* `NULL`, check if `curr` has a left child.
- If `curr` does not have a left child, print `curr` and update it to point to the node on the right of `curr`.
- Else, make `curr` the right child of the rightmost node in `curr`'s left subtree.
- Update `curr` to this left node.

```java
void Morris(Node root) {
    Node curr, prev; 
    if (root == null) return; 
    curr = root; 
    while (curr != null) { 
        if (curr.left_node == null) { 
	    System.out.print(curr.data + " "); 
            curr = curr.right_node; 
        } 
        else { 
            /* Find the previous (prev) of curr */
            prev = curr.left_node; 
            while (prev.right_node != null && prev.right_node != curr) 
            prev = prev.right_node; 
    
            /* Make curr as right child of its prev */
            if (prev.right_node == null) { 
	        prev.right_node = curr; 
	        curr = curr.left_node; 
            } 

            /* fix the right child of prev*/
            else { 
	        prev.right_node = null; 
	        System.out.print(curr.data + " "); 
	        curr = curr.right_node; 
            } 
        } 
    }
}
```
