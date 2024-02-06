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