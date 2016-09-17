#####################################
[cf371] B. Filya and Homework
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/714/problem/B>`_
******************************************************

給定一個長度為 N 的序列 A，你可以選擇一個非負整數 x，針對序列的每一項 A[i]
1. 要嘛 A[i] -= x
2. 要嘛 A[i] += x
3. 要嘛 A[i] 不變
最後使得 A 的每一項都相同，請問該 x 存不存在？

************************
Specification
************************

::

    1 <= N <= 10^5
    0 <= A[i] <= 10^9

************************
分析
************************

.. note:: 水題

一直在腦補題目成從一些項減去 x，一些項加上 x，最後每一項相同…
結果花了一個多小時在想為什麼找平均數會 WA…第 10 次 submit 才 AC
果然熬夜比賽頭腦真的不行

---------------

這題看懂題目的話，就簡單了，假設 K 是序列中的相異數的數量。

1. K = 1, x 即為 0
2. K = 2, x 即為兩數之差
3. K = 3, 若最小數 mn 與最大數 mx 的平均是中間那個數 mid 才有解, x = mx - mid
4. K >= 4，明顯無解

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N;
        cin >> N;
        vector<ll> A(N, 0ll);
        for (int i = 0; i < N; i++)
            cin >> A[i];

        sort(A.begin(), A.end());
        int len = unique(A.begin(), A.end()) - A.begin();

        if (len <= 2) {
            cout << "YES" << endl;
            return 0;
        }

        if (len >= 4) {
            cout << "NO" << endl;
            return 0;
        }

        if (A[1] - A[0] == A[2] - A[1]) {
            cout << "YES" << endl;
            return 0;
        }

        cout << "NO" << endl;

        return 0;
    }
