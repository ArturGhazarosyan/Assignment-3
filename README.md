# Assignment-3
#include <iostream>
#include <cstdlib>
#include <cassert>
using namespace std;
template <class T>

struct tree_node
{
    tree_node <T>* left;
    tree_node <T>* right;
    T data;
};
template <class T>

class BinarySearchTree
{
private:
       tree_node <T>* root;
protected:
    tree_node<T>* find(const T& item, tree_node<T>*&parent) const;
    
public:
    BinarySearchTree()
    {
        root = NULL;
    };
    ~BinarySearchTree() {};
    
    bool isEmpty() const { return root==NULL; }
    void inorder_traverse(T&);
    void preorder_traverse(T&);
    void postorder_traverse(T&);
    void insert(T&);
    void remove(T&);
    int get_height (tree_node<T>*);
    
    
};
template <class T>
void BinarySearchTree<T>:: inorder_traverse(T& p)
{
    if(p != NULL)
    {
        if(p->left) inorder_traverse(p->left);
        cout<<" "<<p->data<<" ";
        if(p->right) inorder_traverse(p->right);
    }
    else return;
}
template <class T>

void BinarySearchTree<T>::preorder_traverse(T& p)
{
    if(p != NULL)
    {
        cout<<" "<<p->data<<" ";
        if(p->left) preorder_traverse(p->left);
        if(p->right) preorder_traverse(p->right);
    }
    else return;
}
template <class T>

void BinarySearchTree<T>::postorder_traverse(T& p)
{
    if(p != NULL)
    {
        if(p->left) postorder_traverse(p->left);
        if(p->right) postorder_traverse(p->right);
        cout<<" "<<p->data<<" ";
    }
    else return;
}
template <class T>

void BinarySearchTree<T>::insert(T& d)
{
    tree_node<T>* t = new tree_node<T>;
    tree_node<T>* parent;
    t->data = d;
    t->left = NULL;
    t->right = NULL;
    parent = NULL;
    
    if(isEmpty()) root = t;
    else
    {
        
        tree_node<T>* curr;
        curr = root;
        while(curr)
        {
            parent = curr;
            if(t->data > curr->data) curr = curr->right;
            else curr = curr->left;
        }
        
        if(t->data < parent->data)
            parent->left = t;
        else
            parent->right = t;
    }
}
template <class T>
void BinarySearchTree<T>::remove(T& d)
{
    
    bool found = false;
    if(isEmpty())
    {
        cout<<" This Tree is empty! "<<endl;
        return;
    }
    
    tree_node<T>* curr;
    tree_node<T>* parent;
    curr = root;
    
    while(curr != NULL)
    {
        if(curr->data == d)
        {
            found = true;
            break;
        }
        else
        {
            parent = curr;
            if(d>curr->data) curr = curr->right;
            else curr = curr->left;
        }
    }
    if(!found)
    {
        cout<<" Data not found! "<<endl;
        return;
    }
    
     // 3 cases are possible
    // 1. we remove a leaf node
    // 2. we remove a node with 1 child
    // 3. we remove a node with 2 child
    
    // 1-st case
    if((curr->left == NULL && curr->right != NULL)|| (curr->left != NULL
                                                      && curr->right == NULL))
    {
        if(curr->left == NULL && curr->right != NULL)
        {
            if(parent->left == curr)
            {
                parent->left = curr->right;
                delete curr;
            }
            else
            {
                parent->right = curr->right;
                delete curr;
            }
        }
        else // left child exists, the right one no and the reverse
        {
            if(parent->left == curr)
            {
                parent->left = curr->left;
                delete curr;
            }
            else
            {
                parent->right = curr->left;
                delete curr;
            }
        }
        return;
    }
    
    
    if( curr->left == NULL && curr->right == NULL)
    {
        if(parent->left == curr) parent->left = NULL;
        else parent->right = NULL;
        delete curr;
        return;
    }
    
    
    //3-rd case
    // we can replace node with smallest value in right subtree
    if (curr->left != NULL && curr->right != NULL)
    {
        tree_node<T>* chkr;
        chkr = curr->right;
        if((chkr->left == NULL) && (chkr->right == NULL))
        {
            curr = chkr;
            delete chkr;
            curr->right = NULL;
        }
        else // the right child has children
        {
            
            
            if((curr->right)->left != NULL)
            {
                tree_node<T>* lcurr;
                tree_node<T>* lcurrp;
                lcurrp = curr->right;
                lcurr = (curr->right)->left;
                while(lcurr->left != NULL)
                {
                    lcurrp = lcurr;
                    lcurr = lcurr->left;
                }
                curr->data = lcurr->data;
                delete lcurr;
                lcurrp->left = NULL;
            }
            else
            {
                tree_node<T>* tmp;
                tmp = curr->right;
                curr->data = tmp->data;
                curr->right = tmp->right;
                delete tmp;
            }
            
        }
        return;
    }
    
}
 template <class T>
tree_node<T>*BinarySearchTree<T>::find(const T& item, tree_node<T>* &parent)const
{ tree_node<T>*t=root;
    parent=NULL;
    while(t!=NULL)
    {
        if (item==t->info)
            break;
       
        {
            parent=t;
            if(item<t->info)
                t=t->left;
            else
                t=t->right;
        }
    }
    return t;
};
// function decide the bigger of 2 numbers
int max (int x, int y)
{
    return (x>y)?x:y;
};

template <class T>
int BinarySearchTree<T>::get_height(tree_node<T>*i)
{
    if(i==NULL)
    
        return -1;
    return max (get_height(i->left),get_height(i->right))+1;
    
}

