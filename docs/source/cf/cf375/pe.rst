#####################################
[cf375] E. One-Way Reform
#####################################

.. sidebar:: Tags

    - ``tag_euler_circle``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/723/problem/E>`_
******************************************************

給定一個無向圖，請將每個邊加上方向，使得轉換後的有向圖最大化「indegree = outdegree」的節點數量。

************************
Specification
************************

::

    1 <= N <= 200
    1 <= M <= N * (N + 1) / 2

************************
分析
************************

.. note:: 歐拉環

一個重要觀察::

    無向圖中，degree 為奇數的節點，轉成有向圖後不可能 indegree = outdegree

所以能滿足 indegree = outdegree 的節點只有在無向圖中那些 degree 為偶數的節點。

另一個觀察::

    無向圖中，degree 為奇數的節點一定有偶數個

因為整個圖總 degree 數是偶數，degree 為偶數的節點貢獻的 degree 數也是偶數，所以 degree 為奇數的點必有偶數個。

哪時能最大化 indegree = outdegree 節點的數量呢？當然是 **每個 degree 為偶數的點轉換後都滿足 indegree = outdegree** 。這讓人想到歐拉環。透過歐拉環，一個簡單的（好吧，不是那麼簡單，那麼直覺）的策略：

如果給定的圖本身就是一個歐拉環，那我們只需把歐拉環找出來即可。
如果不是：

1. 如果給定的圖有 degree 為奇數的點，那們在這些點中加進一些邊，使得整個圖滿足歐拉環的存在性
2. 找出歐拉環，再從歐拉環中刪去我們加入的邊，剩下的邊即為答案

歐拉環本身最大化了轉成有向圖後 indegree = outdegree 節點的數量。而我們加入的邊只連接 degree 為奇數的節點，並不影響答案。``(1)`` 的實際做法為：

如果有 :math:`2k` 的 degree 為奇數的點 :math:`v_1, v_2, \dots, v_{2k}`，我們只需在 :math:`v_i, v_{i+1}` 之間建邊，即可讓所有點的 degree 都是偶數。

---------------------------

找出歐拉環的細節請自行參閱模板。我覺這題出得很好，記得考慮給定的圖可能是多個 disjoint 的單元。比賽時只想到第一個觀察與歐拉環，可惜~

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int MAX_N = 200;
    const int MAX_M = MAX_N * (MAX_N + 1) / 2;

    // list
    struct Node {
        int val;
        Node *nxt;
    };
    int NN = 0;
    Node pool[MAX_M + 1]; // 給定的圖 E 條有向邊，環最大有 E + 1 個節點
    Node* root = NULL;

    Node* new_node(int v) {
        Node* it = &pool[NN++];
        it->val = v;
        it->nxt = NULL;
        return it;
    }

    // graph
    int N, M; // V, E
    vector<int> cycle; // 存找到的（小）歐拉環
    multiset<int> g[MAX_N]; // 用 multiset 存

    void dfs(int u) {
        cycle.push_back(u);
        if (g[u].size() > 0) {
            int v = *g[u].begin();
            g[u].erase(g[u].begin()); // remove 1 edge
            g[v].erase(g[v].find(u)); // remove 1 edge
            dfs(v);
        }
    }

    void find_euler_cycle(int s) {
        root = new_node(s);
        for (Node* it = root; it != NULL; it = it->nxt) {
            if (g[it->val].size() > 0) {
                cycle.clear();
                dfs(it->val);

                Node* nxt = it->nxt;
                Node* pre = it;
                for (int i = 1; i < int(cycle.size()); i++) {
                    Node* temp = new_node(cycle[i]);
                    pre->nxt = temp;
                    pre = temp;
                }
                pre->nxt = nxt;
            }
        }
    }

    typedef pair<int, int> pii;
    int deg[MAX_N];

    int main() {
        int TC;
        scanf("%d", &TC);
        while (TC--) {
            scanf("%d %d", &N, &M);

            NN = 0;
            fill(deg, deg + N, 0);
            for (int u = 0; u < N; u++)
                g[u].clear();

            for (int i = 0; i < M; i++) {
                int u, v; scanf("%d %d", &u, &v); u--; v--;
                g[u].insert(v);
                g[v].insert(u);
                deg[u]++;
                deg[v]++;
            }

            vector<int> odds;
            for (int u = 0; u < N; u++) {
                if (deg[u] % 2 == 1) {
                    odds.push_back(u);
                }
            }

            set<pii> added; // added edges
            for (int i = 0; i < int(odds.size()); i += 2) {
                int u = odds[i], v = odds[i + 1];
                g[u].insert(v);
                g[v].insert(u);
                added.insert(minmax(u, v));
            }

            printf("%d\n", N - int(odds.size()));

            for (int u = 0; u < N; u++) { // 如果圖是多個 disjoint 單元
                if (g[u].size() > 0) {
                    find_euler_cycle(u);

                    for (Node* it = root; it->nxt != NULL; it = it->nxt) {
                        int u = it->val, v = it->nxt->val;

                        auto pos = added.find(minmax(u, v));
                        if (pos == added.end()) {
                            printf("%d %d\n", u + 1, v + 1);
                        }
                        else {
                            added.erase(pos);
                        }
                    }
                }
            }
        }

        return 0;
    }
