Найти среднюю из вершин дерева, у которых высоты поддеревьев не равны, а число потомков в правом и левом поддеревьях совпадает. Удалить её (правым удалением), если такая вершина существует.

Если у вершины отсутствует некоторое поддерево, то его высоту полагаем равной −1, а число вершин этого поддерева — 0.

**Формат входных данных**

Входной файл содержит последовательность чисел — ключи вершин в порядке добавления в дерево.

**Формат выходных данных**

Выходной файл должен содержать последовательность ключей вершин, полученную прямым левым обходом итогового дерева.

**Пример**

in.txt

10

8

12

4

9

11

13

3

6

2

5

7

1

out.txt

10

8

5

3

2

1

6

7

9

12

11

13

```
#include <iostream>
#include <fstream>
#include <string>
#include <algorithm>
#include <vector>
using namespace std;
ofstream fout("out.txt");

class TNode
{
public:
    TNode(int key) : Key(key), Height(-1), Sons(0), Left(0), Right(0)
    {
    }

    int Key;
    int Height;
    int Sons;
    TNode* Left = nullptr;
    TNode* Right = nullptr;
};

class TTree
{
public:
    TTree()
        : Root(0)
    {
    }

    ~TTree()
    {
        DestroyNode(Root);
    }

    TNode* Root;

    void Insert(int x)
    {
        TNode** cur = &Root;
        while (*cur)
        {
            TNode& node = **cur;
            if (x < node.Key)
            {
                cur = &node.Left;
            }
            else if (x > node.Key)
            {
                cur = &node.Right;
            }
            else
            {
                return;
            }
        }
        *cur = new TNode(x);
    }

    TNode* DeleteNum(TNode* root, int key)
    {
        if (root == nullptr)
        {
            return root;
        }
        if (root->Key > key)
        {
            root->Left = DeleteNum(root->Left, key);
            return root;
        }
        else if (root->Key < key)
        {
            root->Right = DeleteNum(root->Right, key);
            return root;
        }

        if (root->Left == nullptr)
        {
            return root->Right;
        }
        else if (root->Right == nullptr)
        {
            return root->Left;
        }
        else
        {
            int min_key = MinValue(root->Right)->Key;
            root->Right = DeleteNum(root->Right, min_key);
            root->Key = min_key;
            return root;
        }
    }

    void DeleteNum(int key) {
        TNode** pp = &Root;
        Root = DeleteNum(*pp, key);
    }


    TNode* MinValue(TNode* node)
    {
        if (node->Left) {
            return MinValue(node->Left);
        }
        else {
            return node;
        }
    }

    void HandS(TNode* node)
    {
        if (!node)
        {
            return;
        }
        HandS(node->Left);
        HandS(node->Right);
        if (!node->Left && !node->Right)
        {
            node->Height = 0;
            node->Sons = 0;
            return;
        }
        else if (!node->Right && node->Left || !node->Left && node->Right)
        {
            if (node->Right)
            {
                node->Height = node->Right->Height + 1;
                node->Sons = node->Right->Sons + 1;
                return;
            }
            else if (node->Left)
            {
                node->Height = node->Left->Height + 1;
                node->Sons = node->Left->Sons + 1;
                return;
            }
        }
        else if (node->Right && node->Left)
        {
            node->Height = max(node->Left->Height, node->Right->Height) + 1;
            node->Sons = node->Right->Sons + node->Left->Sons + 2;
            return;
        }
        return;
    }

    void FlagNode(TNode* node, vector<int> &vec)
    {
        if (!node)
        {
            return;
        }
        FlagNode(node->Left, vec);
        if (node->Left && node->Right)
        {
            if (node->Left->Height != node->Right->Height && node->Left->Sons == node->Right->Sons)
            {
                vec.push_back(node->Key);
            }
        }
        FlagNode(node->Right, vec);
    }

    void straight(TNode* root)
    {
        if (!root)
        {
            return;
        }
        fout << root->Key << endl;
        straight(root->Left);
        straight(root->Right);
    }


private:
    static void DestroyNode(TNode* node)
    {
        if (node)
        {
            DestroyNode(node->Left);
            DestroyNode(node->Right);
            delete node;
        }
    }
};


int main()
{
    TTree tree;
    string line;
    ifstream in;
    in.open("in.txt");
    if (in.is_open())
    {
        while (getline(in, line))
        {
            tree.Insert(stoi(line));
        }
    }
    else
    {
        cout << "error" << endl;
    }
    in.close();
    tree.HandS(tree.Root);
    vector<int> flags;
    tree.FlagNode(tree.Root, flags);
    if (flags.size() % 2 == 1)
    {
        tree.DeleteNum(flags[flags.size() / 2]);
    }
    tree.straight(tree.Root);
    return 0;
}
```
