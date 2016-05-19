#####################################
[cf346] C. Tanya and Toys
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/659/problem/C>`_
******************************************************

存在 10^9 種玩具，第 i 種玩具需要 i 元。
Tania 已有 N 種玩具 a[]，現在在預算 M 元的限制下，Tania 該買哪些玩具來最大化她擁的玩具種類。

************************
Specification
************************

::

    1 <= N <= 10^5
    1 <= M <= 10^9
    1 <= a[i] <= 10^9

************************
分析
************************

.. note:: Greedy

在不買到已有種類的情況下，當然是錢越少的種類越先買啦，買到沒錢為止。

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

        int N, M;
        cin >> N >> M;
        vector<int> a(N, 0);
        for (int i = 0; i < N; i++) {
            cin >> a[i];
        }
        sort(a.begin(), a.end());

        ll cost = 0;
        int idx = 0;
        vector<int> ans;
        for (int i = 1; i <= 1000000000; i++) {
            if (i == a[idx]) {
                idx++;
                continue;
            }

            if (cost + i > M) {
                break;
            }

            cost += i;
            ans.push_back(i);
        }

        cout << ans.size() << "\n";
        for (int i : ans)
            cout << i << " ";
        cout << "\n";

        return 0;
    }
