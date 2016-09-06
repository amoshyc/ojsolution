###################################################
AC Automaton
###################################################

.. sidebar:: Tags

    - ``tag_ac_automaton``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    const int KK = 26;

    struct Node {
        int val;
        int cnt;
        Node* fail;
        Node* nxt[KK];
        Node() {
            val = -1;
            cnt = 0;
            fail = nullptr;
            fill(nxt, nxt + KK, nullptr);
        }
    };

    int NN;
    Node data[150 * 70 + 100];

    void build(Node* root) {
        queue<Node*> que;

        root->fail = nullptr;
        que.push(root);

        while (!que.empty()) {
            Node* r = que.front(); que.pop();

            for (int t = 0; t < KK; t++) {
                Node* u = r->nxt[t];
                if (u == nullptr)
                    continue;

                // new transition t
                // v = r.fail
                //  ↖
                //    r -> u = r.nxt[t]
                // at r currently, where r->fail is known
                // want to compute u->fail
                // i.e. the state to go when mismatch occurs at u

                Node* v = r->fail;
                while (v != nullptr && v->nxt[t] == nullptr) {
                    v = v->fail;
                }

                if (v == nullptr) // There's no transition available
                    u->fail = root;
                else // There's a transition at v
                    u->fail = v->nxt[t];

                que.push(u);
            }
        }
    }

    void insert(Node* root, const string& p, int val) {
        Node* u = root;

        for (const char& c : p) {
            int t = int(c - 'a');
            if (u->nxt[t] == nullptr) {
                data[NN] = Node();
                u->nxt[t] = &data[NN++];
            }
            u = u->nxt[t];
        }

        (u->cnt)++;
        u->val = val;
    }

    void query(Node* root, const string& s, vector<int>& res) {
        Node* u = root;

        for (const char& c : s) {
            int t = int(c - 'a');
            while (u != nullptr && u->nxt[t] == nullptr) {
                u = u->fail; // notice that only root's fail is nullptr
            }

            if (u == nullptr) { // no transition t available (including root)
                u = root; // setting u for next char
                continue;
            }
            else { // There's a transition
                u = u->nxt[t];
                // if there's a match at u, then check all other patterns p
                // if there's a longest p form a match at u, too
                // then p must be L(u)'s longest suffix
                Node* v = u;
                while (v != root) {
                    if (v->cnt > 0) {
                        res[v->val]++;
                    }
                    v = v->fail;
                }
            }
        }
    }

************************
模板驗證
************************

`uva1449 <../../uva/p1449.html>`_
