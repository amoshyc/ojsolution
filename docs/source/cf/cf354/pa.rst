#####################################
[cf354] A. Nicholas and Permutation
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/676/problem/A>`_
******************************************************

給定一序列 a[]，保證是 1 ~ N 的一種排列。
距離定義為兩元素的 index 差。你可以挑任兩個元素交換位置一次，
請問序列中最大元素與最小元素的最遠距離是多少？

************************
Specification
************************

::

    1 <= N <= 100

************************
分析
************************

.. note:: 水題

當然是把最小或最大元素換到頭或尾去啦，直接看 code 吧。

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

        int N; cin >> N;
        vector<int> data(N, 0);
        for (int i = 0; i < N; i++)
            cin >> data[i];

        int MM = max_element(data.begin(), data.end()) - data.begin();
        int mm = min_element(data.begin(), data.end()) - data.begin();

        int res[4];
        res[0] = MM - 0;
        res[1] = N - 1 - MM;
        res[2] = mm - 0;
        res[3] = N - 1 - mm;

        cout << *max_element(res, res + 4) << endl;

        return 0;
    }
