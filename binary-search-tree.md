# [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)

## Code ->
```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(root == NULL || root->val == val) return root;

        if(val<root->val)
            return searchBST(root->left, val);
        if(val>root->val)
            return searchBST(root->right, val);
        
        return NULL;
    }
};
```
# [Ceil from BST](https://www.codingninjas.com/studio/problems/ceil-from-bst_920464?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

```cpp
int findCeil(node *root, int input)
{

    int ceil = -1;
    
    while (root)
    {
        // If the input is already available in BST, return that.
        if (root->data == input)
            return input;

        // If the input is greater than root, then the ceil value must be in the right subtree.
        else if (root->data < input)
            root = root->right;

        // Otherwise, we mark ceil to be root and move to 
        // left subtree where it may be further closer to the input value
        else
        {
            ceil = root->data;
            root = root->left;
        }
    }

    //Return computed ceil value.
    return ceil;
}
```

# [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)

## Approach ->
You know that we must maintin the bst property so instead of inserting the node in the middle somewhere and rearranging the entire tree, just insert the given node on the leaf node. Traverse the tree to determine if it is a leaf node and insert, if its not a leaf node then go either left or right.

## Code ->
```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        // If the BST is initially empty (root is NULL), create a new node with the given value.
        if(root==NULL) return new TreeNode(val);

        // Initialize a pointer 'cur' to traverse the tree.
        TreeNode* cur = root;

        while(cur){
            // If the current node's value is greater than the new value, move to the left subtree if not a leaf
            if(cur->val > val){
                 // If the left child is NULL, insert the new value here and break the loop.
                if(cur->left==NULL){
                    cur->left = new TreeNode(val);
                    break;
                }
                // Otherwise, move to the left child.
                else cur = cur->left;
            }   
            else{
                if(cur->right==NULL){
                    cur->right = new TreeNode(val);
                    break;
                }
                else cur = cur->right;
            }       
        } 

        return root;
    }
};
```

# [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/description/)

## Approach ->
The code aims to delete a node with a given key in a Binary Search Tree (BST) by traversing the tree to locate the target node, and then appropriately adjusting the links based on the number of children the node has, ensuring the resulting tree remains a valid BST. Read the comments in the code to understand.

## Code ->
```cpp
class Solution {
public:
    // Helper function to handle the deletion of a node with two children
    TreeNode* helper(TreeNode* cur){
        // If the left child is NULL, return the right child
        if(cur->left==NULL) return cur->right;
        // If the right child is NULL, return the left child
        if(cur->right==NULL) return cur->left;

        // Save references to the right and left children
        TreeNode* rightChild = cur->right;
        TreeNode* leftChild = cur->left;
        // The node to be returned after deletion (in this case, the left child)
        TreeNode* toReturn = cur->left;

        // Find the rightmost node in the left subtree
        while(leftChild){
            if(leftChild->right==NULL) break;
            leftChild = leftChild->right;
        }
        // Attach the right subtree to the rightmost node of the left subtree
        leftChild->right = rightChild;

        // Return the left subtree as the new node to be linked to the parent
        return toReturn;
    }

    // Main function to delete a node with the given key in the BST
    TreeNode* deleteNode(TreeNode* root, int key) {
        // If the root is NULL, return NULL
        if(root==NULL) return NULL;
        // If the key to be deleted is at the root, call the helper function
        if(root->val==key) return helper(root);

        // Initialize a pointer 'cur' to traverse the tree.
        TreeNode* cur = root;

        // Traverse the tree to find the node to be deleted
        while(cur){
            // If the current node's value is greater than the key, move to the left subtree.
            if(cur->val > key){
                // If the left child exists and its value matches the key, delete the node and break.
                if(cur->left && cur->left->val==key){
                    cur->left = helper(cur->left);
                    break;
                }
                // Otherwise, move to the left child.
                else {
                    cur = cur->left;
                }
            }
            // If the current node's value is less than or equal to the key, move to the right subtree.
            else{
                // If the right child exists and its value matches the key, delete the node and break.
                if(cur->right && cur->right->val==key){
                    cur->right = helper(cur->right);
                    break;
                }
                // Otherwise, move to the right child.
                else {
                    cur = cur->right;
                }
            }
        }

        // Return the root of the modified BST after deletion.
        return root;
    }
};
```
# [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)
## Approach->
The inorder of a bst returns the elements in sorted order because bst follows the property of everything smaller on the left subtree and everything greater on the right subtree
So we can do the inorder traversal and store all elements in a vector but that will take the sc of O(N). To avoid that we can use a counter and when we visit the node we can increase the counter and find our desired element.
What if the question asked us to find the kth largest element? In that case we would traverse the tree and find the size of the tree as n, and then our kth largest would be (n-k)th smallest.

