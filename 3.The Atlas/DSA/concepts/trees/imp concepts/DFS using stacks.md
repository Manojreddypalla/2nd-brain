
```cpp
#include<iostream>
#include<vector>
#include<stack>

using namespace std;

class Node {
public:

    char val;
    Node* left;
    Node* right;

    Node(char value)
    {
        val = value;
        left = NULL;
        right = NULL;
    }
};

vector<char> depthFirstValues(Node* root)
{
    if(root == NULL)
    {
        return {};
    }

    vector<char> result;

    stack<Node*> st;

    st.push(root);

    while(!st.empty())
    {
        Node* current = st.top();
        st.pop();

        result.push_back(current->val);

        if(current->right)
        {
            st.push(current->right);
        }

        if(current->left)
        {
            st.push(current->left);
        }
    }

    return result;
}

int main()
{
    Node* a = new Node('a');
    Node* b = new Node('b');
    Node* c = new Node('c');
    Node* d = new Node('d');
    Node* e = new Node('e');
    Node* f = new Node('f');

    a->left = b;
    a->right = c;

    b->left = d;
    b->right = e;

    c->right = f;

    vector<char> ans = depthFirstValues(a);

    for(char x : ans)
    {
        cout << x << " ";
    }

    return 0;
}
```