#####################################
[cf342] C. K-special Tables
#####################################

.. sidebar:: Tags

    - ``tag_constructive_algorithm``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/625/problem/C>`_
******************************************************

給定 n, k，請輸出 nxn 的矩陣 a[][]，並滿足下列條件：

1. 1 ~ n^2 在矩陣中各出現一次
2. 矩陣中每一個 row 都是遞增的
3. 最大化 k-th column 的和

************************
Specification
************************

::

    1 <= k < n <= 500


************************
分析
************************

.. note:: 水題

要最大化 k-th column 的和，所以 k-th column 中的每個元素越大越好，
但根據條件 2，每一個 row r，在 k-th column 右邊的所有元素 a[r][x]，
恆有 a[r][x] > a[r][k]，所以我們得先滿足他們。所以我們得到一個策略：

::

    數字由大到小，一一反向填入 a[r][k, n)
    填完後，剩下的數，在滿足條件 2 的情況下，一樣反向填入 k-th column 左邊的位置。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_n = 500;

    int n, k;
    int a[max_n][max_n];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> n >> k; k--;
        int idx = n * n;

        for (int r = n - 1; r >= 0; r--) {
            for (int c = n - 1; c >= k; c--) {
                a[r][c] = idx--;
            }
        }
        for (int r = n - 1; r >= 0; r--) {
            for (int c = k - 1; c >= 0; c--) {
                a[r][c] = idx--;
            }
        }

        int s = 0;
        for (int r = 0; r < n; r++)
            s += a[r][k];

        cout << s << "\n";
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < n; c++)
                cout << a[r][c] << " ";
            cout << "\n";
        }

        return 0;
    }
