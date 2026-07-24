This line is enough:

```
totalSum+=current->val;
```

because EVERY node eventually gets pushed into queue.

So:

- if node enters queue
- it will later become `current`
- then its value gets added

Meaning ALL nodes get counted automatically.

# Full C++ Code

```cpp
#include <iostream>
#include <queue>

using namespace std;

class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node(int value) {
        val = value;
        left = NULL;
        right = NULL;
    }
};

int treeSum(Node* root) {
    if (root == NULL) {
        return 0;
    }

    int totalSum = 0;

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        Node* current = q.front();
        q.pop();

        totalSum += current->val;

        if (current->left) {
            q.push(current->left);
        }

        if (current->right) {
            q.push(current->right);
        }
    }

    return totalSum;
}

int main() {
    Node* a = new Node(2);
    Node* b = new Node(4);
    Node* c = new Node(5);
    Node* d = new Node(1);
    Node* e = new Node(3);

    a->left = b;
    a->right = c;

    b->left = d;
    b->right = e;

    cout << treeSum(a);

    return 0;
}
```