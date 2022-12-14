# Является ли бинарное дерево поисковым?

**Условие:**

Задано бинарное дерево. Необходимо проверить, является ли оно поисковым.

Будем предполагать, что в бинарном поисковом дереве могут быть вершины с одинаковыми ключами. Тогда, для того, чтобы дерево было поисковым, должно выполняться следующее требование: для каждой вершины $x$ все ключи в левом поддереве вершины $x$ меньше ключа вершины $x$, а все ключи в правом поддереве больше либо равны ключу вершины $x$.

**Формат входных данных**

Первая строка входного файла содержит единственное целое число $n$ (1 <= n <= 8 * 10^5) – количество вершин в дереве. Следующая строка содержит одно целое число $m$ (-2^{31} <= m <= 2^{31} - 1) – значение в корневой вершине дерева.В каждой из последующих $n - 1$ строк через пробелы перечисляются три параметра m, p и c, которые задают какую-либо вершину дерева. m – целое число (-2^{31} <= m <= 2^{31} - 1), значение, записанное в вершине. p – целое число (1 <= p <= n - 1), номер строки входного файла, в которой был задан родитель текущей вершины (нумерация строк с нуля). Гарантируется, что p меньше, чем номер текущей строки. c может принимать одно из двух значений L или R. Значение L указывает на то, что текущая вершина присоединена к родительской слева, R – справа.Гарантируется, что совокупность всех строк задает корректное бинарное дерево.

**Формат выходных данных**

В единственной строке выведите YES, если заданное дерево является бинарным деревом поиска, и NO в противном случае.

Пример:

bst.in

1

2 1 L

4 2 L

3 1 R

5 2 R

6 4 L

7 4 R

bst.out

NO

```
#include <iostream>
#include <fstream>
#include <climits>
using namespace std;

int main()
{
    ofstream fout("bst.out");
    ifstream in("bst.in");
    int n;
    in >> n;
    int* key = new int[n];
    int* parent = new int[n - 1];
    long long* border = new long long[n * 2];
    char* lr = new char[n - 1];
    in >> key[0];
    for (int i = 0; i < n - 1; i++)
    {
        in >> key[i + 1];
        int temp;
        in >> temp;
        parent[i] = temp - 1;
        in >> lr[i];
    }
    border[0] = LLONG_MIN;
    border[1] = LLONG_MAX;
    int k = 2;
    for (int i = 0; i < n - 1; i++)
    {
        if (lr[i] == 'L')
        {
            border[k] = border[parent[i] * 2];
            border[k + 1] = key[parent[i]];
        }
        else if (lr[i] == 'R')
        {
            border[k] = key[parent[i]];
            border[k + 1] = border[parent[i] * 2 + 1];
        }
        if (border[k] == LLONG_MIN)
        {
            if (key[i + 1] <= border[k] || key[i + 1] >= border[k + 1])
            {
                fout << "NO";
                return 0;
            }
        }
        else if (border[k] != LLONG_MIN)
        {
            if (key[i + 1] < border[k] || key[i + 1] >= border[k + 1])
            {
                fout << "NO";
                return 0;
            }
        }
        k += 2;
    }
    fout << "YES";
    return 0;
}
```
