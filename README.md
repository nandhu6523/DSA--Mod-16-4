# Data Structures Using Python-mod-16-4
# AIM:
To implement a B+ Tree in Python and write a method insert(self, key, value) to insert key-value pairs into the tree with automatic node splitting when capacity exceeds.

# ALGORITHM:
step 1:Create a Node class to represent internal and leaf nodes.

step 2: Maintain leaf nodes in a linked list for B+ Tree properties.

step 3:Implement insertion logic:

   a] Insert values in sorted order into leaves.

   b] When a node exceeds capacity, split and promote the middle key to the parent.

step 4:Display tree level by level to visualize structure.

# PROGRAM:
```
class Node(object):
    
    def __init__(self, order):
        
        self.order = order
        self.keys = []
        self.values = []
        self.leaf = True

    def add(self, key, value):
        
        if not self.keys:
            self.keys.append(key)
            self.values.append([value])
            return None

        for i, item in enumerate(self.keys):
            
            if key == item:
                self.values[i].append(value)
                break

            
            elif key < item:
                self.keys = self.keys[:i] + [key] + self.keys[i:]
                self.values = self.values[:i] + [[value]] + self.values[i:]
                break

        
            elif i + 1 == len(self.keys):
                self.keys.append(key)
                self.values.append([value])

    def split(self):
        
        left = Node(self.order)
        right = Node(self.order)
        mid = self.order // 2

        left.keys = self.keys[:mid]
        left.values = self.values[:mid]

        right.keys = self.keys[mid:]
        right.values = self.values[mid:]

      
        self.keys = [right.keys[0]]
        self.values = [left, right]
        self.leaf = False

    def is_full(self):
     
        return len(self.keys) == self.order

    def show(self, counter=0):
        
        print(counter, str(self.keys))

        
        if not self.leaf:
            for item in self.values:
                item.show(counter + 1)

class BPlusTree(object):
    
    def __init__(self, order=8):
        self.root = Node(order)

    def _find(self, node, key):
        
        for i, item in enumerate(node.keys):
            if key < item:
                return node.values[i], i

        return node.values[i + 1], i + 1

    def _merge(self, parent, child, index):
        
        parent.values.pop(index)
        pivot = child.keys[0]

        for i, item in enumerate(parent.keys):
            if pivot < item:
                parent.keys = parent.keys[:i] + [pivot] + parent.keys[i:]
                parent.values = parent.values[:i] + child.values + parent.values[i:]
                break

            elif i + 1 == len(parent.keys):
                parent.keys += [pivot]
                parent.values += child.values
                break

    def insert(self, key, value):
        parent=None
        child=self.root
        while not child.leaf:
            parent=child
            child, index = self._find(child, key)
        child.add(key,value)
        if child.is_full():
            child.split()
            if parent and not parent.is_full():
                self._merge(parent,child,index)
    def retrieve(self, key):
       
        child = self.root

        while not child.leaf:
            child, index = self._find(child, key)

        for i, item in enumerate(child.keys):
            if key == item:
                return child.values[i]

        return None

    def show(self):
        
        self.root.show()

def demo_node():
    node = Node(order=4)
    node.add('a', 'alpha')
    node.add('b', 'bravo')
    node.add('c', 'charlie')
    node.add('d', 'delta')
    node.show()

    print('\nSplitting node...')
    node.split()
    node.show()

def demo_bplustree():
    print('B+ tree...')
    bplustree = BPlusTree(order=4)
   
    bplustree.insert('a', 'alpha')
    bplustree.insert('b', 'bravo')
    bplustree.insert('c', 'charlie')
    bplustree.insert('d', 'delta')
    bplustree.insert('e', 'echo')
    bplustree.insert('f', 'foxtrot')
    bplustree.show()

    

if __name__ == '__main__':
    demo_node()
    print('\n')
    demo_bplustree()

```
# OUTPUT:
![image](https://github.com/user-attachments/assets/831deb0d-87fb-49f8-9aff-63f3e9afa8f6)

# RESULT:
Thus the implement a B+ Tree in Python and write a method insert(self, key, value) to insert key-value pairs into the tree with automatic node splitting when capacity exceeds was
successfully executed.
