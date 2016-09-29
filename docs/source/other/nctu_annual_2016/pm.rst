#####################################
[cfother] M. Kaprekar’s constant
#####################################

.. sidebar:: Tags

    - ``tag_doubling``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <https://bitbucket.org/mzshieh/nctu-annual-2016>`_
**********************************************************

定義對於 5-digit 正整數 n 的一個操作：
1. 這 5 個 digit 重組所能組出的最大數為 x
2. 這 5 個 digit 重組所能組出的最大數為 y
3. n = x - y

給定一個 5-digit 正整數 n（可能有前綴零），請問 999999 次操作後，n 為多少

************************
Specification
************************

************************
分析
************************

.. note:: 倍增法 or 找循環

某些數會形成循環，例如 {53955, 59994}。你可以找出這些循環並利用之，但實作偏難。
賽後突然發現，這根本倍增法的裸題！code 超短超快…

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 100000;
    int nxt[20][MAX_N];

    int transform(int val) {
        int digits[5];
        for (int i = 0; i < 5; i++) {
            digits[i] = val % 10;
            val /= 10;
        }

        sort(digits, digits + 5);

        int x = 0, y = 0;
        for (int i = 0; i < 5; i++) {
            x = x * 10 + digits[5 - i - 1];
            y = y * 10 + digits[i];
        }

        return x - y;
    }

    void init() {
        for (int i = 0; i < MAX_N; i++)
            nxt[0][i] = transform(i);

        for (int i = 0; i < 19; i++) {
            for (int j = 0; j < MAX_N; j++) {
                nxt[i + 1][j] = nxt[i][nxt[i][j]];
            }
        }
    }

    int solve(int val) {
        int k = 999999;
        for (int i = 19; i >= 0; i--) {
            if (k & (1 << i)) {
                val = nxt[i][val];
            }
        }
        return val;
    }

    int main() {
        init();

        int TC;
        scanf("%d", &TC);
        while (TC--) {
            int val;
            scanf("%d", &val);
            printf("%05d\n", solve(val));
        }

        return 0;
    }
