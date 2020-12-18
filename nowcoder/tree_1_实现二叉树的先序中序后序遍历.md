[实现二叉树先序、中序和后序遍历](https://www.nowcoder.com/practice/566f7f9d68c24691aa5abd8abefa798c?tpId=101&&tqId=33229&rp=1&ru=/ta/programmer-code-interview-guide&qru=/ta/programmer-code-interview-guide/question-ranking)

~~~c++
# include<iostream>
# include<vector>
# include<stack>

using namespace std;

struct TreeNode{
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr){};
    TreeNode(int v, TreeNode* l, TreeNode* r) : val(v), left(l), right(r){}
};

void createTree(TreeNode* root){
    int fa;
    int lch;
    int rch;
    cin >> fa >> lch >> rch;
    if(lch!=0){
        root->left = new TreeNode(lch);
        createTree(root->left);
    }
    if(rch!=0){
        root->right = new TreeNode(rch);
        createTree(root->right);
    }
}

//先序遍历
vector<int> preorderTraversal_1(TreeNode* root) {
    vector<int> ans;
    if(root == nullptr){
        return ans;
    }

    stack<TreeNode*> helpStack;
    helpStack.emplace(root);

    while(!helpStack.empty()){
        root = helpStack.top();
        ans.push_back(root->val);
        helpStack.pop();

        if(root->right!=nullptr){
            helpStack.emplace(root->right);
        }
        if(root->left!=nullptr){
            helpStack.emplace(root->left);
        }
    }
    return ans;
}
// 先序遍历 Morris
void morris_1(TreeNode* head, vector<int>& ans){
    if(head==nullptr){
        return;
    }
    TreeNode* curr = head;
    TreeNode* mostRight = nullptr;

    while(curr!=nullptr){
        mostRight = curr->left;
        if(mostRight!=nullptr){
            while(mostRight->right!=nullptr && mostRight->right!=curr){
                mostRight = mostRight->right;
            }
            if(mostRight->right == nullptr){
                mostRight->right = curr;
                ans.push_back(curr->val);
                curr = curr->left;
                continue;
            }else{
                mostRight->right = nullptr;
            }
        }else{
            ans.push_back(curr->val);
        }
        curr = curr->right;
    }
}
vector<int> preorderTraversal_2(TreeNode* root) {
    vector<int> ans;
    morris_1(root, ans);
    return ans;
}

// ------------------------------------------------------------
// 中序遍历
vector<int> inorderTraversal_1(TreeNode* root) {
    vector<int> ans;
    if(root==nullptr){
        return ans;
    }
    TreeNode* head = root;
    stack<TreeNode*> helpStack;
    while(!helpStack.empty() || head!=nullptr){
        if(head!=nullptr){
            helpStack.emplace(head);
            head = head->left;
        }else{
            head = helpStack.top();
            ans.push_back(head->val);
            helpStack.pop();
            head = head->right;
        }
    }
    return ans;
}
// 中序遍历 Morris
void morris_2(TreeNode* root, vector<int>& ans){
    if(root==nullptr){
        return;
    }
    TreeNode* curr = root;
    TreeNode* mostRight = nullptr;
    while(curr!=nullptr){
        mostRight = curr->left;
        if(mostRight!=nullptr){
            while(mostRight->right!=nullptr && mostRight->right!=curr){
                mostRight = mostRight->right;
            }
            if(mostRight->right == nullptr){
                mostRight->right = curr;
                curr = curr->left;
                continue;
            }else{
                mostRight->right = nullptr;
            }
        }
        ans.push_back(curr->val);
        curr = curr->right;
    }
}
vector<int> inorderTraversal_2(TreeNode* root) {
    vector<int> ans;
    morris_2(root, ans);
    return ans;
}

// ------------------------------------------------------------
// 后序遍历
vector<int> postorderTraversal_1(TreeNode* root) {
    vector<int> ans;
    if(root==nullptr){
        return ans;
    }

    stack<TreeNode*> s1;
    stack<TreeNode*> s2;
    s1.emplace(root);

    while(!s1.empty()){
        root = s1.top();
        s2.emplace(root);
        s1.pop();

        if(root->left!=nullptr){
            s1.emplace(root->left);
        }
        if(root->right!=nullptr){
            s1.emplace(root->right);
        }
    }
    while(!s2.empty()){
        root = s2.top();
        ans.push_back(root->val);
        s2.pop();
    }

    return ans;
}

vector<int> postorderTraversal_2(TreeNode* root) {
    vector<int> ans;
    if(root==nullptr){
        return ans;
    }

    stack<TreeNode*> stk;
    stk.emplace(root);
    TreeNode* c = nullptr; //存放当前栈中的栈顶节点

    while(!stk.empty()){
        c = stk.top();
        if(c->left!=nullptr && root!=c->left && root!=c->right){
            stk.emplace(c->left);
        }else if(c->right!=nullptr && root!=c->right){
            stk.emplace(c->right);
        }else{
            ans.push_back(stk.top()->val);
            stk.pop();
            root = c;
        }
    }
    return ans;
}

TreeNode* reverseEdge(TreeNode* from){
    TreeNode* pre = nullptr;
    TreeNode* next = nullptr;
    while(from!=nullptr){
        next = from->right;
        from->right = pre;
        pre = from;
        from = next;
    }
    return pre;
}
void printEdge(TreeNode* node, vector<int>& ans){
    TreeNode* tail = reverseEdge(node);
    TreeNode* curr = tail;
    while(curr!=nullptr){
        ans.push_back(curr->val);
        curr = curr->right;
    }
    reverseEdge(tail);
} 
void morris_3(TreeNode* root, vector<int>& ans){
    if(root==nullptr){
        return;
    }
    TreeNode* curr = root;
    TreeNode* mostRight = nullptr;
    while(curr!=nullptr){
        mostRight = curr->left;
        if(mostRight!=nullptr){
            while(mostRight->right!=nullptr && mostRight->right!=curr){
                mostRight = mostRight->right;
            }
            if(mostRight->right == nullptr){
                mostRight->right = curr;
                curr = curr->left;
                continue;
            }else{
                mostRight->right = nullptr;
                printEdge(curr->left, ans);
            }
        }
        curr = curr->right;
    }
    printEdge(root, ans);
}
vector<int> postorderTraversal_3(TreeNode* root) {
    vector<int> ans;
    morris_3(root, ans);
    return ans;
}

int main(){
    int n;
    int root;
    cin >> n >> root;
    TreeNode* head = new TreeNode(root);
    createTree(head);
    vector<int> pre;
    vector<int> in;
    vector<int> pos;
    pre = preorderTraversal_2(head);
    in = inorderTraversal_2(head);
    pos = postorderTraversal_3(head);
    for(auto x : pre){
        cout << x <<" ";
    }
    cout << endl;
    
    for(auto x : in){
        cout << x <<" ";
    }
    cout << endl;
    
    for(auto x : pos){
        cout << x <<" ";
    }
    cout << endl;
}
~~~

