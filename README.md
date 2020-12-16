# Binary Search Tree

![](https://github.com/shamy1st/data-structure/blob/main/images/binary-search-tree.png)

average/worst      |        indexing | search          | add             | remove(index or value)
-------------------|-----------------|-----------------|-----------------|-----------------------
Binary Search Tree | O(log n) / O(n) | O(log n) / O(n) | O(log n) / O(n) | O(log n) / O(n)

* **Traversing**

  * **Breadth First**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/breadth-first.png)
  
  * **Depth First**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/depth-first.png)
  
    * **Pre-Order**
    ![](https://github.com/shamy1st/data-structure/blob/main/images/pre-order.png)
    
    * **In-Order**
    ![](https://github.com/shamy1st/data-structure/blob/main/images/in-order.png)
    
    * **Post-Order**
    ![](https://github.com/shamy1st/data-structure/blob/main/images/post-order.png)
    
* **Depth**: number of edges from root to the target node. (root depth = 0)

* **Height** the longest path to a leaf. (leaf node height = 0) (Post-Order Traversal)

* **Minimum Value** left left left till find leaf node. O(log n)

* **Maximum Value** right right right till find leaf node. O(log n)

* **Equality** check that two trees are totally equals.

* **Validation** given a binary tree, validate that it is a binary search tree.

        public class Main {
            public static void main(String[] args) {
                BinarySearchTree<Integer> tree = new BinarySearchTree<>();
                tree.insert(7);
                tree.insert(4);
                tree.insert(9);
                tree.insert(1);
                tree.insert(6);
                tree.insert(8);
                tree.insert(10);
                System.out.println(tree.find(8));
                tree.traverseInOrder();
                System.out.println("height: " + tree.height());
                System.out.println("min: " + tree.min());
                System.out.println("max: " + tree.max());
                System.out.println("isEqual: " + tree.equals(null));
                System.out.println("isValid: " + tree.isBinarySearchTree());
                System.out.println("NodesAtDistance : " + tree.getNodesAtDistance(2));
                tree.traverseBreadthFirst();
            }
        }

        public class BinarySearchTree<T extends Comparable<T>> {
            private class Node<T extends Comparable<T>> {
                private T value;
                private Node<T> left;
                private Node<T> right;

                public Node(T value, Node<T> left, Node<T> right) {
                    this.value = value;
                    this.left = left;
                    this.right = right;
                }

                @Override
                public String toString() {
                    return value.toString();
                }
            }

            private Node<T> root;

            public BinarySearchTree() {

            }

            //O(log n)
            public void insert(T value) {
                if(root == null)
                    root = new Node<T>(value,null, null);

                Node<T> current = root;
                while(true) {
                    if(value.compareTo(current.value) < 0) {
                        if(current.left == null) {
                            current.left = new Node<T>(value, null, null);
                            break;
                        } else {
                            current = current.left;
                        }
                    } else if(value.compareTo(current.value) > 0) {
                        if(current.right == null) {
                            current.right = new Node<T>(value, null, null);
                            break;
                        } else {
                            current = current.right;
                        }
                    } else {
                        //equal: nothing to do
                        break;
                    }
                }
            }

            //O(log n)
            public boolean find(T value) {
                Node<T> current = root;
                while(current != null) {
                    if(value.compareTo(current.value) == 0) {
                        return true;
                    } else if(value.compareTo(current.value) < 0) {
                        current = current.left;
                    } else {
                        current = current.right;
                    }
                }
                return false;
            }

            public void traversePreOrder() {
                traversePreOrder(root);
            }

            private void traversePreOrder(Node<T> node) {
                if(node == null)
                    return;

                System.out.println(node.value);
                traversePreOrder(node.left);
                traversePreOrder(node.right);
            }

            public void traverseInOrder() {
                traverseInOrder(root);
            }

            private void traverseInOrder(Node<T> node) {
                if(node == null)
                    return;

                traverseInOrder(node.left);
                System.out.println(node.value);
                traverseInOrder(node.right);
            }

            public void traversePostOrder() {
                traversePostOrder(root);
            }

            private void traversePostOrder(Node<T> node) {
                if(node == null)
                    return;

                traversePostOrder(node.left);
                traversePostOrder(node.right);
                System.out.println(node.value);
            }

            public int height() {
                return height(root);
            }

            private int height(Node<T> node) {
                if(node == null)
                    return -1;

                return 1 + Math.max(height(node.left), height(node.right));
            }

            public T min() {
                if(root == null)
                    throw new IllegalStateException();
                return minBinarySearchTree(root);
            }

            //O(log n)
            //min value for "Binary Search Tree"
            private T minBinarySearchTree(Node<T> node) {
                if(node.left == null)
                    return node.value;

                return minBinarySearchTree(node.left);
            }

            public T max() {
                if(root == null)
                    throw new IllegalStateException();
                return maxBinarySearchTree(root);
            }

            //O(log n)
            //max value for "Binary Search Tree"
            private T maxBinarySearchTree(Node<T> node) {
                if(node.right == null)
                    return node.value;

                return maxBinarySearchTree(node.right);
            }

            //O(n)
            //min value for "Binary Tree"
            private T minBinaryTree(Node<T> node) {
                if(node.left == null && node.right == null)
                    return node.value;
                else if(node.left == null)
                    return minValue(node.value, minBinaryTree(node.right));
                else if(node.right == null)
                    return minValue(node.value, minBinaryTree(node.left));

                return minValue(node.value, minValue(minBinaryTree(node.left), minBinaryTree(node.right)));
            }

            private T minValue(T value1, T value2) {
                if(value1.compareTo(value2) < 0)
                    return value1;
                return value2;
            }

            public boolean equals(BinarySearchTree<T> other) {
                if(other == null)
                    return false;
                return equals(root, other.root);
            }

            private boolean equals(Node<T> node1, Node<T> node2) {
                if(node1 == null && node2 == null)
                    return true;

                if(node1 != null && node2 != null)
                    return node1.value.compareTo(node2.value) == 0
                            && equals(node1.left, node2.left)
                            && equals(node1.right, node2.right);

                return false;
            }

            public boolean isBinarySearchTree() {
                if(root == null)
                    return true;
                return isBinarySearchTree(root);
            }

            private boolean isBinarySearchTree(Node<T> node) {
                if(node.left == null && node.right == null)
                    return true;
                else if(node.left == null && node.right != null)
                    return node.value.compareTo(node.right.value) < 0 && isBinarySearchTree(node.right);
                else if(node.left != null && node.right == null)
                    return node.value.compareTo(node.left.value) > 0 && isBinarySearchTree(node.left);

                return node.value.compareTo(node.left.value) > 0
                        && node.value.compareTo(node.right.value) < 0
                        && isBinarySearchTree(node.left) && isBinarySearchTree(node.right);
            }

            //to check non binary search tree
            public void swapRoot() {
                Node temp = root.left;
                root.left = root.right;
                root.right = temp;
            }

            public ArrayList<T> getNodesAtDistance(int k) {
                ArrayList<T> list = new ArrayList<>();
                getNodesAtDistance(root, k, list);
                return list;
            }

            private void getNodesAtDistance(Node<T> node, int distance, ArrayList<T> list) {
                if(node == null)
                    return;
                if(distance == 0) {
                    list.add(node.value);
                    return;
                }

                getNodesAtDistance(node.left, distance-1, list);
                getNodesAtDistance(node.right, distance-1, list);
            }

            //Level Order
            public void traverseBreadthFirst() {
                for(int i=0;i<=height(); i++) {
                    getNodesAtDistance(i).forEach(item -> System.out.println(item));
                }
            }
        }

* **Balanced Binary Tree**  abs[ height(left) - height(right) ] <= 1

* **Unbalanced Binary Tree**
![](https://github.com/shamy1st/data-structure/blob/main/images/unbalanced-binary-tree.png)
![](https://github.com/shamy1st/data-structure/blob/main/images/right-skewed.png)
![](https://github.com/shamy1st/data-structure/blob/main/images/left-skewed.png)

* **Self-Balancing Trees**
  * **AVL Tree** (Adelson-Velsky and Landis)
  * **Red-Black Tree**
  * **B-Tree**
  * **Splay Tree**
  * **2-3 Tree**
