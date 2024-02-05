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