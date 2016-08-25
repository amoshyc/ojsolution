###################################################
[cfedu16] B. Optimal Point on a Line
###################################################

.. sidebar:: Tags

    - ``tag_math``
    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/710/problem/B>`_
******************************************************


************************
Specification
************************


************************
分析
************************

.. note:: 水題

就中位數，大家高中應該都學過。

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

        vector<int> A(N, 0);
        for (int i = 0; i < N; i++)
            cin >> A[i];

        sort(A.begin(), A.end());

        if (A.size() & 1)
            cout << A[A.size() / 2] << endl;
        else
            cout << A[int(A.size()) / 2 - 1] << endl;

        return 0;
    }
