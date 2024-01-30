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
# [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
## Approach ->
To calculate the Maximum Depth, we can simply take the maximum of the depths of the left and right subtree and add 1 to it. And when there is no element left (i.e. we are on null) then return 0, that way for the NULL node we will return 0, and the leaf node will be 1 + 0 == 1 so that will return 1 and the node above will be 1 + 1 == 2 and that will return 2 and so on.. look at the code to understand.

## Code ->
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==NULL) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

# [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

## Approaches ->
1. Naive approach: For a Balanced Binary Tree, Check left subtree height and right subtree height for every node present in the tree. Hence, traverse the tree recursively and calculate the height of left and right subtree from every node, and whenever the condition of Balanced tree violates, simply return false. Time Complexity: O(N*N) 

2. Can we optimize the above brute force solution? Which operation do you think can be skipped to optimize the time complexity?

Ain’t we traversing the subtrees again and again in the above example?

Yes, so can we skip the repeated traversals?

What if we can make use of post-order traversal?

So, the idea is to use post-order traversal. Since, in postorder traversal, we first traverse the left and right subtrees and then visit the parent node, similarly instead of calculating the height of the left subtree and right subtree every time at the root node, use post-order traversal, and keep calculating the heights of the left and right subtrees and perform the validation.

## Code ->
```cpp
class Solution {
public:
    int findHeight(TreeNode* root){
        if(root==NULL) return 0;
        
        // find left subtree height and check if it is equal to -1 return -1
        int lh = findHeight(root->left);
        if(lh==-1) return -1;

        // similarly check for right subtree and return -1 if its giving -1
        int rh = findHeight(root->right);
        if(rh==-1) return -1;

        // the -1 originally comes from here. when the absolute difference bw left and right subtree is greater than 1 then that means it is not a balanced binary tree, hence return -1
        if(abs(lh-rh)>1) return -1;

        // return 1 + max of lh and rh to find the actual height of the tree
        return 1 + max(lh, rh);
    }
    bool isBalanced(TreeNode* root) {
        int height = findHeight(root);
        // if the returned value is -1 then return false else return true
        if(height==-1) return false;
        return true;
    }
};
```

# [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/submissions/)

## Approach ->
In the same code as of finding the height of BT, add left and right node's height and store it in ans if it is greater than ans.

## Code ->
```cpp
class Solution {
public:
    int ans = 0;
    int helper(TreeNode* root){
        if(!root) return 0;

        int x = helper(root->left);
        int y = helper(root->right);

        ans = max(x+y, ans);
        return 1 + max(x,y);
    }
    int diameterOfBinaryTree(TreeNode* root) {
        helper(root);
        return ans;
    }
};
```

# [124. Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

## Code ->
```cpp
class Solution {
public:
    int ans = INT_MIN;

    int helper(TreeNode* root){
        if(root==NULL) return 0;

        // Calculate the maximum path sum in the left and right subtrees. If any node is making the sum as negative then it's better to not include that negative number, so we will take 0 instead. That's why we have taken the max bw 0 and sum returned.
        int l = max(0, helper(root->left));
        int r = max(0, helper(root->right));

        // Update the global maximum path sum using the current node + left subtree sum + right subtree sum
        ans = max(ans, l+r+root->val);

        // Return the maximum path sum starting from the current node to its parent
        return root->val + max(l, r);
    }
    int maxPathSum(TreeNode* root) {
        helper(root);
        return ans;
    }
};
```

# [100. Same Tree](https://leetcode.com/problems/same-tree/description/)

## Code ->
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==NULL && q==NULL) return true;
        if(p==NULL || q==NULL) return false;
        if(p->val != q->val) return false;

        bool l = isSameTree(p->left, q->left);
        bool r = isSameTree(p->right, q->right);

        return (l&&r);
    }
};
```