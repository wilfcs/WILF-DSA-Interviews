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
# [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

## Code ->
```cpp
class Solution {
public:
    // Recursive helper function to check if two subtrees are symmetric
    bool helper(TreeNode* t1, TreeNode* t2) {
        // Base case: Both nodes are NULL, indicating symmetry
        if (t1 == NULL && t2 == NULL) {
            return true;
        }
        // Base case: One of the nodes is NULL, indicating asymmetry
        if (t1 == NULL || t2 == NULL || t1->val != t2->val) {
            return false;
        }

        // Recursive call for comparing left subtree of t1 with right subtree of t2
        // and right subtree of t1 with left subtree of t2
        return (helper(t1->left, t2->right) && helper(t1->right, t2->left));
    }

    // Main function to check if a binary tree is symmetric
    bool isSymmetric(TreeNode* root) {
        // Call the helper function to compare the left subtree with the right subtree
        return helper(root->left, root->right);
    }
};
```

# [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

## Code ->
```cpp
class Solution {
public:
    // Helper function to traverse the binary tree and construct paths
    void helper(TreeNode* root, vector<string> &ans, string s) {
        // Base case: If the current node is NULL, return
        if (root == NULL) {
            return;
        }
        
        // If the current node is a leaf node (no left or right child),
        // append its value to the current path and add the path to the answer
        if (root->left == NULL && root->right == NULL) {
            s += to_string(root->val); // Convert int to string
            ans.push_back(s); // Add the current path to the answer
            return;
        }

        // If the current node has left or right child, append its value to
        // the current path and continue traversing left and right subtrees
        s += to_string(root->val) + "->"; // Append current node value to the path

        helper(root->left, ans, s); // Recursively traverse left subtree
        helper(root->right, ans, s); // Recursively traverse right subtree

        s.pop_back(); // Backtrack: Remove the last character (->) to backtrack in the path
    }

    // Main function to get all binary tree paths
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans; // Vector to store the paths
        helper(root, ans, ""); // Call helper function to populate the answer vector
        return ans; // Return the vector containing all binary tree paths
    }
};
```
# [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

## Approaches ->
1. Brute Force: 
Make two vectors and store the path of both the target nodes. Now compare these two path vectors and return the last matching node. For example in example 2: pathOfP = [3,5] and pathOfQ = [3,5,2,4], so return 5. But here the space complexity will be O(2N).

