#####################################
[cf368] D. Persistent Bookcase
#####################################

.. sidebar:: Tags

    - ``tag_dfs``
    - ``tag_offline``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/707/problem/D>`_
******************************************************

有一個 NxM 的書櫃，一開始，每個格子都沒有書。
現有 Q 筆操作，針對每個操作，請輸出該操作結束後，書櫃中書的數量。

操作可能為：
1. ``1 i j``: 如果格子 (i, j) 沒有書，放入一本書
2. ``2 i j``: 如果格子 (i, j) 有書，將之移除
3. ``3 i``: 反轉 i-th row 的每一個格子（有→無，無→有）
4. ``4 k``: 將書櫃變成第 k 筆操作後的狀態。

************************
Specification
************************

::

    1 <= N, M <= 10^3
    1 <= Q <= 10^5
    Memory <= 512 MB

************************
分析
************************

.. note:: 離線處理

第 4 種操作與記憶體的限制造成我們得用可持久化的資料結構或離線處理。
以下給出離線處理的方法，使用 dfs。長知識了，第一次見到這種解法。
這裡假設 Q 陣列是 1-based index。

--------------------------

嘗試把操作 4 先處理掉，把每個操作視為一個節點，補一個節點（節點 0）記錄一開始的狀態。
那這 Q 筆操作（Q + 1 個節點）會形成一棵樹，其中節點 0 是 root。


其中節點 u:
1. 如果操作 Q[u] 是第 ``1~3`` 種操作，那節點 u 的 parent 是節點 u - 1
2. 如果操作 Q[u] 是 ``4 k``，那節點 u 的 parent 是節點 k

樹建出來後，我們就可以從 root dfs，直接模擬操作。
我們只需模擬操作 1 ~ 3，這三個操作都可以在 O(1) 模擬。
另外記得 dfs 往上溯時，要把狀態還原。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    struct Query {
        int id, cmd, i, j, ans;
    };

    const int MAX_N = 1000;
    const int MAX_M = 1000;
    const int MAX_Q = 100000;

    int N, M, Q;
    bool A[MAX_N][MAX_M];
    vector<int> g[MAX_Q + 1];
    vector<Query> query;

    int cnt = 0;
    int cntR[MAX_N];
    bool inv[MAX_N];

    inline bool get(int i, int j) {
        return A[i][j] ^ inv[i];
    }

    bool apply(const Query& q) {
        bool state = false;
        if (q.cmd == 1) {
            state = get(q.i, q.j);
            if (state == false) {
                cnt++;
                cntR[q.i]++;
                A[q.i][q.j] ^= 1;
            }
        }
        if (q.cmd == 2) {
            state = get(q.i, q.j);
            if (state == true) {
                cnt--;
                cntR[q.i]--;
                A[q.i][q.j] ^= 1;
            }
        }
        if (q.cmd == 3) {
            inv[q.i] ^= 1;
            cnt -= cntR[q.i];
            cntR[q.i] = M - cntR[q.i];
            cnt += cntR[q.i];
        }
        return state;
    }

    void undo(const Query& q, bool state) {
        if (q.cmd == 1) {
            if (state == false) {
                cnt--;
                cntR[q.i]--;
                A[q.i][q.j] ^= 1;
            }
        }
        if (q.cmd == 2) {
            if (state == true) {
                cnt++;
                cntR[q.i]++;
                A[q.i][q.j] ^= 1;
            }
        }
        if (q.cmd == 3) {
            inv[q.i] ^= 1;
            cnt -= cntR[q.i];
            cntR[q.i] = M - cntR[q.i];
            cnt += cntR[q.i];
        }
    }

    void dfs(int u) {
        bool originState = apply(query[u]);

        query[u].ans = cnt;
        for (int v : g[u]) {
            dfs(v);
        }

        undo(query[u], originState);
    }

    int main () {
        scanf("%d %d %d", &N, &M, &Q);

        query.push_back((Query) {0, -1, -1, -1, -1});
        for (int id = 1; id <= Q; id++) {
            int cmd; scanf("%d", &cmd);
            if (cmd <= 2) {
                int i, j; scanf("%d %d", &i, &j); i--; j--;
                query.push_back((Query) {id, cmd, i, j, -1});
                g[id - 1].push_back(id);
            }
            else if (cmd == 3) {
                int i; scanf("%d", &i); i--;
                query.push_back((Query) {id, cmd, i, -1, -1});
                g[id - 1].push_back(id);
            }
            else {
                int k; scanf("%d", &k);
                query.push_back((Query) {id, cmd, -1, -1, -1});
                g[k].push_back(id);
            }
        }

        dfs(0);
        for (int i = 1; i <= Q; i++)
            printf("%d\n", query[i].ans);

        return 0;
    }
