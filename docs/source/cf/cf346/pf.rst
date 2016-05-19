#####################################
[cf346] F. Polycarp and Hay
#####################################

.. sidebar:: Tags

    - ``tag_dfs``
    - ``tag_dsu``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/659/problem/F>`_
******************************************************

給定 NxM 的格子，每個格子上有數值 a[i][j]，可以將任何格子減去任意數字，直到變為零。
在進行一些上述操作後，Polycarp 希望滿足以下性質::

    1. 所有格子的數值總和等於 K
    2. 所有格子的數值是一樣的（除了那些變為零的格子）
    3. 至少有一個格子的數值是一開始的數值
    4. 除了變為零的格子，其餘所有格子是連通的。

其中連通定義為任相異格子可以只走相鄰的非零格子到達彼此。
請問給定的格子可能達成 Polycarp 所希望的嗎？若可能，請輸出操作後的格子。

************************
Specification
************************

::

    1 <= N <= 1000
    1 <= M <= 1000
    1 <= K <= 10^18
    1 <= a[i][j] <= 10^9

************************
分析
************************

.. note:: 從性質 3 著手

從性質 3 出發，我們可以想到一個解法::

    枚舉每個格子 a[i][j]，看 a[i][j] 連通的格子中，
    是否有 K / a[i][j] 個格子，格子的數值 >= a[i][j]

因為那些滿足上述條件的格子我們都將他們的數值減為 a[i][j]。
所以我們只需要看是否有 K / a[i][j] 個這樣的格子。

於是題目就變成如何計算有多少滿足條件的格子。用 dfs 是可行的。
但我們不能每個格子都跑一次 dfs，這樣太花時間了。

引進 dsu 可以解決這個問題::

    枚舉格子的順序為由數值大的格子枚舉到數值小的。
    每次枚舉時都將該格子與其相鄰格 unite，同時也維護每個集合的大小。

這樣就會有引下性質::

    當枚舉到 a[i][j]，它所屬的那個集合中的那些格子的數值都 >= a[i][j]。
    所以我可以在 O(1) 的時間判斷 a[i][j] 有沒有足夠 >= a[i][j] 的格子。
    若足夠，用 dfs 找出所有格子。

另外，如果 a[i][j] 不能整除 K，
這個格子也不可能成為滿足性質 3 的那個格子，可以不用判斷，只需 unite。

-------------------

這題的測資也太多了吧… 107 筆。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef tuple<int, int, int> tiii;
    typedef long long ll;

    const int dr[4] = {-1, 0, +1, 0};
    const int dc[4] = {0, +1, 0, -1};

    const int MAX_N = 1000;
    const int MAX_M = 1000;
    int N, M; ll K;
    ll w[MAX_N][MAX_M];
    vector<tiii> vs;

    ll res[MAX_N][MAX_M];
    ll cnt = 0, need, min_;

    inline int id(int r, int c) {
        return r * M + c;
    }

    int par[1000 * 1000];
    void init() {
        memset(par, -1, sizeof(par)); // par[i] = -rank[i] if i is root
    }

    int root(int a) {
        if (par[a] < 0) return a;
        return (par[a] = root(par[a]));
    }

    void unite(int a, int b) {
        a = root(a);
        b = root(b);
        if (a == b) return; // already in same set
        if (-par[a] > -par[b]) swap(a, b); // if (rank[a] > rank[b])
        par[b] += par[a]; // height[b] += height[a]
        par[a] = b;
    }

    bool same(int a, int b) {
        return root(a) == root(b);
    }

    void dfs(int r, int c) {
        if (cnt == need) return;

        cnt++;
        res[r][c] = min_;

        for (int i = 0; i < 4; i++) {
            int nr = r + dr[i];
            int nc = c + dc[i];
            if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;
            if (res[nr][nc] != 0) continue;
            if (w[nr][nc] < min_) continue;

            dfs(nr, nc);
        }
    }

    bool solve() {
        sort(vs.begin(), vs.end(), greater<tiii>());

        init();
        for (auto v : vs) {
            int val = get<0>(v);
            int r = get<1>(v);
            int c = get<2>(v);
            for (int i = 0; i < 4; i++) {
                int nr = r + dr[i];
                int nc = c + dc[i];
                if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;
                if (w[nr][nc] < w[r][c]) continue;
                if (!same(id(r, c), id(nr, nc)))
                    unite(id(r, c), id(nr, nc));
            }

            int size = -par[root(id(r, c))];

            // cout << r << ", " << c << ": " << size << endl;

            if (K % val == 0) {
                need = K / val;
                if (need <= size) {
                    min_ = val;
                    cnt = 0;
                    dfs(r, c);
                    return true;
                }
            }
        }

        return false;
    }

    int main() {
        scanf("%d %d %lld", &N, &M, &K);

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                scanf("%lld", &w[i][j]);
                vs.push_back(tiii(w[i][j], i, j));
            }
        }

        if (solve()) {
            puts("YES");
            for (int r = 0; r < N; r++) {
                for (int c = 0; c < M; c++) {
                    printf("%lld ", res[r][c]);
                }
                puts("");
            }
        }
        else {
            puts("NO");
        }


        return 0;
    }
