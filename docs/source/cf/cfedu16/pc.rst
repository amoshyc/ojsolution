###################################################
[cfedu16] C. Magic Odd Square
###################################################

.. sidebar:: Tags

    - ``tag_constructive_algorithm``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/710/problem/C>`_
******************************************************

給定正整數 N，請輸出一個 NxN 的矩陣，其中，1 ~ N^2 各出現一次。
且矩陣每一行、每一列、兩個對角線的總和都是奇數。

************************
Specification
************************

::

    1 <= N <= 49

************************
分析
************************

.. note:: 建構式演算法

我想到的解法是，在第 N / 2 行，與第 N / 2 列都填入奇數。
這會把矩陣切成四塊::

    A1 o A2
    o  o  o
    A3 o A4

可以證，剩下的奇數的個數，與偶數的個數都一定是 4 的倍數。
選 4 個奇數，對稱地填入 A1, A2, A3, A4。
A1, A2 與 A3, A4 以 N/2-th col 對稱
A1, A3 與 A2, A4 以 N/2-th row 對稱
重覆此過程，直到奇數用完為止。對偶數也這樣做。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int A[49][49];

    int main() {
        int N;
        scanf("%d", &N);

        vector<int> odd;
        vector<int> even;
        for (int i = 1; i <= N * N; i++) {
            if (i & 1)
                odd.push_back(i);
            else
                even.push_back(i);
        }

        for (int r = 0; r < N; r++) {
            A[r][N / 2] = odd.back();
            odd.pop_back();
        }
        for (int c = 0; c < N; c++) {
            if (c == N / 2) continue;
            A[N / 2][c] = odd.back();
            odd.pop_back();
        }

        // for (int r = 0; r < N; r++) {
        //     for (int c = 0; c < N; c++) {
        //         printf("%2d ", A[r][c]);
        //     }
        //     puts("");
        // }
        //
        // puts("----------------");

        bool empty = odd.empty();
        for (int r = 0; r < N / 2 && !empty; r++) {
            for (int c = 0; c < N / 2 && !empty; c++) {
                A[r][c] = odd.back(); odd.pop_back();
                A[r][N - c - 1] = odd.back(); odd.pop_back();
                A[N - r - 1][c] = odd.back(); odd.pop_back();
                A[N - r - 1][N - c - 1] = odd.back(); odd.pop_back();

                empty = odd.empty();
            }
        }

        // for (int r = 0; r < N; r++) {
        //     for (int c = 0; c < N; c++) {
        //         printf("%2d ", A[r][c] % 2);
        //     }
        //     puts("");
        // }
        //
        // puts("-----------------");

        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                if (A[r][c] == 0) {
                    A[r][c] = even.back();
                    even.pop_back();
                }
            }
        }

        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                printf("%d ", A[r][c]);
            }
            puts("");
        }

        return 0;
    }