2. Visit every node, if you find NULL return NULL, if you find the target return target, if for any recursion call one side is returning target or null just return target or null and if both sides are returning target then return the root itself. You won't understand this ik for sure. So here is the explanation, watch the dry run carefully and you'd understand [link](https://www.youtube.com/watch?v=_-QHfMDde90&list=PLkjdNRgDmcc0Pom5erUBU4ZayeU9AyRRu&index=27)

## Codes->
1. 
```cpp
class Solution {
public:
    // Helper function to find the path from the root to the target node in the tree
    bool findPath(TreeNode* root, TreeNode* target, vector<TreeNode*>& path) {
        // Base case: If the current node is NULL, return false
        if (root == NULL)
            return false;

        // Add the current node to the path
        path.push_back(root);

        // If the current node is the target node, return true
        if (root == target)
            return true;

        // Recursively search for the target node in the left and right subtrees
        if (findPath(root->left, target, path) || findPath(root->right, target, path))
            return true;

        // If the target node is not found in the current subtree, backtrack
        path.pop_back();
        return false;
    }

    // Main function to find the lowest common ancestor of two nodes in the binary tree
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // Vectors to store the paths from the root to nodes p and q
        vector<TreeNode*> pathOfP;
        vector<TreeNode*> pathOfQ;

        // Find the paths from the root to nodes p and q
        findPath(root, p, pathOfP);
        findPath(root, q, pathOfQ);

        // Initialize the result node as a placeholder
        TreeNode* ans = new TreeNode(-1);

        // Compare the paths to find the lowest common ancestor
        for (int i = 0; i < pathOfP.size(); i++) {
            // If one of the paths is shorter than the other, break the loop
            if (i == pathOfQ.size())
                break;

            // If the nodes in the paths are the same, update the result node
            if (pathOfP[i] == pathOfQ[i])
                ans = pathOfP[i];
            else
                break;
        }
        // Return the lowest common ancestor
        return ans;
    }
};
```
2.
```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // If the current node is NULL or either of the nodes is found, return the current node
        if (root == NULL || root == p || root == q)
            return root;

        // Recursively search for the nodes in the left and right subtrees
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        // If one of the nodes is not found in the left subtree, return the right subtree result even if its null
        if (left == NULL)
            return right;
        // If one of the nodes is not found in the right subtree, return the left subtree result
        else if (right == NULL)
            return left;
        // If both nodes are found in different subtrees, return the current node as the LCA
        else
            return root;
    }
};
```

# [Children Sum Property](https://www.codingninjas.com/studio/problems/children-sum-property_8357239?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Code ->
```cpp
bool isParentSum(Node *root) {
    // Base case: If the root is NULL or it is a leaf node (no children),
    // then the children sum property is trivially satisfied.
    if (root == NULL || (root->left == NULL && root->right == NULL))
        return true;

    int v1 = 0, v2 = 0;

    // If the left child exists, assign its value to v1.
    if (root->left) v1 = root->left->data;

    // If the right child exists, assign its value to v2.
    if (root->right) v2 = root->right->data;

    // Check if the children sum property doesn't hold for the current value then return false
    if (v1 + v2 != root->data) return false;

    // Recursively check the children sum property for the left and right subtrees.
    // If either subtree violates the property, the entire tree violates it so we are using & operator
    return (isParentSum(root->left) && isParentSum(root->right));
}
```

# [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

## Approaches->
1. Simply cound all nodes. TC-> O(n)
2. [Approach 2](https://takeuforward.org/binary-tree/count-number-of-nodes-in-a-binary-tree/)

## Codes ->
1. 
```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root==NULL) return 0;

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```
2. 
```cpp
class Solution {
public:
    // Function to calculate the height of the left subtree
    int leftHeight(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + leftHeight(root->left);
    }

    // Function to calculate the height of the right subtree
    int rightHeight(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + rightHeight(root->right);
    }

    // Function to count the number of nodes in the complete binary tree
    int countNodes(TreeNode* root) {
        // Base case: if the root is NULL, there are no nodes
        if (root == NULL) return 0;

        // Calculate the height of the left and right subtrees
        int lh = leftHeight(root);
        int rh = rightHeight(root);

        // If the left and right subtrees have the same height, the tree is complete
        if (lh == rh) {
            // Calculate the number of nodes using the formula for a complete binary tree
            return pow(2, lh) - 1;
        }

        // If the left and right subtrees have different heights, recursively count nodes
        // in the left and right subtrees and add 1 for the current node
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

# [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/)

## Approach ->
Idea is very simple, perform the level order traversal and instead of just storing the node in the queue like you do in a normal level order traversal, store its vertical and level as well.
Take a map and store the vertical, level and values on that level. Finally formulate your answer. You might not have understood this explanation so check the link below. Solve this question while revising Himanshu, this is a code heavy question and not a concept heavy question.
[link](https://takeuforward.org/data-structure/vertical-order-traversal-of-binary-tree/)

## Code ->
```cpp
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Using a map to store the nodes with the same vertical and level position
        map<int, map<int, multiset<int>>> mp;
        // Using a queue for BFS traversal
        queue<pair<TreeNode*, pair<int, int>>> q;

        // Pushing the root node to the queue with initial vertical and level positions (0, 0)
        q.push({root, {0,0}});

        // BFS traversal
        while(q.size()){
            // Extracting the current node and its positions from the front of the queue
            auto p = q.front();
            TreeNode* node = p.first;
            int vertical = p.second.first, level = p.second.second;
            q.pop();

            // Inserting the node's value into the map at the corresponding positions
            mp[vertical][level].insert(node->val);

            // Enqueueing the left child with adjusted positions
            if(node->left) q.push({node->left, {vertical-1, level+1}});
            // Enqueueing the right child with adjusted positions
            if(node->right) q.push({node->right, {vertical+1, level+1}});
        }

        // Constructing the final result from the map
        vector<vector<int>> ans;

        for(auto x: mp){
            vector<int> temp;
            for(auto y: x.second){
                for(auto z: y.second){
                    temp.push_back(z);
                }
            }
            ans.push_back(temp);
        }

        return ans;
    }
};
```

# [Top View of Binary Tree](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1)

## Approach ->
Imagine if you do vertical order traversal and just include the first occurence of any vertical node then will that be your answer? yes. So in this question no need to keep track of your level, just keep track of your node and vertical and you're good.
Code this please while revising.

## Code ->
```cpp
class Solution {
public:
    // Function to return a list of nodes visible from the top view 
    // from left to right in Binary Tree.
    vector<int> topView(Node *root) {
        // Map to store the top view nodes with their vertical positions
        map<int, int> mp;
        // Queue for level order traversal
        queue<pair<Node*, int>> q;
        
        // Pushing the root node to the queue with its initial vertical position (0)
        q.push({root, 0});
        
        // Level order traversal
        while(q.size()) {
            auto it = q.front();
            q.pop();
            Node* curNode = it.first;
            int vertical = it.second;
            
            // If the vertical position is not present in the map, add the node's data to the map
            if (mp.find(vertical) == mp.end()) {
                mp[vertical] = curNode->data;
            }
            
            // Enqueue the left child with adjusted vertical position
            if (curNode->left) {
                q.push({curNode->left, vertical - 1});
            }
            
            // Enqueue the right child with adjusted vertical position
            if (curNode->right) {
                q.push({curNode->right, vertical + 1});
            }
        }
        
        // Constructing the final result from the map
        vector<int> ans;
        
        for (auto x : mp) {
            ans.push_back(x.second);
        }
        
        return ans;
    }
};
```

# [Bottom View of Binary Tree](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/1)

## Code ->
```cpp
class Solution {
  public:
    vector <int> bottomView(Node *root) {
        map<int, int> mp;
        queue<pair<Node*, int>> q;
        
        q.push({root, 0});
        
        // Level order traversal
        while(q.size()) {
            auto it = q.front();
            q.pop();
            Node* curNode = it.first;
            int vertical = it.second;
            
            // the last occuring vertical node would make the answer
            mp[vertical] = curNode->data;
            
            if (curNode->left) {
                q.push({curNode->left, vertical - 1});
            }
            
            if (curNode->right) {
                q.push({curNode->right, vertical + 1});
            }
        }
        
        vector<int> ans;
        
        for (auto x : mp) {
            ans.push_back(x.second);
        }
        
        return ans;
    }
};
```

# [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)

## Approach ->
[link](https://takeuforward.org/data-structure/maximum-width-of-a-binary-tree/)
Everything is same as of this approach in the link provided other than the "Prevention of Integer Overflow" part. I did the Prevention of Integer Overflow by taking unsigned long long integer values all around.

## Code ->
```cpp
class Solution {
public:
    // Using unsigned long long for larger range
    typedef unsigned long long ll;

