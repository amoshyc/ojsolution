#####################################
[cf367] B. Interesting drink
#####################################

.. sidebar:: Tags

    - ``tag_binary_search``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/706/problem/B>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 經典題

計算序列中小於某個數 x 的有幾個，二分搜的經典題。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N;
        cin >> N;

        vector<int> A(N, 0);
        for (int i = 0; i < N; i++)
            cin >> A[i];

        sort(A.begin(), A.end());

        int Q;
        cin >> Q;
        for (int i = 0; i < Q; i++) {
            int m; cin >> m;
            cout << int(upper_bound(A.begin(), A.end(), m) - A.begin()) << "\n";
        }

        return 0;
    }
