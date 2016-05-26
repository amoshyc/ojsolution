#####################################
[cf354] D. Theseus and labyrinth
#####################################

.. sidebar:: Tags

    - ``tag_bfs``
    - ``tag_bitmask``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/676/problem/D>`_
******************************************************

NxM 的迷宮，每個格子有自己的牆壁，牆壁上可能有門。
在每個單位時間，你有兩種選擇::

    1. 順時針旋轉每個格子 90 度
    2. 通過門到達相鄰格

其中，相鄰格相鄰的那兩道牆壁上都要有門才能通過。

現給定每格的四道牆壁上是否有門，與起點 sr, sc 終點 tr, tc，
請問從起點到終點最少要花多少單位的時間？

牆壁狀態為::

    + : this block has 4 doors (one door to each neighbouring block);
    - : this block has 2 doors — to the left and to the right neighbours;
    | : this block has 2 doors — to the top and to the bottom neighbours;
    ^ : this block has 1 door — to the top neighbour;
    > : this block has 1 door — to the right neighbour;
    < : this block has 1 door — to the left neighbour;
    v : this block has 1 door — to the bottom neighbour;
    L : this block has 3 doors — to all neighbours except left one;
    R : this block has 3 doors — to all neighbours except right one;
    U : this block has 3 doors — to all neighbours except top one;
    D : this block has 3 doors — to all neighbours except bottom one;
    * : this block is a wall and has no doors.

************************
Specification
************************

::

    1 <= N, M <= 1000

************************
分析
************************

.. note:: bfs

如果不能旋轉，則這題顯而易見的是跑個 bfs。

現在稍做修改即可::

    bfs 中 queue 存四個狀態 <r, c, rot, level>
    代表現在在 r, c 這個格子，整個迷宮已旋轉了 rot 次，到現在這個格子，花了 level 時間。

從某一格出發，每個單位時間，可以選擇::

    1. 停在原地，迷宮每格都順時針轉 90 度。
    2. 走到相鄰格，可以走得過去的話

    前者即為在 queue 中加入 <r, c, rot + 1, level + 1>
    後者則在 queue 中加入 <nr, nc, rot, level + 1>，
    是否走的過去則可以用 bitmask 來判斷

從上述算法可以發現，每個格子最多只會有四個狀態（轉了 0 次，1 次，2 次，3 次），
所以 bfs 的 vis 陣列即為::

    bool vis[N][M][4]

-----------------------------------

實作上，可以使用 bitmask 的技巧, 上下左右，分別有沒有門，可以編碼為::

    const int T = 0b1000;
    const int D = 0b0100;
    const int L = 0b0010;
    const int R = 0b0001;

一次旋轉即為::

    inline int rotate(int val) {
        int res = 0;
        if (val & T) res |= R;
        if (val & R) res |= D;
        if (val & D) res |= L;
        if (val & L) res |= T;
        return res;
    }

判斷是否連通也即為簡單，例如 (r, c) 這格，與 (r + 1, c) 這格是連通的，
若且唯若 (r, c) 下面的牆有門，且，(r + 1, c) 上面的牆有門。

細節請看程式碼~

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef tuple<int, int, int, int> t4i; // <r, c, rot, level>
    const int dr[4] = {-1, +1, 0, 0};
    const int dc[4] = {0, 0, -1, +1};

    const int T = 0b1000;
    const int D = 0b0100;
    const int L = 0b0010;
    const int R = 0b0001;

    const int MAX_N = 1000;
    const int MAX_M = 1000;
    int N, M;
    int maze[MAX_N][MAX_M];
    bool vis[MAX_N][MAX_M][4];
    int SR, SC; // start position
    int TR, TC; // target position

    inline int get_val(char c) {
        if (c == '+') return (T | D | L | R);
        if (c == '-') return (L | R);
        if (c == '|') return (T | D);
        if (c == '^') return (T);
        if (c == '<') return (L);
        if (c == '>') return (R);
        if (c == 'v') return (D);
        if (c == 'L') return (T | D | R);
        if (c == 'R') return (T | D | L);
        if (c == 'U') return (D | L | R);
        if (c == 'D') return (T | L | R);
        return 0;
    }

    inline int rotate(int val) {
        int res = 0;
        if (val & T) res |= R;
        if (val & R) res |= D;
        if (val & D) res |= L;
        if (val & L) res |= T;
        return res;
    }

    bool connect(int r, int c, int rot, int dir, int nr, int nc) {
        rot %= 4;
        int val1 = maze[r][c];
        for (int _ = 0; _ < rot; _++)
            val1 = rotate(val1);
        int val2 = maze[nr][nc];
        for (int _ = 0; _ < rot; _++)
            val2 = rotate(val2);

        if (dir == 0) { // T
            return (val1 & T) && (val2 & D);
        }
        if (dir == 1) { // D
            return (val1 & D) && (val2 & T);
        }
        if (dir == 2) { // L
            return (val1 & L) && (val2 & R);
        }
        if (dir == 3) { // R
            return (val1 & R) && (val2 & L);
        }

        return false;
    }

    int bfs() {
        memset(vis, false, sizeof(vis));
        queue<t4i> q;

        q.push(t4i(SR, SC, 0, 0));
        vis[SR][SC][0] = true;

        while (!q.empty()) {
            t4i v = q.front(); q.pop();

            int r = get<0>(v);
            int c = get<1>(v);
            int rot = get<2>(v);
            int level = get<3>(v);

            // printf("%d, %d, %d, %d\n", r, c, rot, level);

            if (r == TR && c == TC)
                return level;

            // neighbors
            for (int dir = 0; dir < 4; dir++) {
                int nr = r + dr[dir];
                int nc = c + dc[dir];
                if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;
                if (!vis[nr][nc][rot % 4] && connect(r, c, rot, dir, nr, nc)) {
                    vis[nr][nc][rot % 4] = true;
                    q.push(t4i(nr, nc, rot, level + 1));
                }
            }

            // rotate
            if (!vis[r][c][(rot + 1) % 4]) {
                vis[r][c][(rot + 1) % 4] = true;
                q.push(t4i(r, c, rot + 1, level + 1));
            }
        }

        return -1;
    }

    int main() {
        scanf("%d %d", &N, &M);
        for (int r = 0; r < N; r++) {
            char inp[MAX_M + 1];
            scanf("%s", inp);
            for (int c = 0; c < M; c++) {
                maze[r][c] = get_val(inp[c]);
            }
        }
        scanf("%d %d", &SR, &SC); SC--; SR--;
        scanf("%d %d", &TR, &TC); TC--; TR--;

        printf("%d\n", bfs());

        return 0;
    }