    int widthOfBinaryTree(TreeNode* root) {
        // Queue to perform level order traversal
        queue<pair<TreeNode*, ll>> q;
        // Pushing the root node with its index (assuming 0-based indexing)
        q.push({root, 0});
        // Variable to store the maximum width
        ll ans = 0;

        while (q.size()) {
            // Get the number of nodes at the current level
            int size = q.size();
            // Get the index of the leftmost and rightmost nodes at the current level
            ll left =  q.front().second;
            ll right = q.back().second;

            // Update the maximum width
            ans = max(ans, right - left + 1);

            // Process nodes at the current level
            for (int i = 0; i < size; i++) {
                // Get the current node and its index
                TreeNode* node = q.front().first;
                ll idx = q.front().second;
                q.pop();

                // Enqueue the left child with adjusted index
                if (node->left) {
                    q.push({node->left, 2 * idx + 1});
                }

                // Enqueue the right child with adjusted index
                if (node->right) {
                    q.push({node->right, 2 * idx + 2});
                }
            }
        }

        // Convert the result to int before returning
        return static_cast<int>(ans);
    }
};
```

# [863. All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

## Approach->
While revising just code this out man, otherwise you won't be able to grasp it properly and might forget the approach during the interview.

Intuition:

Storing Parent Nodes:

The initial step involves creating a map (parent) to store the parent of each node in the binary tree using DFS.
This is done to facilitate easy traversal from child nodes to their parent nodes during the search for nodes at distance K.
Breadth-First Search (BFS) from Target Node:

The primary goal is to find nodes at a distance K from a given target node.
Utilize BFS to explore nodes level by level, starting from the target node.
A queue (q) is employed to keep track of nodes to be processed.
Exploring Neighbors:

Within each level of BFS:
Check and enqueue the left child if it exists and has not been visited.
Check and enqueue the right child if it exists and has not been visited.
Check and enqueue the parent node if it exists and has not been visited.
This ensures that the traversal explores the immediate neighbors of the current node.
Tracking Visited Nodes:

Maintain a set (visited) to keep track of nodes that have been visited to avoid redundant processing.
Distance K Adjustment:

After processing each level, decrement the distance k.
If k reaches 0, break out of the loop, as further exploration beyond distance K is unnecessary.
Collecting Result:

During BFS, collect the values of nodes at distance K in a vector (ans).
The final result vector contains the values of nodes at the desired distance.
Overall Strategy:

Utilizing DFS to store parent nodes aids in efficient traversal during BFS.
BFS explores the tree level by level, keeping track of visited nodes.
The algorithm stops when the desired distance K is reached or when all reachable nodes have been explored.

[Link to explanation vid](https://www.youtube.com/watch?v=1PMjfQl_UVQ)

## Code ->
```cpp
class Solution {
public:
    // Function to store the parent of each node in the binary tree using DFS.
    void storeParent(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& parent) {
        if (root == NULL) return;

        // Store the parent of the left child.
        if (root->left) parent[root->left] = root;
        // Recursively call the function for the left subtree.
        storeParent(root->left, parent);

        // Store the parent of the right child.
        if (root->right) parent[root->right] = root;
        // Recursively call the function for the right subtree.
        storeParent(root->right, parent);
    }

