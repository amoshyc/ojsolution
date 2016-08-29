###################################################
Max Flow
###################################################

.. sidebar:: Tags

    - ``tag_max_flow``
    - ``tag_template``

.. contents:: TOC
    :depth: 3

************************
程式碼
************************

=================
Structure Version
=================

.. code-block:: cpp
    :linenos:

    // ===== Declaration of DinicGraph =====
    struct Edge {
        int to, cap, rev;
    };
    struct DinicGraph {
        const static int INF = 0x3f3f3f3f;
        int V;
        vector< vector<Edge> > g;
        vector<int> iter;
        vector<int> level;
        DinicGraph();
        DinicGraph(int v);
        void add_edge(int u, int v, int cap);
        void bfs(int s);
        int dfs(int v, int t, int f);
        int max_flow(int s, int t);
    };
    // ===== end of Declaration =====


.. code-block:: cpp
    :linenos:

    // ===== Implementation of DinicGraph =====
    DinicGraph::DinicGraph() {
        ;
    }

    DinicGraph::DinicGraph(int v): V(v) {
        g = vector< vector<Edge> >(V, vector<Edge>());
        iter = vector<int>(V, 0);
        level = vector<int>(V, 0);
    }

    void DinicGraph::add_edge(int u, int v, int cap) {
        g[u].push_back((Edge){v, cap, (int)g[v].size()});
        g[v].push_back((Edge){u, 0, (int)g[u].size() - 1});
    }

    void DinicGraph::bfs(int s) {
        fill(level.begin(), level.end(), -1);
        queue<int> q;

        level[s] = 0;
        q.push(s);

        while (!q.empty()) {
            int v = q.front(); q.pop();
            for (int i = 0; i < int(g[v].size()); i++) {
                const Edge& e = g[v][i];
                if (e.cap > 0 && level[e.to] < 0) {
                    level[e.to] = level[v] + 1;
                    q.push(e.to);
                }
            }
        }
    }

    int DinicGraph::dfs(int v, int t, int f) {
        if (v == t) return f;
        for (int& i = iter[v]; i < int(g[v].size()); i++) {
            Edge& e = g[v][i];
            if (e.cap > 0 && level[v] < level[e.to]) {
                int d = dfs(e.to, t, min(f, e.cap));
                if (d > 0) {
                    e.cap -= d;
                    g[e.to][e.rev].cap += d;
                    return d;
                }
            }
        }
        return 0;
    }

    int DinicGraph::max_flow(int s, int t) { // dinic
        int flow = 0;
        for (;;) {
            bfs(s);
            if (level[t] < 0) return flow;
            fill(iter.begin(), iter.end(), 0);
            int f;
            while ((f = dfs(s, t, INF)) > 0) {
                flow += f;
            }
        }
    }
    // ===== end of Implementation =====

================
Function Version
================

.. code-block:: cpp
    :linenos:

    struct Edge {
        int to, cap, rev;
        Edge(int a, int b, int c) {
            to = a;
            cap = b;
            rev = c;
        }
    };

    const int INF = 0x3f3f3f3f;
    const int MAX_V = 20000 + 10;
    // vector<Edge> g[MAX_V];
    vector< vector<Edge> > g(MAX_V);
    int level[MAX_V];
    int iter[MAX_V];

    inline void add_edge(int u, int v, int cap) {
        g[u].push_back((Edge){v, cap, (int)g[v].size()});
        g[v].push_back((Edge){u, 0, (int)g[u].size() - 1});
    }

    void bfs(int s) {
        memset(level, -1, sizeof(level));
        queue<int> q;

        level[s] = 0;
        q.push(s);

        while (!q.empty()) {
            int v = q.front(); q.pop();
            for (int i = 0; i < int(g[v].size()); i++) {
                const Edge& e = g[v][i];
                if (e.cap > 0 && level[e.to] < 0) {
                    level[e.to] = level[v] + 1;
                    q.push(e.to);
                }
            }
        }
    }

    int dfs(int v, int t, int f) {
        if (v == t) return f;
        for (int& i = iter[v]; i < int(g[v].size()); i++) {
            Edge& e = g[v][i];
            if (e.cap > 0 && level[v] < level[e.to]) {
                int d = dfs(e.to, t, min(f, e.cap));
                if (d > 0) {
                    e.cap -= d;
                    g[e.to][e.rev].cap += d;
                    return d;
                }
            }
        }
        return 0;
    }

    int max_flow(int s, int t) { // dinic
        int flow = 0;
        for (;;) {
            bfs(s);
            if (level[t] < 0) return flow;
            memset(iter, 0, sizeof(iter));
            int f;
            while ((f = dfs(s, t, INF)) > 0) {
                flow += f;
            }
        }
    }

************************
模板驗證
************************

`poj3281 <../../poj/p3281.html>`_
