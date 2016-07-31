###################################################
BIT
###################################################

.. sidebar:: Tags

    - ``tag_bit``
    - ``tag_template``

.. contents:: TOC
    :depth: 3

************************
程式碼
************************

==================
Structure Version
==================

Declaration

.. code-block:: cpp

    template <class T>
    struct BIT {
        int R;
        vector<T> v;
        BIT();
        BIT(int r);
        T sum(int idx);
        void add(int idx, T x);
    };

Implementation


.. code-block:: cpp

    template <class T>
    BIT<T>::BIT(){
        ;
    }

    template <class T>
    BIT<T>::BIT(int r): R(r) {
        v = vector<T>(R + 1, 0);
    }

    template <class T>
    T BIT<T>::sum(int idx) {
        T s = 0;
        for (int i = idx; i > 0; i -= (i & -i))
            s += v[i];
        return s;
    }

    template <class T>
    void BIT<T>::add(int idx, T x) {
        for (int i = idx; i <= N; i += (i & -i))
            v[i] += x;
    }

==================
Function Version
==================

.. code-block:: cpp
    :linenos:

    const int MAX_R = 20000;
    int R;
    ll bit[MAX_R + 1];

    ll sum(int i) {
        ll s = 0;
        while (i > 0) {
            s += bit[i];
            i -= (i & -i);
        }
        return s;
    }

    void add(int i, ll x) {
        while (i <= R) {
            bit[i] += x;
            i += (i & -i);
        }
    }

************************
模板驗證
************************

`poj1990 <http://codepad.org/UeDMdncD>`_