## Code ->
```cpp
class Solution {
public:
    // IMPORTANT: pass ans and also k by reference
    void inorder(TreeNode* root, int &k, int &ans){
        // NOTE HOW K IS PASSED BY REFERENCE
        if(root==NULL) return;

        inorder(root->left, k, ans);

        k--;
        if(k==0){
            ans = root->val;
            return;
        }

        inorder(root->right, k, ans);
    }
    int kthSmallest(TreeNode* root, int k) {
        
        int ans;

        inorder(root, k, ans);

        return ans;

    }
```
# [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

## Approach -> 
Maintain a range for each node and if a node's value crosses the range then return false.

## Code->
```cpp
class Solution {
public:
    // Helper function to check if the subtree rooted at 'root' is a valid BST
    bool helper(TreeNode* root, long long minVal, long long maxVal){
        // If the current node is NULL, it is a valid BST by definition
        if(root==NULL) return true;
        
        // Check if the current node's value is within the valid range for a BST
        if(root->val <= minVal || root->val >= maxVal) return false;
        
        // Recursively check the left and right subtrees, updating the valid range
        // For the left subtree, the maximum value becomes the current node's value
        // For the right subtree, the minimum value becomes the current node's value
        return helper(root->left, minVal, root->val) && helper(root->right, root->val, maxVal);
    }

    bool isValidBST(TreeNode* root) {
        // Call the helper function with initial range constraints
        // The initial range is set to (LLONG_MIN, LLONG_MAX) representing the full range of long long
        return helper(root, LLONG_MIN, LLONG_MAX);
    }
};
```

# [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

## Approach ->
Go to every node and figure out if both the targets exist on the left then call the recursive function for the left side, and if both the targets exist on the right side then call the recursive fn for the right side but if none of the condition is true and they exist on the opposite sides then return the node itself because that node itself will be the intersection point.

## Code ->
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root) return NULL;

        // If both nodes are smaller than the current root, search in the left subtree
        if(root->val > p->val && root->val > q->val)
            return lowestCommonAncestor(root->left, p, q);

        // If both nodes are larger than the current root, search in the right subtree
        if(root->val < p->val && root->val < q->val)
            return lowestCommonAncestor(root->right, p, q);

        // If the current root is between the values of p and q, or equal to either of them,
        // then it is the lowest common ancestor
        return root;
    }
};
```

# [1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/description/)

## Approach ->
The code aims to construct a binary search tree (BST) from a preorder traversal. It utilizes a recursive approach, where a helper function constructs nodes by ensuring that each node's value follows the BST property. The upper bound parameter dynamically adjusts during recursion, controlling the valid range of values for each node. This ensures that the left subtree contains values strictly less than the node, and the right subtree contains values strictly greater. The process continues until the entire BST is constructed from the given preorder traversal.

## Code ->
```cpp
class Solution {
public:
    // Helper function to construct the BST from preorder traversal
    TreeNode* helper(vector<int> &preorder, int &i, int upperBound) {
        // If we have processed all elements in preorder or the current element is greater than the upper bound
        if (i == preorder.size() || preorder[i] > upperBound) {
            return NULL; // Return NULL indicating no subtree here
        }

        // Create a new node with the current element as its value
        TreeNode* node = new TreeNode(preorder[i++]);

        // Recursively construct the left subtree with values less than the current node's value
        node->left = helper(preorder, i, node->val);

        // Recursively construct the right subtree with values greater than the upper bound i.e. the range of the parent nodes
        node->right = helper(preorder, i, upperBound);

        // Return the constructed node
        return node;
    }

