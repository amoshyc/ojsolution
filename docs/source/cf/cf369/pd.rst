#####################################
[cf369] D. Directed Roads
#####################################

.. sidebar:: Tags

    - ``tag_math``
    - ``tag_dfs``
    - ``tag_fast_pow``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/711/problem/D>`_
******************************************************

給定一個節點數為 N 的有向圖。其中每個節點 u，都有一條有向邊 <u, nxt[u]> (nxt[u] ≠ u)。
如果選定一些邊，將這些邊的方向反轉，所形成的新的圖沒有環，則代表這些邊的集合有性質 P
請問給定的圖的所有邊的子集（共 2^N 的）中，有多少集合也擁有性質 P？

************************
Specification
************************

::

    2 <= N <= 2 * 10^5
    1 <= nxt[u] <= N

************************
分析
************************

.. note:: 環

首先，仔細想想這個圖，可以發現這個圖可以分解成一個或多個「單向連通單元」。

考慮那些含有有向環的單元 :math:`G' = (V, E)`，因為每個節點的 out degree 都是 1，
所以這個單元不會出現二個以上的有向環。假設此單元中的那個環是邊集合 :math:`C`，
那這個有向環可以產生出 :math:`2^{|C|} - 2` 個子集符合題目要求。
其中的 :math:`-2` 來自於空集合與 :math:`C` 本身，這兩個情況反轉邊後，這個單元中仍有有向環存在。

而 :math:`G'` 中不在環上的邊集合 :math:`V - C` 可以產生 :math:`2^{|V| - |C|}` 個子集（即所有的字集），
都符合題目要求，同樣因為每個節點 out degree 都是 1，這些邊轉或不轉都不會產生或消除環。
所以總得來說，:math:`G'` 共有 :math:`(2^{|C|} - 2) * 2^{|V| - |C|}` 個邊的集合，反轉後的 :math:`G'` 是沒有環的。

根據乘法原理，各「單向連通單元」的值乘起來就是答案。

------------------

實作上，就是簡單的不斷找環，先做 :math:`2^{|C|} - 2` 的部份，
最後再一次計算所有不在環上的邊:

.. math::
    2^{N - R} \text{ where R is the total number of edges not in directed cycles}

而因為每個節點的 out degree 都是 1，所以不會出現大環中有小環，兩環連通等情況。
我們只需簡單的 dfs，利用深度，即可計算出每個環是多大。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const ll mod = 1e9 + 7;
    const int MAX_N = 200000;

    int N;
    int nxt[MAX_N];
    int root[MAX_N];
    int depth[MAX_N];
    ll ans = 1;
    int rest_cnt = 0;

    ll fast_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a;
        while (b) {
            if (b & 1)
                ans = ans * base % mod;
            base = base * base % mod;
            b >>= 1;
        }
        return ans;
    }

    void dfs(int u, int dep, int rt) {
        root[u] = rt;
        depth[u] = dep;

        int v = nxt[u];
        if (root[v] == -1) dfs(v, dep + 1, rt);
        else if (root[v] == root[u]) { // in same tree -> cycle
            int cycle_size = depth[u] - depth[v] + 1;
            ans = ans * ((fast_pow(2, cycle_size) - 2) + mod) % mod;
            rest_cnt -= cycle_size;
        }
    }

    int main() {
        memset(root, -1, sizeof(root));
        memset(depth, 0, sizeof(depth));

        scanf("%d", &N);
        for (int i = 0; i < N; i++) {
            scanf("%d", &nxt[i]);
            nxt[i]--;
        }

        rest_cnt = N; // cnt of edges not in cycle
        for (int v = 0; v < N; v++) {
            if (root[v] == -1) {
                dfs(v, 0, v);
            }
        }

        ans = ans * fast_pow(2, rest_cnt) % mod;
        printf("%lld\n", ans);

        return 0;
    }