    // Main function to find nodes at distance K from the target node in the binary tree.
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        // Map to store the parent of each node in the binary tree.
        unordered_map<TreeNode*, TreeNode*> parent;
        // Populate the parent map using DFS.
        storeParent(root, parent);

        // Set to keep track of visited nodes.
        unordered_set<int> visited;
        // Queue for BFS traversal.
        queue<TreeNode*> q;
        // Start BFS from the target node.
        q.push(target);
        // Mark the target node as visited.
        visited.insert(target->val);

        // BFS traversal to find nodes at distance K.
        while (q.size()) {
            // Number of nodes at the current level.
            int size = q.size();

            // Process nodes at the current level.
            while (size--) {
                // Get the front node from the queue.
                TreeNode* curNode = q.front();
                q.pop();

                // Check left child and enqueue if not visited.
                if (curNode->left && !visited.count(curNode->left->val)) {
                    q.push(curNode->left);
                    visited.insert(curNode->left->val);
                }

                // Check right child and enqueue if not visited.
                if (curNode->right && !visited.count(curNode->right->val)) {
                    q.push(curNode->right);
                    visited.insert(curNode->right->val);
                }

                // Check parent and enqueue if not visited.
                if (parent.count(curNode) && !visited.count(parent[curNode]->val)) {
                    q.push(parent[curNode]);
                    visited.insert(parent[curNode]->val);
                }
            }

            // Decrement the distance after processing nodes at the current level.
            k--;
            // If distance becomes 0, break out of the loop.
            if (k == 0) break;
        }

