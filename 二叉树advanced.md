# 513.找树左下角的值
```cpp
class Solution {
public:
    int maxDepth = INT_MIN;
    int result;
    void traversal(TreeNode* cur,int depth){
        if(!cur->left && !cur->right ){
            if(depth>maxDepth){
                maxDepth = depth;
                result = cur->val;
            }
        }
        if(cur->left){
            depth++;
            traversal(cur->left,depth);
            depth--;
        }
        if(cur->right){
            depth++;
            traversal(cur->right,depth);
            depth--;
        }
    }

    int findBottomLeftValue(TreeNode* root) {
        int depth = 0;
        traversal(root,depth);
        return result;
    }
};
```
- 递归的时候注意回溯

# 112.路径总和
```cpp
class Solution {
public:
    bool traversal(TreeNode* node, int sum, int target){
        sum = sum + node->val;
        // cout<<node->val<<", "<<!node->left<<", "<<!node->right<<", "<<target<<","<<sum<<endl;

        if(!node->left && !node->right){
            if(sum == target){
                return true;
            }else{
                return false;
            }
        }
        if(node->left){
            bool flag= traversal(node->left,sum,target);
            if(flag){
                return true;
            }
            // sum = sum - node->left->val;
        }
        if(node->right){
             bool flag= traversal(node->right,sum,target);
            if(flag){
                return true;
            }
            // return traversal(node->right,sum,target);
            // sum = sum - node->right->val;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(!root) return false;
        return traversal(root,0,targetSum);
    }
};
```
- 回溯的时候注意变量是全局变量还是局部变量

# 106.从中序和后序遍历构造二叉树
```cpp
class Solution {
public:
    TreeNode* traversal(vector<int>& inorder,vector<int>& postorder){
        if(postorder.size()==0) return NULL;


        int rootValue = postorder[postorder.size()-1];
        int index = 0;
        for(;index<inorder.size();index++){
            if(inorder[index] == rootValue) break;
        }
        TreeNode* root = new TreeNode(rootValue);
        if(postorder.size()==1) return root;

        vector<int> inorder_first(inorder.begin(),inorder.begin()+index);
        vector<int> inorder_second(inorder.begin()+index+1,inorder.end());

        postorder.resize(postorder.size()-1);
        vector<int> postorder_first(postorder.begin(),postorder.begin()+inorder_first.size());
        vector<int> postorder_second(postorder.begin()+inorder_first.size(),postorder.end());

        root->left = traversal(inorder_first,postorder_first);
        root->right =  traversal(inorder_second,postorder_second);
        return root;
    }   

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) return NULL;
        return traversal(inorder, postorder);
    }
};
```
- 划分区间
