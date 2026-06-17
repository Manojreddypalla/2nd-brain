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
#include<iostream>
#include<queue>

usingnamespacestd;

classNode {
public:

int val;
Node* left;
Node* right;

Node(intvalue)
    {
val =value;
left =NULL;
right =NULL;
    }
};

inttreeSum(Node*root)
{
if(root==NULL)
    {
return0;
    }

inttotalSum =0;

queue<Node*>q;

q.push(root);

while(!q.empty())
    {
Node*current =q.front();
q.pop();

totalSum+=current->val;

if(current->left)
        {
q.push(current->left);
        }

if(current->right)
        {
q.push(current->right);
        }
    }

returntotalSum;
}

intmain()
{
Node*a =newNode(2);
Node*b =newNode(4);
Node*c =newNode(5);
Node*d =newNode(1);
Node*e =newNode(3);

a->left =b;
a->right =c;

b->left =d;
b->right =e;

cout<<treeSum(a);

return0;
}
```