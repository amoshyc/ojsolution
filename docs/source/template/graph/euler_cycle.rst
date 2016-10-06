###################################################
Euler Cycle
###################################################

.. sidebar:: Tags

    - ``tag_euler_cycle``
    - ``tag_template``

.. contents:: TOC
    :depth: 3

************************
有向圖
************************

在有向圖上找 Euler Cycle，以下為 Hierholzer's algorithm，時間 O(E)
圖解可參 `<http://www.voidcn.com/blog/pacosonswjtu/article/p-4884825.html>`_

第一份 code 為手寫 linked list，第二份為使用 C++ 內建的 list（可讀性較高，但很慢）。

==========================
程式碼（手寫 linkedlist）
==========================

.. code-block:: cpp
    :linenos:

    const int MAX_N = 10000;
    const int MAX_M = 50000;

    // list
    struct Node {
        int val;
        Node *nxt;
    };
    int NN = 0;
    Node pool[MAX_M + 1]; // 給定的圖 E 條有向邊，環最大有 E + 1 個節點
    Node* root = NULL;

    Node* new_node(int v) {
        Node* it = &pool[NN++];
        it->val = v;
        it->nxt = NULL;
        return it;
    }

    // graph
    int N, M; // V, E
    vector<int> cycle; // 存找到的（小）歐拉環
    vector<int> g[MAX_N];

    void dfs(int u) {
        cycle.push_back(u);
        if (g[u].size() > 0) {
            int v = g[u].back();
            g[u].pop_back();
            dfs(v);
        }
    }

    void find_euler_cycle(int s) {
        root = new_node(s);
        for (Node* it = root; it != NULL; it = it->nxt) {
            if (g[it->val].size() > 0) {
                cycle.clear();
                dfs(it->val);

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

        find_euler_cycle(0); // 如果圖是連通的

        for (Node* it = root; it != NULL; it = it->nxt) {
            printf("%d\n", (it->val) + 1);
        }

        // for (int u = 0; u < N; u++) { // 圖是多個 disjoint 單元
        //     if (g[u].size() > 0) {
        //         find_euler_cycle(u);
        //
        //         for (Node* it = root; it->nxt != NULL; it = it->nxt) {
        //             // ...
        //         }
        //     }
        // }

        return 0;
    }

===========================
程式碼（c++ 內建 list）
===========================

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
無向圖
************************

改為使用 multiset<int> g[MAX_N] 來存圖，整體時間為 O(ElgE)

==========================
程式碼
==========================

.. code-block:: cpp
    :linenos:

    const int MAX_N = 200;
    const int MAX_M = MAX_N * (MAX_N + 1) / 2;

    // list
    struct Node {
        int val;
        Node *nxt;
    };
    int NN = 0;
    Node pool[MAX_M + 1]; // 給定的圖 E 條有向邊，環最大有 E + 1 個節點
    Node* root = NULL;

    Node* new_node(int v) {
        Node* it = &pool[NN++];
        it->val = v;
        it->nxt = NULL;
        return it;
    }

    // graph
    int N, M; // V, E
    vector<int> cycle; // 存找到的（小）歐拉環
    multiset<int> g[MAX_N]; // 用 multiset 存

    void dfs(int u) {
        cycle.push_back(u);
        if (g[u].size() > 0) {
            int v = *g[u].begin();
            g[u].erase(g[u].begin()); // remove 1 edge
            g[v].erase(g[v].find(u)); // remove 1 edge
            dfs(v);
        }
    }

    void find_euler_cycle(int s) {
        root = new_node(s);
        for (Node* it = root; it != NULL; it = it->nxt) {
            if (g[it->val].size() > 0) {
                cycle.clear();
                dfs(it->val);

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
        // 圖建雙向邊
        for (int u = 0; u < N; u++) { // 圖是多個 disjoint 單元
            if (g[u].size() > 0) {
                find_euler_cycle(u);

                for (Node* it = root; it->nxt != NULL; it = it->nxt) {
                    // ...
                }
            }
        }
    }

************************
模板驗證
************************

`poj2230 <../../poj/p2230.html>`_ （有向圖）
`cf375e <../../cf/cf375/pe.html>`_ （無向圖）
