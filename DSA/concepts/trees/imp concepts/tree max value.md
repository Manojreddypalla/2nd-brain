```cpp
#include<iostream>
#include<queue>
#include<climits>

using namespace std;

class Node {
public:

    int val;
    Node* left;
    Node* right;

    Node(int value)
    {
        val = value;
        left = NULL;
        right = NULL;
    }
};

int treeMaxValue(Node* root)
{
    int largest = INT_MIN;

    queue<Node*> q;

    q.push(root);

    while(!q.empty())
    {
        Node* current = q.front();
        q.pop();

        if(current->val > largest)
        {
            largest = current->val;
        }

        if(current->left)
        {
            q.push(current->left);
        }

        if(current->right)
        {
            q.push(current->right);
        }
    }

    return largest;
}

int main()
{
    Node* a = new Node(10);
    Node* b = new Node(4);
    Node* c = new Node(7);
    Node* d = new Node(20);
    Node* e = new Node(8);

    a->left = b;
    a->right = c;

    b->left = d;
    b->right = e;

    cout << treeMaxValue(a);

    return 0;
}
```