    // Main function to construct the BST from preorder traversal
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        int i = 0; // Start from the first element in preorder
        return helper(preorder, i, INT_MAX); // Call the helper function with initial upper bound as INT_MAX
    }
};
```

# [ Predecessor And Successor In BST](https://www.codingninjas.com/studio/problems/predecessor-and-successor-in-bst_893049?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## Approaches ->
1. We know that the inorder of a bst is sorted, so store the elements in the sorted order inside a vector and return the elements just before and after key from the vector.
2. The code aims to find the predecessor and successor of a given key in a Binary Search Tree (BST). It traverses the tree, updating the predecessor when the current node's value is less than the key and updating the successor when the value is greater. The final predecessor is the maximum value in the left subtree, and the successor is the minimum value in the right subtree. This approach ensures that the predecessor is the largest node before the key, and the successor is the smallest node after the key in the BST's inorder traversal.

## Code ->
```cpp
// The function takes a binary search tree (BST) represented by the root node and a key as input.
// It returns a pair of integers representing the predecessor and successor of the given key in the BST.
pair<int, int> predecessorSuccessor(TreeNode *root, int key)
{
    // Initialize variables to store the predecessor and successor values.
    TreeNode* cur = root;  // Start from the root node.
    int pred = -1;  // Initialize predecessor to -1.
    int suc = -1;   // Initialize successor to -1.

    // Traverse the BST to find the node with the given key or determine its predecessor and successor.
    while(cur){
        if(cur->data == key) {
            break;  // If the current node's data matches the key, exit the loop.
        }
        else if(cur->data > key){
            // If the current node's data is greater than the key, update successor and move to the left subtree.
            suc = cur->data;
            cur = cur->left;
        }
        else{
            // If the current node's data is less than the key, update predecessor and move to the right subtree.
            pred = cur->data;
            cur = cur->right;
        } 
    }

    // In a BST, the inorder predecessor will be the maximum element in the left subtree from the target.
    TreeNode* leftTree = cur->left;
    while(leftTree){
        pred = leftTree->data;  // Update predecessor.
        leftTree = leftTree->right;  // Move to the rightmost node in the left subtree.
    }

    // The successor will be the minimum element in the right subtree from the target.
    TreeNode* rightTree = cur->right;
    while(rightTree){
        suc = rightTree->data;  // Update successor.
        rightTree = rightTree->left;  // Move to the leftmost node in the right subtree.
    }

    // Return the pair of predecessor and successor.
    return {pred, suc};
}
```

# [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

## Approaches ->
1. We know that if we store the inorder of bst it will be in sorted order. Once that is done we can simply apply the two pointer to reach our answer in the sorted array that we obtained. TC->O(2n) where O(n) is for tree traversal and O(n) is for array traversal. SC-> O(n)
2. We can do this in O(n) TC also by simply storing the visited nodes in a hashmap and simply checking if target - node.val exists or not in the hashmap.

## Code ->
```cpp
class Solution {
public:
    bool helper(TreeNode* root, int k, unordered_set<int> &visited){
        if(root == NULL) return false;

        // If k - root->val is found in the visited set, return true
        if(visited.count(k - root->val)) return true;

        // Add the current node's value to the visited set
        visited.insert(root->val);

        // Recursively check the left and right subtrees
        return helper(root->left, k, visited) || helper(root->right, k, visited);
    }

    bool findTarget(TreeNode* root, int k) {
        unordered_set<int> visited;
        return helper(root, k, visited);
    }
};
```

# [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/description/)

## Approaches ->
1. Do the inorder traversal on the tree and store the elements, the elements are supposed to be in the sorted order but two elements will not be in the sorted order. Now sort the vector. Now do the inorder traversal again and and keep comparing the elements with the sorted vector, and when you find that the values are different, then just replace the value of the node while doing the inorder traversal. TC-> O(n log n) SC-> O(n)
2. Let's try to reduce the TC as well as SC. So there are two possible cases when we do the inorder traversal. Case 1 is when the two elements at the wrong position are far from each other and the case 2 is when both the elements are adjacent to each other.. Let's lool at both of them->

Case1: To be swapped nodes are not adjacent -> [3, 25, 7, 8, 10, 15, 20, 5]
Case2: To be swapped nodes are adjacent -> [3, 5, 8, 7, 10, 15, 20, 25]

To tackle both the cases we will maintian three global variables-> start, mid, and end. And to keep track of the previous element while doing the recursive calls we keep a prev variable also. When there is first violation, i.e. when the first time you encounter that the previous element is greater than the current element then you make the start as prev and mid as current node. Now, in the case 2 there will not be second violation because both faulty nodes are adjacent to each other. But in case 1 there will be a second violation. So in case of second violation make end as current node. And after the recursive calls are over, you can simply swap start and mid if there was just one violation i.e. end is NULL. And you cvan swap start and end if there were two violations i.e. end is no null to finally recover the BST.

## Code ->
```cpp
class Solution {
public:
    // Global variables to keep track of nodes that need to be swapped
    TreeNode* start = NULL;
    TreeNode* mid = NULL;
    TreeNode* end = NULL;
    
