# LCS

Даны две последовательности $A$ и $B$, каждая имеет длину $n$.

Найти наибольшее $k$, для которого существуют две последовательности индексов $0 \le i_1 < i_2 < \ldots < i_k < n$ и $0 \le j_1 < j_2 < \ldots < j_k < n$, что $A_{i_1} = B_{j_1}$, $A_{i_2} = B_{j_2}$, …, $A_{i_k} = B_{j_k}$.

Также нужно найти и сами последовательности индексов.

**Формат входных данных**

В первой строке записано число $n$ (1 <= n <= 1000), длина последовательностей $A$ и $B$. Во второй строке содержится $n$ целых чисел $a_i$ (1 <= a_i <= 1000) — элементы последовательности $A$. В третьей строке содержатся $n$ целых чисел $b_j$ (1 <= b_j <= 1000) — элементы последовательности $B$.

**Формат выходных данных**

В первой строке выведите число $k$. Во второй строке выведите индексы $i_{1}\, i_{2}\, \ldots\, i_k$. В третьей строке выведите индексы $j_{1}\, j_{2}\, \ldots\, j_k$. Если подходящий последовательностей индексов несколько, выведите любые из них.

**Примеры**

стандартный ввод

5
1 2 3 4 5
1 3 2 4 4

стандартный вывод

3
0 1 3
0 2 4

```
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main()
{
	int n;
	cin >> n;
	int* arr1 = new int[n + 1];
	int* arr2 = new int[n + 1];
	int** matr = new int* [n + 1];
	for (int i = 0; i < n + 1; i++)
	{
		matr[i] = new int[n + 1];
	}
	for (int i = 0; i <= n; i++)
	{
		matr[0][i] = 0;
		matr[i][0] = 0;
	}
	for (int i = 1; i <= n; i++)
	{
		cin >> arr1[i];
	}
	for (int i = 1; i <= n; i++)
	{
		cin >> arr2[i];
	}
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= n; j++)
		{
			if (arr1[i] == arr2[j])
			{
				matr[i][j] = matr[i - 1][j - 1] + 1;
			}
			else
			{
				matr[i][j] = max(matr[i - 1][j], matr[i][j - 1]);
			}
		}
	}
	cout << matr[n][n] << endl;
	vector<int> ind1;
	vector<int> ind2;
	int i = n;
	int j = n; 
	int k = matr[n][n];
	while (matr[i][j] != 0)
	{
		if (arr1[i] == arr2[j])
		{
			ind1.push_back(i - 1);
			ind2.push_back(j - 1);
			i--;
			j--;
		}
		else if (matr[i - 1][j] == matr[i][j])
		{
			i--;
		}
		else
		{
			j--;
		}
	}
	reverse(ind1.begin(), ind1.end());
	reverse(ind2.begin(), ind2.end());
	for (int t : ind1)
	{
		cout << t << " ";
	}
	cout << endl;
	for (int t : ind2)
	{
		cout << t << " ";
	}
}
```
