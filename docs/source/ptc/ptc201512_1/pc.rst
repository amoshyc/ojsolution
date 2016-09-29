###################################################
C. Warehouse
###################################################

.. sidebar:: Tags

    - ``tag_doubling``

.. contents:: TOC
    :depth: 2


****************************************************************************
`題目 <http://140.116.249.152/e-Tutor/mod/programming/view.php?id=26867>`_
****************************************************************************

給定 NxN 的格子，每個格子上要嘛是障礙物，要嘛是一個箭頭指向八個方向之一，
代表下一步是往哪裡走。之後有 Q 筆詢問，
每筆詢問 (x, y, k) 代表詢問從 (x, y) 出發走 K 步後在哪裡，
如果過程中遇到障礙物或超出邊界則停在前一格上面。

※ 每筆詢問保證一開始的 (x, y) 是可走的。

************************
Specification
************************

::

    1 <= N <= 1000
    Q <= 50
    0 <= K <= 10^8


************************
分析
************************

.. note:: 倍增法

使用類似倍增法建出下列陣列，再搭配快速冪::

    int next[i][x, y] = 從 x, y 出發，走 2^i 步後到哪裡。

利用::

    next[i][x, y] = next[i-1][ next[i-1][x, y] ]

可在 O(MAX_LOG_K * N * N) 建出數列。

之後就是用快速冪，即利用::

    1 步後 (next[0][x, y])
    2 步後 (next[1][x, y])
    4 步後 (next[2][x, y])
    8 步後 (next[3][x, y])
    16 步後 (next[4][x, y])
    ...

組出 K 步後在哪裡，每次詢問 O(lg(k))。

---------------------------------------------

比賽時，記憶體並沒有限制，題目放上 etutor 後，記憶體有 64MB 的限制，
造成 next 陣列開不出來（etutor 竟然把 MLE 回報成 TLE…）。
現在有新增第二筆測資，N <= 500，這樣 next 陣列的大小就沒超出限制，
所以下面的程式只能解出第二筆測資。


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <cstdio>
    #include <cstring>
    using namespace std;

    const int MAX_N = 500;
    const int MAX_LOG_K = 27; // ceil(lg(10^8))
    const int dr[8] = {0, -1, -1, -1, 0, +1, +1, +1};
    const int dc[8] = {+1, +1, 0, -1, -1, -1, 0, +1};

    int N, Q;
    int G[MAX_N * MAX_N];
    int next[MAX_LOG_K][MAX_N * MAX_N];

    inline int e(int r, int c) { // encode
        return r * N + c;
    }

    void init() {
        for (int r = 0; r < N; r++) {
            for (int c = 0; c < N; c++) {
                int idx = e(r, c);
                if (G[idx] == -1) next[0][idx] = -1;
                else {
                    int nr = r + dr[G[idx]];
                    int nc = c + dc[G[idx]];
                    if (nr < 0 || nr >= N || nc < 0 || nc >= N || G[e(nr, nc)] == -1)
                        next[0][idx] = idx;
                    else
                        next[0][idx] = e(nr, nc);
                }
            }
        }

        for (int i = 1; i < MAX_LOG_K; i++) {
            for (int r = 0; r < N; r++) {
                for (int c = 0; c < N; c++) {
                    int idx = e(r, c);
                    if (next[i-1][idx] == -1) next[i][idx] = -1;
                    else next[i][idx] = next[i-1][next[i-1][idx]];
                }
            }
        }
    }

    int query(int r, int c, int k) {
        int ans = e(r, c);
        int base = 0;

        while (k != 0) {
            if (k & 1)
                ans = next[base][ans];
            base++;
            k >>= 1;
        }

        return ans;
    }

    int main() {
        while (scanf("%d %d", &N, &Q) != EOF) {
            memset(G, -1, sizeof(G));
            memset(next, -1, sizeof(next));

            for (int r = 0; r < N; r++) {
                char input[N + 1];
                scanf("%s", input);
                for (int c = 0; c < N; c++) {
                    if (input[c] == 'x') G[e(r, c)] = -1;
                    else G[e(r, c)] = input[c] - '1' + 0;
                }
            }

            init();

            while (Q--) {
                int r, c, k;
                scanf("%d %d %d", &c, &r, &k);
                int ans = query(r, c, k);
                printf("%d %d\n", ans % N, ans / N);
            }
        }

        return 0;
    }