    // Variable to store the previous node during the inorder traversal
    TreeNode* prev;

    // Inorder traversal to identify nodes violating the BST property
    void inorder(TreeNode* root){
        // Base case: If the current node is NULL, return
        if(root == NULL) return;

        // Traverse left subtree
        inorder(root->left);

        // Check if there is a violation in the BST property
        if(prev->val > root->val){
            // First violation: Set start and mid with prev and root respectively
            if(start == NULL){
                start = prev;
                mid = root;
            }
            // Second violation: Set end as root
            else
                end = root;
        }

        // Update prev to the current node
        prev = root;

        // Traverse right subtree
        inorder(root->right);
    }

    // Main function to recover the BST
    void recoverTree(TreeNode* root) {
        // Initialize prev with a dummy node having a very small value
        prev = new TreeNode(INT_MIN);

        // Perform inorder traversal to identify faulty nodes
        inorder(root);

        // Swap the values of start and end if two violations occurred, else swap start and mid
        if(end != NULL)
            swap(start->val, end->val);
        else
            swap(start->val, mid->val);
    }
};

```

# [Size of Largest BST in Binary Tree](https://www.codingninjas.com/studio/problems/size-of-largest-bst-in-binary-tree_893103?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Code ->
```cpp
// Class to store information about a subtree
class info {
public:
    int maxi;   // Maximum value in the subtree
    int mini;   // Minimum value in the subtree
    bool isBST; // Whether the subtree is a BST or not
    int size;   // Size of the subtree
};

// Recursive function to calculate information about the subtree rooted at 'root'
info solve(TreeNode* root, int& ans) {
    // Base case: If the current node is NULL, return default info
    if (root == NULL) {
        return {INT_MIN, INT_MAX, true, 0};
    }

    // Recursively calculate info for left and right subtrees
    info left = solve(root->left, ans);
    info right = solve(root->right, ans);

    // Information for the current node
    info currNode;

    // Size of the current subtree
    currNode.size = left.size + right.size + 1;

    // Maximum value in the subtree
    currNode.maxi = max(root->data, max(left.maxi, right.maxi));

    // Minimum value in the subtree
    currNode.mini = min(root->data, min(left.mini, right.mini));

    // Check if the current subtree is a BST
    if (left.isBST && right.isBST && root->data > left.maxi && root->data < right.mini) {
        currNode.isBST = true;
    } else {
        currNode.isBST = false;
    }

    // Update the global maximum size if the current subtree is the largest BST
    if (currNode.isBST) {
        ans = max(ans, currNode.size);
    }

    return currNode;
}

// Main function to find the size of the largest BST in a binary tree
int largestBST(TreeNode* root) {
    // Initialize the global variable to store the result
    int ans = 0;

    // Call the solve function to calculate information about the subtree and update the result
    info temp = solve(root, ans);

    // Return the size of the largest BST
    return ans;
}
```
# [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

## Approach ->
Pick middle element, make a new node with that middle element. Now assign left of that node and right of that node with recursive calls from start to mid and mid+1 to end.

## Code ->
```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums, int start, int end){
        if(end<=start) return NULL; 
        int midIdx=(end+start)/2;
        TreeNode* root=new TreeNode(nums[midIdx]);
        root->left=sortedArrayToBST(nums, start, midIdx);
        root->right=sortedArrayToBST(nums, midIdx+1,end);
        return root;
    }
    
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedArrayToBST(nums, 0,nums.size());
    }
};
```