        // Collect the values of nodes at distance K in a vector.
        vector<int> ans;
        while (q.size()) {
            ans.push_back(q.front()->val);
            q.pop();
        }

        // Return the result vector.
        return ans;
    }
};
```

# [Burning Tree](https://www.geeksforgeeks.org/problems/burning-tree/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## Approach ->
Very similar to the last q. Read the code and you'd understand. ( tip: don't miss your & operator if you pass your nodes by reference, that wasted half an hour of my time in debugging).

## Code ->
```cpp
class Solution {
public:
    // Helper function to map parent nodes using Depth-First Search (DFS).
    void mapParent(Node* root, unordered_map<Node*, Node*>& parent) {
        if (root == NULL) return;

        // Store the parent of the left child.
        if (root->left) parent[root->left] = root;
        mapParent(root->left, parent);

        // Store the parent of the right child.
        if (root->right) parent[root->right] = root;
        mapParent(root->right, parent);
    }

    // Helper function to find the target node in the tree.
    void findTargetNode(Node* root, int start, Node*& target) {
        if (root == NULL) return;

        // If the current node's data matches the target, set target to the current node.
        if (root->data == start) {
            target = root;
            return;
        }

        // Recursively search for the target in the left and right subtrees.
        findTargetNode(root->left, start, target);
        findTargetNode(root->right, start, target);
    }

    // Main function to calculate the minimum time to burn the entire tree.
    int minTime(Node* root, int start) {
        // Map to store the parent of each node.
        unordered_map<Node*, Node*> parent;
        mapParent(root, parent);

        // Variable to store the target node.
        Node* target = NULL;
        // Find the target node in the tree.
        findTargetNode(root, start, target);

        // If the target node is not found, return an appropriate value (here, -1).
        if (target == NULL) {
            return -1;
        }

        // Set to keep track of visited nodes.
        unordered_set<int> visited;
        // Queue for Breadth-First Search (BFS).
        queue<Node*> q;
        // Start BFS from the target node.
        q.push(target);
        // Mark the target node as visited.
        visited.insert(target->data);

        // Variable to store the minimum time required.
        int ans = 0;

        // BFS traversal to calculate the minimum time.
        while (!q.empty()) {
            int size = q.size();

            // Process nodes at the current level.
            while (size--) {
                // Get the front node from the queue.
                Node* cur = q.front();
                q.pop();

                // Check left child and enqueue if not visited.
                if (cur->left && !visited.count(cur->left->data)) {
                    visited.insert(cur->left->data);
                    q.push(cur->left);
                }
                // Check right child and enqueue if not visited.
                if (cur->right && !visited.count(cur->right->data)) {
                    visited.insert(cur->right->data);
                    q.push(cur->right);
                }
                // Check parent and enqueue if not visited.
                if (parent.count(cur) && !visited.count(parent[cur]->data)) {
                    visited.insert(parent[cur]->data);
                    q.push(parent[cur]);
                }
            }
            // Increment the time.
            ans++;
        }

        // Return the minimum time (decremented by 1 to exclude the initial level).
        return --ans;
    }
};
```

# [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

## Approach ->
The algorithm leverages the properties of preorder and inorder traversals to reconstruct a binary tree.

In the preorder traversal, the first element is always the root of the current subtree. The algorithm starts by picking the root from the preorder list and incrementing an index to move to the next element.

Then, it searches for the position of the root value in the inorder traversal. The elements to the left of this position correspond to the left subtree, and the elements to the right correspond to the right subtree.

Recursively, the algorithm constructs the left subtree with the elements from the left side of the inorder position and the right subtree with the elements from the right side. This process is repeated for each subtree until the entire binary tree is reconstructed.

The key idea is that in each recursive call, the algorithm focuses on a specific root, finds its position in the inorder traversal, and uses this information to divide and conquer, constructing the left and right subtrees of the current root.

[Video explanation](https://www.youtube.com/watch?v=G5c1wM3Kpuw)

## Code ->
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    // Helper function to construct the binary tree recursively.
    TreeNode* solve(vector<int>& preorder, vector<int>& inorder, int start, int end, int &idx) {
        // Base case: If the start index exceeds the end index, return NULL.
        // note: start will always be greater than end if there is no element left in inorder vector to add in the root. dry run
        if (start > end) {
            return nullptr;
        }

        // Extract the current root value from the preorder traversal.
        int rootVal = preorder[idx++];
        
        // Find the index of the root value in the inorder traversal.
        int i = start;
        for (; i <= end; i++) {
            if (inorder[i] == rootVal) {
                break;
            }
        }

        // Create a new TreeNode with the current root value.
        TreeNode* root = new TreeNode(rootVal);

        // Recursively construct the left subtree with the range (start to i-1) in the inorder traversal.
        root->left = solve(preorder, inorder, start, i - 1, idx);
        // Recursively construct the right subtree with the range (i+1 to end) in the inorder traversal.
        root->right = solve(preorder, inorder, i + 1, end, idx);

        // Return the constructed root node.
        return root;
    }

    // Main function to build the binary tree using preorder and inorder traversals.
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        int idx = 0; // Index to keep track of the current root value in the preorder traversal.
        
        // Call the solve function to construct the entire binary tree.
        return solve(preorder, inorder, 0, n - 1, idx);
    }
};
```
# [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

