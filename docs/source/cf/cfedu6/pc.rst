#####################################
[cfedu6] C. Pearls in a Row
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/620/problem/C>`_
******************************************************

給定長度為 N 的序列 A，定義一個區間是好的若且唯若該區間包含至少兩個相同的數。
請問序列 A 最多可以分割成幾個好區間？並且請輸出這些區間。無解時輸出 -1

************************
Specification
************************

::

    1 <= N <= 3 * 10^5

************************
分析
************************

.. note:: 使用 Set

Greedy 可解，用 set 實作很方便。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef pair<int, int> pii;
    const int max_n = 300000;
    int N;
    int type[max_n];
    vector<pii> ans;

    bool solve() {
        int start = 0;
        set<int> exist;

        for (int i = 0; i < N; i++) {
            if (exist.count(type[i]) == 0) {
                exist.insert(type[i]);
            }
            else {
                ans.push_back(pii(start, i));
                start = i + 1;
                exist.clear();
            }
        }

        if (exist.size() != 0) {
            if (ans.size() == 0) return false;

            pii back = ans.back(); ans.pop_back();
            ans.push_back(pii(back.first, N-1));
        }

        return true;
    }

    int main() {
        scanf("%d", &N);
        for (int i = 0; i < N; i++)
            scanf("%d", &type[i]);

        if (solve() == false)
            puts("-1");
        else {
            printf("%d\n", int(ans.size()));
            for (auto p : ans) {
                printf("%d %d\n", p.first + 1, p.second + 1);
            }
        }

        return 0;
    }
