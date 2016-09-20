###################################################
Treap (bst)
###################################################

.. sidebar:: Tags

    - ``tag_treap``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    // Remember srand(time(NULL)), but it cannot be used on poj(g++)
    struct Treap { // val: bst, pri: heap
        int pri, size, val;
        Treap *lch, *rch;
        Treap() {}
        Treap(int v) {
            pri = rand();
            size = 1;
            val = v;
            lch = rch = NULL;
        }
    };

    inline int size(Treap* t) {
        return (t ? t->size : 0);
    }
    inline void pull(Treap* t) {
        t->size = 1 + size(t->lch) + size(t->rch);
    }

    int NN = 0;
    Treap pool[30000];

    Treap* merge(Treap* a, Treap* b) { // a < b
        if (!a || !b) return (a ? a : b);
        if (a->pri > b->pri) {
            a->rch = merge(a->rch, b);
            pull(a);
            return a;
        }
        else {
            b->lch = merge(a, b->lch);
            pull(b);
            return b;
        }
    }

    void split(Treap* t, Treap*& a, Treap*& b, int k) {
        if (!t) { a = b = NULL; return; }
        if (size(t->lch) < k) {
            a = t;
            split(t->rch, a->rch, b, k - size(t->lch) - 1);
            pull(a);
        }
        else {
            b = t;
            split(t->lch, a, b->lch, k);
            pull(b);
        }
    }

    // get the rank of val
    // result is 1-based
    int get_rank(Treap* t, int val) {
        if (!t) return 0;
        if (val < t->val)
            return get_rank(t->lch, val);
        else
            return get_rank(t->rch, val) + size(t->lch) + 1;
    }

    // get kth smallest item
    // k is 1-based
    Treap* get_kth(Treap*& t, int k) {
        Treap *a, *b, *c, *d;
        split(t, a, b, k - 1);
        split(b, c, d, 1);
        t = merge(a, merge(c, d));
        return c;
    }

    void insert(Treap*& t, int val) {
        int k = get_rank(t, val);
        Treap *a, *b;
        split(t, a, b, k);
        pool[NN] = Treap(val);
        Treap* n = &pool[NN++];
        t = merge(merge(a, n), b);
    }

For debugging

.. code-block:: cpp
    :linenos:

    void pp(Treap* t) {
        printf("(size = %d, val = %d, pri = %d)\n", t->size, t->val, t->pri);
    }

    void dfs(Treap* t, string ind = "") {
        printf("%s", ind.c_str());
        if (!t) puts("NULL");
        else {
            pp(t);
            dfs(t->lch, ind + "L   ");
            dfs(t->rch, ind + "R   ");
        }
    }

    void inorder(Treap* t) {
        if (!t) return;
        inorder(t->lch);
        pp(t);
        inorder(t->rch);
    }

************************
模板驗證
************************

`uva501 <../../uva/p501.html>`_
