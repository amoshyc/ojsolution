###################################################
Trie (Fixed Length)
###################################################

存固定長度資料的 Trie

.. sidebar:: Tags

    - ``tag_trie``
    - ``tag_template``

.. contents:: TOC
    :depth: 3


*************************************
程式碼（靜態儲存．非遞迴）
*************************************

.. code-block:: cpp
    :linenos:

    struct Node {
        int cnt;
        Node* nxt[2];
        Node() {
            cnt = 0;
            fill(nxt, nxt + 2, nullptr);
        }
    };

    const int MAX_Q = 200000;
    int Q;

    int NN = 0;
    Node data[MAX_Q * 30];
    Node* root = &data[NN++];

    void insert(Node* u, int x) {
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            if (u->nxt[t] == nullptr) {
                u->nxt[t] = &data[NN++];
            }

            u = u->nxt[t];
            u->cnt++;
        }
    }

    void remove(Node* u, int x) {
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            u = u->nxt[t];
            u->cnt--;
        }
    }

    int query(Node* u, int x) {
        int res = 0;
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            // if it is possible to go the another branch
            // then the result of this bit is 1
            if (u->nxt[t ^ 1] != nullptr && u->nxt[t ^ 1]->cnt > 0) {
                u = u->nxt[t ^ 1];
                res |= (1 << i);
            }
            else {
                u = u->nxt[t];
            }
        }
        return res;
    }

************************
程式碼（動態配置）
************************

.. code-block:: cpp
    :linenos:

    struct Node {
        int cnt;
        ui val;
        Node* nxt[2];
    };

    Node* insert(Node* u, ui x, int idx) {
        if (u == NULL) {
            u = new Node;
            u->val = -1;
            u->cnt = 0;
            for (int i = 0; i < 2; i++)
                u->nxt[i] = NULL;
        }

        if (idx == -1) {
            u->val = x;
            u->cnt++;
            return u;
        }

        int flag = ((x & (1 << idx)) ? 1 : 0);
        u->nxt[flag] = insert(u->nxt[flag], x, idx - 1);

        return u;
    }

    Node* remove(Node* u, ui x, int idx) {
        if (idx == -1) {
            u->cnt--;
            if (u->cnt == 0) {
                delete u;
                return NULL;
            }
            return u;
        }

        int flag = ((x & (1 << idx)) ? 1 : 0);
        u->nxt[flag] = remove(u->nxt[flag], x, idx - 1);

        auto isNULL = [](const Node* v) { return v == NULL; };
        if (all_of(u->nxt, u->nxt + 2, isNULL)) {
            delete u;
            return NULL;
        }

        return u;
    }

    Node* query(Node* u, ui x, int idx) {
        if (idx == -1) {
            return u;
        }

        if (u->nxt[1] == NULL) return query(u->nxt[0], x, idx - 1);
        if (u->nxt[0] == NULL) return query(u->nxt[1], x, idx - 1);

        if ((x & (1 << idx)) == 0) { // left
            return query(u->nxt[0], x, idx - 1);
        }
        else { // right
            return query(u->nxt[1], x, idx - 1);
        }
    }

************************
模板驗證
************************

- `cf #367 D 靜態儲存 <http://codeforces.com/contest/706/submission/19882408>`_
- `cf #367 D 動態配置 <http://codeforces.com/contest/706/submission/19863621>`_