## Approach ->
In this question we have to Invert the binary tree.
So we use Post Order Treversal in which first we go in Left subtree and then in Right subtree then we return back to Parent node.
When we come back to the parent node we swap it's Left subtree and Right subtree.
Don't forget to revise how to construct a tree from scratch because in the real interview you won't be given to just write the function.

## Code ->
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // Helper function to invert the binary tree recursively
    void helper(TreeNode* root) {
        // Base case: If the current node is NULL, return
        if (!root) return;

        // Recursively invert the left and right subtrees
        helper(root->left);
        helper(root->right);

        // Swap the left and right children of the current node
        swap(root->left, root->right);
    }

    // Main function to invert the binary tree and return its root
    TreeNode* invertTree(TreeNode* root) {
        // Call the helper function to perform the inversion
        helper(root);

        // Return the root of the inverted binary tree
        return root;
    }
};
```

# [572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/description/)

## Approach ->
Traverse in the root and find if at some position its value is equal to the first value in subRoot tree. If yes then call a check function to check if both of the trees are of same values or not
## Code ->
```cpp
class Solution {
public:
    bool checkEqual(TreeNode* root, TreeNode* subRoot){
        if(root==NULL && subRoot==NULL) return true;
        if(root==NULL || subRoot==NULL) return false;
        if(root->val != subRoot->val) return false;

        return checkEqual(root->left, subRoot->left) && checkEqual(root->right, subRoot->right);

    }
    void preorder(TreeNode* root, TreeNode* subRoot, bool &ans){
        if(root==NULL) return;
        if(root->val == subRoot->val){
            bool isEqual = checkEqual(root, subRoot);
            if(isEqual){
                ans = true;
                return;
            }
        }
        preorder(root->left, subRoot, ans);
        preorder(root->right, subRoot, ans);
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        bool ans = false;
        preorder(root, subRoot, ans);
        return ans;
    }
};
```
TC -> O(m * n)
