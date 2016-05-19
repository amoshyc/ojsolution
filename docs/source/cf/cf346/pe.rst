#####################################
[cf346] E. New Reform
#####################################

.. sidebar:: Tags

    - ``tag_dfs``
    - ``tag_dsu``
    - ``tag_connected_component``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/659/problem/E>`_
******************************************************

給定一個無向圖 G = (V, E)，將所有的邊轉成有向邊後，請問最少會有幾個 indegree = 0 的點。

************************
Specification
************************

::

    1 <= |V| <= 10^5
    1 <= |E| <= 10^5

************************
分析
************************

.. note:: 連通單元、環

從範測可以發現::

    1. 存在環的連通單元，轉換過後不會產生任何 indegree = 0 的點。
    2. 其它每個連通單元，都會產生一個。
       因為從該連通單元中隨便一個點做 dfs，只有 root 的 indegree = 0。

現在問題就 reduce 到圖中有幾個環呢？環以外有多少個連通單元呢？

一個簡單的做法就是從所有尚未到過的點 dfs，看能不能夠成環。若可以，cnt += 0；若不行，cnt++;

另一個方法則可以利用 disjoint set 來解，因為這個性質::

    構成環的連通單元的邊的數量 ecnt 與點的數量 vcnt，必有以下性質：
    ecnt = vcnt - 1

我們可以把所有邊 e = (u, v) 的兩端點 unite(u, v) 起來。
同時統計每個集合（即連通單元）有幾個節點，幾個邊。

最後再來檢查有幾個連通單元有環，幾個沒有。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_n = 100000;
    // const int max_m = 100000;
    int n, m;
    vector<int> g[max_n];
    bool vis[max_n];

    bool dfs(int u, int p) {
        // cout << u << ", " << p << endl;
        vis[u] = true;

        for (int v : g[u]) {
            if (v == p) continue;

            if (vis[v]) return true;
            if (dfs(v, u)) return true;
        }

        return false;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> n >> m;
        for (int i = 0; i < m; i++) {
            int u, v; cin >> u >> v;
            u--; v--;
            g[u].push_back(v);
            g[v].push_back(u);
        }

        int cnt = 0;
        for (int u = 0; u < n; u++) {
            if (!vis[u]) {
                bool has_cycle = dfs(u, -1);
                if (!has_cycle)
                    cnt++;
            }
        }

        cout << cnt << "\n";

        return 0;
    }

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    #define st first
    #define nd second
    #define pb push_back

    typedef pair<int, int> pii;

    const int MAX_V = 100000;
    const int MAX_E = 100000;
    int V, E;
    pii edge[MAX_E];

    int par[MAX_V];
    int vcnt[MAX_V];
    int ecnt[MAX_V];

    void init() {
        memset(par, -1, sizeof(par));
    }

    int root(int u) {
        if (par[u] < 0) return u;
        return (par[u] = root(par[u]));
    }

    void merge(int u, int v) {
        u = root(u);
        v = root(v);
        if (u == v) return;
        if (-par[u] > -par[v]) swap(u, v);
        par[v] += par[u];
        par[u] = v;
    }

    bool same(int u, int v) {
        return root(u) == root(v);
    }

    int main() {
        init();

        scanf("%d %d", &V, &E);
        for (int i = 0; i < E; i++) {
            int a, b; scanf("%d %d", &a, &b); a--; b--;
            edge[i] = pii(a, b);
            merge(a, b);
        }

        for (int i = 0; i < V; i++) {
            vcnt[root(i)]++;
        }
        for (int i = 0; i < E; i++) {
            ecnt[root(edge[i].st)]++;
        }

        int ans = 0;
        for (int i = 0; i < V; i++) {
            if (par[i] >= 0) continue;

            if (ecnt[i] == vcnt[i] - 1)
                ans++;
        }

        printf("%d\n", ans);

        return 0;
    }
