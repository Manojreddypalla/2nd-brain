```cpp
#include<iostream>
#include<vector>
#include<queue>

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

vector<char> breadthFirstValues(Node* root)
{
    if(root == NULL)
    {
        return {};
    }

    vector<char> values;

    queue<Node*> q;

    q.push(root);

    while(!q.empty())
    {
        Node* current = q.front();

        q.pop();

        values.push_back(current->val);

        if(current->left != NULL)
        {
            q.push(current->left);
        }

        if(current->right != NULL)
        {
            q.push(current->right);
        }
    }

    return values;
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

    vector<char> ans = breadthFirstValues(a);

    for(char x : ans)
    {
        cout << x << " ";
    }

    return 0;
}
```