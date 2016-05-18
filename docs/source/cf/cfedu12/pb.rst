###################################################
[cfedu12] B. Shopping
###################################################

.. sidebar:: Tags

    - ``tag_implementation``
    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/665/problem/B>`_
******************************************************

水題就不打了。自己看題目。

************************
Specification
************************


************************
分析
************************

.. note:: 水題

直接模擬。用 deque 很方便

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

        int N, M, K;
        cin >> N >> M >> K;

        deque<int> data;
        for (int i = 0; i < K; i++) {
            int val; cin >> val;
            data.push_back(val);
        }

        ll cnt = 0;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                int val; cin >> val;
                auto it = find(data.begin(), data.end(), val);

                cnt += (it - data.begin() + 1);
                data.erase(it);
                data.push_front(val);
            }
        }

        cout << cnt << "\n";

        return 0;
    }
