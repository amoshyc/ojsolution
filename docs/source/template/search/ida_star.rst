###################################################
IDA*
###################################################

.. sidebar:: Tags

    - ``tag_ida_star``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int dr[4] = {0, 0, +1, -1};
    const int dc[4] = {+1, -1, 0, 0};
    const int dir[4] = {'R', 'L', 'D', 'U'};
    const int INF = 0x3f3f3f3f;
    const int FOUND = -1;
    vector<char> path;
    int A[15][15], Er, Ec;

    int H() {
        int h = 0;
        for (int r = 0; r < 4; r++) {
            for (int c = 0; c < 4; c++) {
                if (A[r][c] == 0) continue;
                int expect_r = (A[r][c] - 1) / 4;
                int expect_c = (A[r][c] - 1) % 4;
                h += abs(expect_r - r) + abs(expect_c - c);
            }
        }
        return h;
    }

    int dfs(int g, int pdir, int bound) {
        int h = H();
        int f = g + h;
        if (f > bound) return f;
        if (h == 0) return FOUND;

        int mn = INF;
        for (int i = 0; i < 4; i++) {
            if (i == (pdir ^ 1)) continue;

            int nr = Er + dr[i];
            int nc = Ec + dc[i];
            if (nr < 0 || nr >= 4) continue;
            if (nc < 0 || nc >= 4) continue;

            path.push_back(dir[i]);
            swap(A[nr][nc], A[Er][Ec]);
            swap(nr, Er); swap(nc, Ec);
            int t = dfs(g + 1, i, bound);
            if (t == FOUND) return FOUND;
            if (t < mn) mn = t;
            swap(nr, Er); swap(nc, Ec);
            swap(A[nr][nc], A[Er][Ec]);
            path.pop_back();
        }

        return mn;
    }

    bool IDAstar() {
        path.clear();
        int bound = H();
        for (;;) {
            int t = dfs(0, -1, bound);
            if (t == FOUND) return true;
            if (t == INF) return false;
            // 下次要搜的 bound >= 50，真的解也一定 >= 50，剪枝
            if (t >= 50) return false;
            bound = t;
        }
        return false;
    }

************************
模板驗證
************************

`uva10181 <../../uva/p10181.html>`_
