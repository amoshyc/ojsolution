###################################################
Treap (implicit key)
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
    struct Treap { // implicit key (key = index)
        int pri, size, val;
        int inc, mn; bool rev;
        Treap *lch, *rch;
        Treap() {}
        Treap(int v) {
            pri = rand();
            size = 1;
            val = v;
            inc = 0;
            mn = v;
            rev = false;
            lch = rch = NULL;
        }
    };

    inline int size(Treap* t) {
        return (t ? t->size : 0);
    }

    // Notice that lch, rch may be NULL
    // flag t->inc is set,
    // => the subtree of t (t not included) is not up-to-date
    // flag t->rev is set,
    // => every node in substree of t (t not included) should
    //    swap its 2 children
    // Every Treap corresponds to a range in array
    inline void push(Treap* t) {
        if (t->rev) {
            if (t->lch) {
                swap(t->lch->lch, t->lch->rch);
                t->lch->rev ^= 1;
            }
            if (t->rch) {
                swap(t->rch->lch, t->rch->rch);
                t->rch->rev ^= 1;
            }
            t->rev = false;
        }

        if (t->inc) {
            if (t->lch) {
                t->lch->val += t->inc;
                t->lch->inc += t->inc;
                t->lch->mn += t->inc;
            }
            if (t->rch) {
                t->rch->val += t->inc;
                t->rch->inc += t->inc;
                t->rch->mn += t->inc;
            }
            t->inc = 0;
        }
    }
    inline void pull(Treap* t) {
        t->size = 1 + size(t->lch) + size(t->rch);

        t->mn = t->val;
        if (t->lch) t->mn = min(t->mn, t->lch->mn);
        if (t->rch) t->mn = min(t->mn, t->rch->mn);
    }

    int NN = 0;
    Treap pool[200000];

    inline Treap* new_treap(int val) {
        pool[NN] = Treap(val);
        return &pool[NN++];
    }

    Treap* merge(Treap* a, Treap* b) { // a < b
        if (!a || !b) return (a ? a : b);
        if (a->pri > b->pri) {
            push(a);
            a->rch = merge(a->rch, b);
            pull(a);
            return a;
        }
        else {
            push(b);
            b->lch = merge(a, b->lch);
            pull(b);
            return b;
        }
    }

    // size(a) will be k
    // t is unable to use afterwards
    void split(Treap* t, Treap*& a, Treap*& b, int k) {
        if (!t) { a = b = NULL; return; }
        push(t);
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

    void add(Treap*& t, int x, int y, int inc) {
        Treap *a, *b, *c, *d;
        split(t, a, b, y); // t -> a, b
        split(a, c, d, x - 1); // a -> c, d
        d->inc += inc;
        d->val += inc;
        d->mn += inc;
        t = merge(merge(c, d), b);
    }

    void reverse(Treap*& t, int x, int y) {
        Treap *a, *b, *c, *d;
        split(t, a, b, y); // t -> a, b
        split(a, c, d, x - 1); // a -> c, d
        swap(d->lch, d->rch);
        d->rev ^= 1;
        t = merge(merge(c, d), b);
    }

    void revolve(Treap*& t, int x, int y, int k) { // 右移 k 位
        int len = y - x + 1;
        Treap *a, *b, *c, *d;
        split(t, a, b, y); // t -> a, b
        split(a, c, d, x - 1); // a -> c, d
        k = k % len;
        Treap *e, *f;
        split(d, e, f, len - k); // d -> e, f
        t = merge(merge(c, merge(f, e)), b);
    }

    void insert(Treap*& t, int x, int val) {
        Treap *a, *b;
        split(t, a, b, x);
        t = merge(merge(a, new_treap(val)), b);
    }

    void remove(Treap*& t, int x) {
        Treap *a, *b, *c, *d;
        split(t, a, b, x - 1); // t -> a, b
        split(b, c, d, 1); // b -> c, d
        t = merge(a, d);
    }

    int get_min(Treap*& t, int x, int y) {
        Treap *a, *b, *c, *d;
        split(t, a, b, y); // t -> a, b
        split(a, c, d, x - 1); // a -> c, d
        int ans = d->mn;
        t = merge(merge(c, d), b);
        return ans;
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

`poj3580 <../../poj/p3580.html>`_
