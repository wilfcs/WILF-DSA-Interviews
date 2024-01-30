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

# [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

## Approach ->
Simple level order traversal but on odd levels reverse the level vector. Don't increase the TC of the code by using the reverse stl, instead insert into the level vector in reverse manner by keeping a flag variable. Look at the code properly.

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;

        // Boolean flag to track whether the current level is even or odd
        bool isEven = true;

        // Check if the tree is empty to avoid runtime error 
        if(root==NULL) return ans;

        q.push(root);

        while(q.size()){
            int size = q.size();
            vector<int> level(size);

            for(int i=0; i<size; i++){
                TreeNode* node = q.front();
                q.pop();
                
                if(node->left != NULL) q.push(node->left);
                if(node->right != NULL) q.push(node->right);
                
                // Determine the index to insert the node's value based on whether the level is even or odd
                int index = isEven ? i : size-i-1;
                // Insert the node's value into the level vector at the calculated index
                level[index]=node->val;
            }
            // Toggle the isEven flag
            isEven = !isEven;
            ans.push_back(level); 
        }

        return ans;
    }
};
```

# [Boundary Traversal of Binary Tree](https://www.codingninjas.com/studio/problems/boundary-traversal-of-binary-tree_790725?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach ->
Traverse the left boundary of the tree and add its elements to the resultant vector except the leaf nodes, then add the leaf nodes to the resultant, then traverse the right boundary and add the reverse of it to the resultant vector other than the leaf nodes. Follow iterative approach to traverse the boundaries to avoid conflict in answer.

## Code ->
```cpp
// Check if the given node is a leaf node
bool isLeaf(TreeNode<int> *root) {
  return !root->left && !root->right;
}

// Add the left boundary nodes to the result vector
void addLeftBoundary(TreeNode<int> *root, vector<int> &res) {
  TreeNode<int> *cur = root->left;
  while (cur) {
    // If the current node is not a leaf, add it to the result
    if (!isLeaf(cur)) res.push_back(cur->data);
    // Move to the left child if it exists; otherwise, move to the right child
    if (cur->left) cur = cur->left;
    else cur = cur->right;
  }
}

// Add the right boundary nodes to the result vector
void addRightBoundary(TreeNode<int> *root, vector<int> &res) {
  TreeNode<int> *cur = root->right;
  vector<int> tmp;
  while (cur) {
    // If the current node is not a leaf, add it to a temporary vector
    if (!isLeaf(cur)) tmp.push_back(cur->data);
    // Move to the right child if it exists; otherwise, move to the left child
    if (cur->right) cur = cur->right;
    else cur = cur->left;
  }
  // Add the nodes from the temporary vector in reverse order to maintain anti-clockwise order
  for (int i = tmp.size() - 1; i >= 0; --i) {
    res.push_back(tmp[i]);
  }
}

// Recursively add leaf nodes to the result vector
void addLeaves(TreeNode<int> *root, vector<int> &res) {
  // If the current node is a leaf, add it to the result
  if (isLeaf(root)) {
    res.push_back(root->data);
    return;
  }
  // Recursively process left and right children
  if (root->left) addLeaves(root->left, res);
  if (root->right) addLeaves(root->right, res);
}

// Main function to traverse the boundary nodes in anti-clockwise order
vector<int> traverseBoundary(TreeNode<int> *root) {
  vector<int> res;
  // Return an empty vector if the tree is empty
  if (!root) return res;

  // Add the root node to the result if it is not a leaf
  if (!isLeaf(root)) res.push_back(root->data);

  // Add the left boundary nodes
  addLeftBoundary(root, res);

  // Add leaf nodes
  addLeaves(root, res);

  // Add the right boundary nodes
  addRightBoundary(root, res);

  return res;
}
```

Pending -> vertical order traversal, top view, bottom view of bt

# [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

## Approaches ->
1. You can do the level order traversal and put the last node of each level in your answer vector.
2. Better approach would be to use recursion to do so to avoid the extra space (auxiliary space would be O(log n) or O(Height) in avg case due to recursive stack and O(n) in worst case if the tree is skewed). In this approach simply maintain a variable to determine the level of the BT. Maintian a DS to store the visited levels. Call the recursion for the right side of the tree and then the left side. When a level is visited for the first time, it signifies the discovery of a node from the right side at that level, so push that node's value in our answer.  Also push that level in the DS to ensure that it has been visited the first time.

## Codes ->
1.
```cpp
class Solution {
public:
    vector <int> ans;
    void helper(TreeNode* root){
        if(root == NULL) return;
        queue <TreeNode*> q;
        q.push(root);
        while(q.size()){
            int size = q.size();
            // vector <int> temp;
            for(int i=0; i<size; i++){
                TreeNode* nnode = q.front();
                q.pop();
                if(nnode->left) q.push(nnode->left);
                if(nnode->right) q.push(nnode->right);

                // if it is the last node of the current level then push it in ans
                if(i==size-1) ans.push_back(nnode->val);
            }
            // ans.push_back(temp[temp.size()-1]);
        }  
    }
    vector<int> rightSideView(TreeNode* root) {
        helper(root);
        return ans;
    }
};
```

2.
```cpp
class Solution {
public:
    void helper(TreeNode* root, vector<int> &ans, vector<int> &visitedLevels, int curLevel){
        if(root==NULL) return;

        // if the level is visited for the first time, enter it into the visitedLevels and push the node's val in ans.
        if(visitedLevels.size()==curLevel){
            ans.push_back(root->val);
            visitedLevels.push_back(curLevel);
        }

        // call recursion for the right side and then the left side while increasing the size of the curLevel by 1
        helper(root->right, ans, visitedLevels, curLevel+1);
        helper(root->left, ans, visitedLevels, curLevel+1);
    }
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        vector<int> visitedLevels;

        helper(root, ans, visitedLevels, 0);
        return ans;
    }
};
```