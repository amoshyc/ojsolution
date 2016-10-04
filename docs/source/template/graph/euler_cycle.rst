###################################################
Euler Cycle
###################################################

.. sidebar:: Tags

    - ``tag_euler_cycle``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
原理
************************

在有向圖上找 Euler Cycle，以下為 Hierholzer's algorithm，時間 O(E)
圖解可參 `<http://www.voidcn.com/blog/pacosonswjtu/article/p-4884825.html>`_

第一份 code 為手寫 linked list，第二份為使用 C++ 內建的 list（可讀性較高，但很慢）。

***************************
程式碼（手寫 linkedlist）
***************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 10000;
    const int MAX_M = 50000;

    // graph
    int N, M;
    vector<int> g[MAX_N];

    vector<int> cycle;
    void dfs(int u) {
        cycle.push_back(u);
        if (g[u].size() > 0) {
            int v = g[u].back();
            g[u].pop_back();
            dfs(v);
        }
    }

    // list
    struct Node {
        int data;
        Node *nxt;
    };
    int NN = 0;
    Node pool[MAX_M * 2 + 1]; // 給定的圖 E 條邊，最大的環有 E * 2 + 1 個節點
    Node* root = NULL;

    Node* new_node(int v) {
        Node* it = &pool[NN++];
        it->data = v;
        it->nxt = NULL;
        return it;
    }

    void find_euler_cycle() {
        root = new_node(0);

        for (Node* it = root; it != NULL; it = it->nxt) {
            int u = it->data;
            if (g[u].size() > 0) {
                cycle.clear();
                dfs(u);

                Node* nxt = it->nxt;
                Node* pre = it;
                for (int i = 1; i < int(cycle.size()); i++) {
                    Node* temp = new_node(cycle[i]);
                    pre->nxt = temp;
                    pre = temp;
                }
                pre->nxt = nxt;
            }
        }
    }

    int main() {
        // ...

        find_euler_cycle();

        for (Node* it = root; it != NULL; it = it->nxt) {
            printf("%d\n", (it->data) + 1);
        }

        return 0;
    }

***************************
程式碼（c++ 內建 list）
***************************

.. code-block:: cpp
    :linenos:

    const int MAX_N = 10000;
    int N, M;
    vector<int> g[MAX_N];

    vector<int> cycle;
    void dfs(int u) {
        cycle.push_back(u);
        if (g[u].size() > 0) {
            int v = g[u].back();
            g[u].pop_back();
            dfs(v);
        }
    }

    list<int> find_euler_cycle() {
        list<int> res;
        res.push_back(0);

        for (list<int>::iterator it = res.begin(); it != res.end(); it++) {
            if (g[*it].size() > 0) {
                cycle.clear();
                dfs(*it);
                list<int>::iterator pos = it; ++pos;
                res.insert(pos, cycle.begin() + 1, cycle.end());
            }
        }

        return res;
    }

    int main() {
        // ...

        list<int> ans = find_euler_cycle();
        for (list<int>::iterator it = ans.begin(); it != ans.end(); ++it) {
            printf("%d\n", *it + 1);
        }

        return 0;
    }

************************
模板驗證
************************

`poj2230 <../../poj/p2230.html>`_
