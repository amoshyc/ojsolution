########################################
[cfedu7] E. Ants in Leaves
########################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_greedy``
    - ``tag_sorting``
    - ``tag_dfs``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/622/problem/E>`_
******************************************************

給定一個大小為 n 的 rooted tree，tree 的 leaves 上各有一隻螞蟻。
每一單位時間，任意數量的螞蟻可以同時往上爬，
但在同一時間，任何節點（除 root 外）只能有一隻螞蟻。
求使所有螞蟻都爬到 root，最小要花多少時間？

************************
Specification
************************

::

    1 <= n <= 500000


************************
分析
************************

.. note:: 先找出遞迴式，並用 dfs + sorting 找出 dp 的順序

root 的性質與其它節點不同，嘗試先解決掉它。

我們可以發現題目所要求的答案，就是各隻螞蟻到達 level 1 [#f1]_ 的時間的最大值再加一。

接下來，針到 level 1 的每一點 k，令::

    dp[u] = 螞蟻 u 爬到點 k 所需的時間
    (u 在 k 的子樹中)

可以找到狀態轉移方程式::

    dp[u] = max(dp[v] + 1, depth[u])
    v 是 u 前一隻到達 k 的螞蟻
    depth[u] 是 u 的深度

接下來問題就簡單啦，只是我們找到子樹中所有螞蟻到達 k 的順序，
就可以在 O(n) 的時間建出 dp，dp 表格中的最大值即為子樹 k 的答案。

而順序是什麼呢？答案來自於 Greedy，深度越深的螞蟻越晚到。
仔細一下，也應該如此，因為如果較深的螞蟻較早到，他在路上一定會卡到其它螞蟻的。

所以題目的答案即為 ::

    max([子樹 x 的答案 + 1 | x in level 1])

.. [#f1] root 是 level 0，其 children 是 level 1，以此類推…

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_n = 500000;

    int n;
    int t[max_n];
    int d[max_n];
    vector<int> leaf; // children
    vector<int> g[max_n];

    void dfs(int u, int p, int dep) {
        d[u] = dep;

        if (g[u].size() == 1) {
            leaf.push_back(u);
            return;
        }

        for (int v : g[u]) {
            if (v != p) {
                dfs(v, u, dep + 1);
            }
        }
    }

    int main() {
        scanf("%d", &n);
        for (int i = 0; i < n - 1; i++) {
            int u, v; scanf("%d %d", &u, &v);
            u--; v--;
            g[u].push_back(v);
            g[v].push_back(u);
        }

        int ans = -1;
        for (int u : g[0]) {
            leaf.clear();
            dfs(u, 0, 0);
            sort(leaf.begin(), leaf.end(), [&](int i, int j) {
                return d[i] < d[j];
            }); // 越淺的越早到

            t[0] = d[leaf[0]];
            ans = max(ans, t[0] + 1);
            for (int i = 1; i < leaf.size(); i++) {
                t[i] = max(d[leaf[i]], t[i - 1] + 1);
                ans = max(ans, t[i] + 1);
            }
        }

        printf("%d\n", ans);

        return 0;
    }
