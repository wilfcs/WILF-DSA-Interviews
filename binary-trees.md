Types of Binary Trees:
1. Full Binary Tree – It is a type of tree in which each node has either 0 or 2 children.
2. Complete Binary Tree – It is a type of tree where all the levels of the tree are filled with nodes except the last level. The last level must have each node as left as possible.
3. Perfect Binary Tree – It is a type of tree in which all the leaf nodes should be on the same level.
4. Balanced Binary Tree – In a balanced binary tree, the height of the tree should be at max log(N), where N is the number of nodes. The tree should not be skewed.
5. Degenerate Tree – A degenerate tree is a skewed tree that is similar to a linked list where each node is connected to the left of the previous node and the tree goes leftwards only.

# Preorder Traversal (DFS)
```cpp
class Solution {
public:
    vector<int> ans;
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == NULL) return ans;
        ans.push_back(root->val);
        preorderTraversal(root->left);
        preorderTraversal(root->right);
        return ans;
    }
};
```

# Inorder Traversal (DFS)
```cpp
class Solution {
public:
    vector<int> ans;
    vector<int> inorderTraversal(TreeNode* root) {
        if(root == NULL) return ans;
        inorderTraversal(root->left);
        ans.push_back(root->val);
        inorderTraversal(root->right);
        return ans;
    }
};
```

# Postorder Traversal (DFS)
```cpp
class Solution {
public:
    vector<int> ans;
    vector<int> postorderTraversal(TreeNode* root) {
        if(root == NULL) return ans;
        postorderTraversal(root->left);
        postorderTraversal(root->right);
        ans.push_back(root->val);
        return ans;
    }
};
```

# Level Order Traversal (BFS)
## Steps:
- Take a queue data structure and push the root node to the queue.
- Set a while loop which will run till our queue is non-empty.
- In every iteration, pop out from the front of the queue and assign it to a variable (say temp).
- If temp has a left child, push it to the queue.
- If temp has a right child, push it to the queue.
- At last push the value of the temp node to our “ans” data structure.
## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        if(root==NULL) return ans;

        q.push(root);

        while(q.size()){
            int size = q.size();
            vector<int> level;

            for(int i=0; i<size; i++){
                TreeNode* node = q.front();
                q.pop();

                if(node->left) q.push(node->left);
                if(node->right) q.push(node->right);

                level.push_back(node->val);
            }
            ans.push_back(level);
        }

        return ans;
    }
};
```
