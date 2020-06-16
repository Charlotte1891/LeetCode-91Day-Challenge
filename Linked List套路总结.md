<h3>Linked List 套路总结</h3> 

<strong>Reverse</strong>

```java
Node reverse(Node head) {
  Node cur = head;
  Node prev = null;
  Node next = null;
  while (cur != null) {
    next = cur.next; // store previous next position before changing cur.next
    cur.next = prev;
    prev = cur; // move along the linked list
    cur = next;
  }
  return prev;
}
```



<strong>SortedMerge</strong>

```java
// Merge two sorted linked lists
// Use a temporary dummy node as the start of result list
// Use another pointer tail pointing to the last node of result list for append
// Remove one node from either a or b and adding it to tail
// Result is dummy.next
Node dummyNode = new Node(0);
Node tail = dummyNode;

while(headA != null && headB != null) {
  if(headA.val < headB.val) {
    tail.next = headA;
    headA = headA.next;
  }
  if(headB.val < headA.val) {
    tail.next = headB;
    headB = headB.next;
  }
  tail = tail.next;
}

if(headA == null) {
  tail.next = headB;
}else {
  tail.next = headA;
}

return dummyNode.next;  
```



<strong>Intersect</strong>

<em>HashMap</em>

```javascript
let data = new Set() // store all nodes in linked list A
while(headA !== null) {
  data.add(headA)
  headA = headA.next
}
while(headB !== null) {
  if(data.has(headB)) return headB
  headB = headB.next
}
return null
```

<em>Double Pointer</em>

<p style="color:MediumSeaGreen;">
  l1 = a + c, l2 = b + c. Redirect a to l2 after iterating over l1, until intersection point: a + c + b. Redirect b to l1 after iterating over l2, until intersection point: b + c + a. Than a and b must meet at the intersection point.
</p>

```javascript
let a = headA, b = headB
while(a != b) {
  a = a ? a.next : null
  b = b ? b.next : null
  if(a === null && b === null) return null
  if(a === null) a = headB
  if(b === null) b = headA
}
return a
```



<strong>Cycle</strong>

<em>HashMap</em>

```javascript
let data = new Set()
while(head) {
  if(data.has(head)) {
    return head
  } else {
    data.add(head)
  }
  head = head.next
}
return null
```

<em>Double Pointer</em>

```javascript
// the second time fast meet slow is the entrance of cycle
if(head === null || head.next === null) return null
let fast = slow = head
do{
  if(fast !== null && fast.next !== null) {
    fast = fast.next.next // 2 steps each time
  }else {
    fast = null
  }
  slow = slow.next // 1 step each time
} while(fast !== slow) // first encounter

if (fast === null) return null
fast = head
while(fast !== slow) { // second encounter
  fast = fast.next
  slow = slow.next
}
return fast
```



<strong> Design </strong>

1. Decide needed data structure

   ```javascript
   data needs to be stored in sequence --> array, linked list
   add/delete frequently in O(1) --> linked list
   get data in O(1) --> hashmap to store the index
   ```

   Example: 146. LRU Cache

   Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

   get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
   put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

   The cache is initialized with a positive capacity.

   ```javascript
    function ListNode(key, val) {
         this.key = key
         this.val = val;
         this.pre = this.next = null;
     }
   
     var LRUCache = function(capacity) {
         this.capacity = capacity
         this.size = 0
         this.data = {}
         this.head = new ListNode()
         this.tail = new ListNode()
         this.head.next = this.tail
         this.tail.pre = this.head
     };
   
     function get (key) {
         if(this.data[key] !== undefined){
             let node = this.data[key]
             this.removeNode(node)
             this.appendHead(node)
             return node.val
         } else {
             return -1
         }
     };
   
     function put (key, value) {
         let node
         if(this.data[key] !== undefined){
             node = this.data[key]
             this.removeNode(node)
             node.val = value
         } else {
             node = new ListNode(key, value)
             this.data[key] = node
             if(this.size < this.capacity){
                 this.size++
             } else {
                 key = this.removeTail()
                 delete this.data[key]
             }
         }
         this.appendHead(node)
     };
   
     function removeNode (node) {
         let preNode = node.pre,
             nextNode = node.next
         preNode.next = nextNode
         nextNode.pre = preNode
     };
   
     function appendHead (node) {
         let firstNode = this.head.next
         this.head.next = node
         node.pre = this.head
         node.next = firstNode
         firstNode.pre = node
     };
   
     function removeTail () {
         let key = this.tail.pre.key
         this.removeNode(this.tail.pre)
         return key
     };
   ```

   

