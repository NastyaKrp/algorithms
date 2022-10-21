# Единицы 

Дано число $N$. Необходимо определить, сколько есть бинарных строк длины $N$, в которых ровно $K$ единиц.

**Формат входных данных**

Первая строка входных данных содержит два целых неотрицательных числа $N$ и $K$ $(0 \le K \le N \le 10^6)$.

**Формат выходных данных**
Выведите одно число - ответ на задачу. Так как ответ может быть очень большим, необходимо его вывести по модулю $10^9 + 7$.

```
#include <iostream>
using namespace std;

long long pow(long long x, long long n)
{
    long long res = 1;
    while (n > 0)
    {
        if (n % 2 == 1)
        {
            res = (res * x) % 1000000007;
        }
        x = (x * x) % 1000000007;
        n = n >> 1;
    }
    return res;
}

int main()
{
    int n, k;
    cin >> n >> k;
    long long modulus = 1000000007;
    long long a = 1, b = 1;


    for (int i = n - k + 1; i <= n; i++)
    {
        a *= i;
        a %= modulus;
    }
    for (int i = 1; i <= k; i++)
    {
        b *= i;
        b %= modulus;
    }
    cout << (a * pow(b, modulus - 2)) % modulus;
}
```
