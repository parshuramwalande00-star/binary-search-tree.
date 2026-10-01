# binary-search-tree
#include <iostream>
using namespace std;

// Template Node
template <typename T>
struct Node {
    T data;
    Node<T>* left;
    Node<T>* right;

    Node(T value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

// Template BST Class
template <typename T>
class BST {
private:
    Node<T>* root;

    // Insert helper
    Node<T>* insert(Node<T>* node, T value) {
        if (node == nullptr)
            return new Node<T>(value);

        if (value < node->data)
            node->left = insert(node->left, value);
        else if (value > node->data)
            node->right = insert(node->right, value);

        return node;
    }

    // Search helper
    bool search(Node<T>* node, T value) {
        if (node == nullptr)
            return false;

        if (node->data == value)
            return true;

        if (value < node->data)
            return search(node->left, value);

        return search(node->right, value);
    }

    // Find minimum node
    Node<T>* findMin(Node<T>* node) {
        while (node != nullptr && node->left != nullptr)
            node = node->left;

        return node;
    }

    // Delete helper
    Node<T>* remove(Node<T>* node, T value) {
        if (node == nullptr)
            return nullptr;

        if (value < node->data) {
            node->left = remove(node->left, value);
        }
        else if (value > node->data) {
            node->right = remove(node->right, value);
        }
        else {
            // No child
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }

            // Only right child
            if (node->left == nullptr) {
                Node<T>* temp = node->right;
                delete node;
                return temp;
            }

            // Only left child
            if (node->right == nullptr) {
                Node<T>* temp = node->left;
                delete node;
                return temp;
            }

            // Two children
            Node<T>* temp = findMin(node->right);
            node->data = temp->data;
            node->right = remove(node->right, temp->data);
        }

        return node;
    }

    // In-order traversal
    void inOrder(Node<T>* node) {
        if (node == nullptr)
            return;

        inOrder(node->left);
        cout << node->data << " ";
        inOrder(node->right);
    }

    // Pre-order traversal
    void preOrder(Node<T>* node) {
        if (node == nullptr)
            return;

        cout << node->data << " ";
        preOrder(node->left);
        preOrder(node->right);
    }

    // Post-order traversal
    void postOrder(Node<T>* node) {
        if (node == nullptr)
            return;

        postOrder(node->left);
        postOrder(node->right);
        cout << node->data << " ";
    }

    // Destructor helper
    void destroyTree(Node<T>* node) {
        if (node == nullptr)
            return;

        destroyTree(node->left);
        destroyTree(node->right);

        delete node;
    }

public:

    // Constructor
    BST() {
        root = nullptr;
    }

    // Destructor
    ~BST() {
        destroyTree(root);
    }

    // Public insert
    void insert(T value) {
        root = insert(root, value);
    }

    // Public search
    bool search(T value) {
        return search(root, value);
    }

    // Public delete
    void remove(T value) {
        root = remove(root, value);
    }

    // Traversals
    void inOrder() {
        inOrder(root);
        cout << endl;
    }

    void preOrder() {
        preOrder(root);
        cout << endl;
    }

    void postOrder() {
        postOrder(root);
        cout << endl;
    }
};


// Main Function
int main() {

    BST<int> tree;

    int choice, value;

    do {
        cout << "\n==============================\n";
        cout << "      BINARY SEARCH TREE\n";
        cout << "==============================\n";
        cout << "1. Insert\n";
        cout << "2. Search\n";
        cout << "3. Delete\n";
        cout << "4. In-Order Traversal\n";
        cout << "5. Pre-Order Traversal\n";
        cout << "6. Post-Order Traversal\n";
        cout << "7. Exit\n";
        cout << "==============================\n";

        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {

        case 1:
            cout << "Enter value: ";
            cin >> value;
            tree.insert(value);
            cout << "Value inserted successfully.\n";
            break;

        case 2:
            cout << "Enter value to search: ";
            cin >> value;

            if (tree.search(value))
                cout << "Value found in BST.\n";
            else
                cout << "Value not found.\n";

            break;

        case 3:
            cout << "Enter value to delete: ";
            cin >> value;

            if (tree.search(value)) {
                tree.remove(value);
                cout << "Value deleted successfully.\n";
            }
            else {
                cout << "Value not found.\n";
            }

            break;

        case 4:
            cout << "In-Order: ";
            tree.inOrder();
            break;

        case 5:
            cout << "Pre-Order: ";
            tree.preOrder();
            break;

        case 6:
            cout << "Post-Order: ";
            tree.postOrder();
            break;

        case 7:
            cout << "Exiting program...\n";
            break;

        default:
            cout << "Invalid choice!\n";
        }

    } while (choice != 7);

    return 0;
}