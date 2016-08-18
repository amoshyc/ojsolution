###################################################
[cfedu9] F. Magic Matrix
###################################################

.. sidebar:: Tags

    - ``tag_mst``
    - ``tag_dfs``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/632/problem/F>`_
******************************************************

給定 NxN 的矩陣 A，請問 A 是否是 Magic 的，Magic 的定義如下

For all i, j
::

    1. A[i][i] = 0
    2. A[i][j] = A[j][i]
    3. A[i][j] <= max(A[i][k], A[j][k]) for all k

************************
Specification
************************

::

    1 <= N <= 2550
    0 <= A[i][j] <= 10^9

************************
分析
************************

.. note:: minimax distance

A is magic if and only if::

    1. A[i][i] = 0
    2. A[i][j] = A[j][i]
    3. A[i][j] <= max(A[i][k], A[j][k]) for all k

條件三可以改寫，因為條件二::

    A[i][j] <= max(A[i][k], A[k][j]) for all k

再改寫::

    A[i][j] = min(max(A[i][k], A[k][j]) for all k)

這一看，不就是 minimax distance 的三角不等式嗎？
沒聽過？請參 uva 534，那隻青蛙會跳給你看~

所以要判定條件三，只要把 A 視為相鄰矩陣轉成圖（這會是一個完全圖）。
然後找出任兩個節點 i, j 之間的 minimax distance，看該距離是不是等於 A[i][j]

問題變成找出所有節點間的 minimax distance。
全源最短？第一個直覺當然是 Floyd Warshall，很可惜會 TLE。
難道是跑 V 次 Dijkstra？嗯嗯，還是會 TLE。

答案是建 MST。
MST 中任兩點的最長邊即為該兩點的 minimax distance
這是因為 MST 會最小化任兩個節點之間的最長邊。
這可以用反證法證明，你也可以想想 Kruskal 建 MST 的過程。

求出 MST 後，可以透成跑 V 次 dfs 來計算任兩點的 minimax distance。
詳細地說，即以每個節點當起點，跑一次 dfs，計算該點到其它點的最長邊是多少。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    #define st first
    #define nd second
    typedef pair<int, int> pii; // <e.to, w>

    const int MAX_N = 2500;
    int N;
    int A[MAX_N][MAX_N];

    struct Dsu {
        vector<int> par;

        Dsu(){}
        Dsu(int N) {
            par = vector<int>(N, -1);
        }

        int root(int a) {
            if (par[a] < 0) return a;
            return (par[a] = root(par[a]));
        }

        void unite(int a, int b) {
            a = root(a);
            b = root(b);
            if (a == b) return; // already in same set
            if (-par[a] > -par[b]) swap(a, b); // if (size[a] > size[b])
            par[b] += par[a]; // size[b] += size[a]
            par[a] = b;
        }

        bool same(int a, int b) {
            return root(a) == root(b);
        }

        int size(int a) { // correct if a is root
            return -par[a];
        }
    };

    struct Edge {
        int u, v, w;
        bool operator < (const Edge& e) const {
            return w < e.w;
        }
    };

    // kruskal
    vector<Edge> edges;
    vector<pii> mst[MAX_N];

    // minimax distance
    int B[MAX_N][MAX_N];

    void kruskal() {
        for (int u = 0; u < N; u++)
            for (int v = u + 1; v < N; v++)
                edges.push_back((Edge) {u, v, A[u][v]});

        sort(edges.begin(), edges.end());

        Dsu dsu(N);

        for (const Edge& e : edges) {
            if (!dsu.same(e.u, e.v)) {
                dsu.unite(e.u, e.v);

                mst[e.u].push_back(pii(e.v, e.w));
                mst[e.v].push_back(pii(e.u, e.w));
            }
        }
    }

    void dfs(int s, int u, int p, int mx) {
        B[s][u] = mx;

        for (const pii& e : mst[u]) {
            int v = e.st;
            int w = e.nd;

            if (v == p) continue;
            else dfs(s, v, u, max(mx, w));
        }
    }

    bool solve() {
        for (int i = 0; i < N; i++)
            if (A[i][i] != 0)
                return false;

        for (int r = 0; r < N; r++)
            for (int c = 0; c < r; c++)
                if (A[r][c] != A[c][r])
                    return false;

        kruskal();

        for (int u = 0; u < N; u++) {
            dfs(u, u, -1, 0);

            for (int v = 0; v < N; v++) {
                if (B[u][v] != A[u][v]) {
                    return false;
                }
            }
        }

        return true;
    }

    int main() {
        scanf("%d", &N);
        for (int r = 0; r < N; r++)
            for (int c = 0; c < N; c++)
                scanf("%d", &A[r][c]);

        puts(((solve()) ? "MAGIC" : "NOT MAGIC"));

        return 0;
    }
