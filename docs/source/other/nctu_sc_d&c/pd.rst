########################################
[cf168] C. k-Multiple Free Set
########################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/275/problem/C>`_
******************************************************

給定 N 個數字 A[i]，與一正整數 K，請從中選出一個最大子集 S，使得 S 中的任兩數 :math:`x, y (x < y), y \ne Kx`。

************************
Specification
************************

::

    1 <= N <= 10^5
    1 <= K <= 10^9
    1 <= A[i] <= 10^9

************************
分析
************************

.. note:: Greedy

:math:`S` 可以這樣建構：
1. 將 :math:`A` 分解成許多成倍增關係的數列 :math:`x, kx, k^2x, \dots` （數列數盡量少）。
2. 對每個數列 :math:`B_i`，可以選一半的數字加進 :math:`S` 中。

例如::

    A = [2, 3, 5, 6, 9, 10], K = 2
    可以分解成
    B1 = [2]
    B2 = [3, 6, 9]
    B3 = [5, 10]
    所以 S 的一個解為 = [2, 3, 9, 5]

實作上，我們可以模擬這個分解過程::

    1. 將 A 由小到大排序。
    2. 針對 A[i]，看可不可以接在 B[j] 後面
        A. 若可以，將 A[i] 加到 B[j] 最後
        B. 若不行，新增一個數列 B = [A[i]]
    3. 加總「每個數列長度的一半」。

這個想法也可以以 Greedy 實作::

    1. 將 A 由小到大排序, S = {}
    2. 針對 A[i]
        a. 如果 A[i] % K != 0，則將 A[i] 加進 S
        b. 如果 A[i] % K == 0，則看 A[i] / K 是否已經在 S 中
            A. 若已在，則 A[i] 不可能在 S 中，處理下一個
            B. 若不在，則將 A[i] 將進 S

---------------

注意，這題 A 並沒有形成偏序集（關係為 :math:`y = kx`），因為不滿足 transitivity。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int N, K;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N >> K;

        vector<int> A(N);
        for (int i = 0; i < N; i++)
            cin >> A[i];

        sort(A.begin(), A.end());

        set<int> ans;
        for (int i : A) {
            if (i % K != 0) ans.insert(i);
            else {
                if (ans.find(i / K) == ans.end())
                    ans.insert(i);
            }
        }

        cout << ans.size() << endl;

        return 0;
    }
