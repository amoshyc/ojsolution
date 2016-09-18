#####################################
[cf372] D. Complete The Graph
#####################################

.. sidebar:: Tags

    - ``tag_dijkstra``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/716/problem/D>`_
******************************************************

給定一個無向圖 G = (N, M) 與 L, S, T，限制所有邊的權重為正整數。
其中一些邊一開始是沒有權重的。請設定這些邊的權重，
使得節點 S 到節點 T 的最短路徑的長度是 L。
如果無解，請輸出 "NO"；有果有解，請輸出 "YES"，並輸出所有邊。

************************
Specification
************************

::

    1 <= N <= 10^3
    1 <= M <= 10^4
    1 <= L <= 10^9

************************
分析
************************

.. note:: dijkstra

讓我們稱這些可以設定權重的邊為 varE。
根據題意，每一個 varE 權重能設定的範圍為 [1, INF]。
INF 設為一個大於 L 的數即可。

一個重要的觀察是::

    降低一個 varE 的權重，新的最短路徑長必小於等於原先的最短路徑長。

可以利用 greedy 設計出一個算法::

    一開始每個 varE 的權重都是 INF
    一個一個降低 varE 的權重，直到最短路徑長 = L
    （先把前一個 varE 降到最小值 1 後，再調整下一個）
    （最後一個要調整的 varE，權重可能不是降到 1，而且某個數）

其中找最短路徑長的部份，用 dijkstra 跑一下即可。
正確性證明可看官方題解，雖然我覺得還挺直覺的。

---------------------------

無解發生在兩種情況，顯然的：

1.  所有 varE 的權重都是最小值 1，最短路徑長仍 > L。
2.  所有 varE 的權重都是最大值 INF，最短路徑長仍 < L。

varE 再怎麼調整，最短路徑長都不可能 = L。這兩個情況，可以先判掉。

細節直接看程式碼吧，整體時間為 O(M * NlgM)

************************
AC Code
************************

程式碼是改自 huzzah 的 `AC Code <http://codeforces.com/contest/716/submission/20698605>`_

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    #define st first
    #define nd second
    typedef pair<int, int> pii;
    typedef long long ll;
    const ll INF = 0x3f3f3f3f;

    struct Edge {
        int id, to;
    };

    const int MAX_N = 1000;
    const int MAX_M = 10000;
    int N, M, L, S, T;
    ll w[MAX_M];
    vector<pii> E;
    vector<int> varE;
    vector<Edge> g[MAX_N];

    typedef pair<ll, int> P;
    ll dijkstra(int s, int t) {
        vector<ll> d(N, INF);
        priority_queue<P, vector<P>, greater<P>> pq;
        d[s] = 0;
        pq.push(P(0ll, s));
        while (!pq.empty()) {
            P p = pq.top(); pq.pop();
            ll dis = p.st; int v = p.nd;
            if (dis > d[v]) continue;
            for (Edge& e : g[v]) {
                if (d[e.to] > d[v] + w[e.id]) {
                    d[e.to] = d[v] + w[e.id];
                    pq.push(P(d[e.to], e.to));
                }
            }
        }
        return d[t];
    }

    int main() {
        scanf("%d %d %d %d %d", &N, &M, &L, &S, &T);
        for (int i = 0; i < M; i++) {
            int u, v, z; scanf("%d %d %d", &u, &v, &z);
            E.push_back(pii(u, v));
            g[u].push_back((Edge) {i, v});
            g[v].push_back((Edge) {i, u});
            w[i] = z;
            if (z == 0)
                varE.push_back(i);
        }

        for (int id : varE)
            w[id] = 1;
        ll best = dijkstra(S, T);

        for (int id : varE)
            w[id] = INF;
        ll worst = dijkstra(S, T);

        if (best > L || worst < L) {
            puts("NO");
            return 0;
        }

        puts("YES");
        if (worst > L) {
            for (int id : varE)
                w[id] = INF;

            for (int id : varE) {
                w[id] = 1;
                int dis = dijkstra(S, T);
                if (dis <= L) {
                    w[id] += (L - dis);
                    break;
                }
            }
        }

        for (int id = 0; id < M; id++) {
            printf("%d %d %lld\n", E[id].st, E[id].nd, w[id]);
        }

        return 0;
    }
