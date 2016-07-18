###################################################
Sparse Table
###################################################

.. sidebar:: Tags

    - ``tag_sparse_table``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    int sptb[MAX_LOG_N][MAX_N];
    // sptb[i][j] = max value of [j, j + 2^i)

    void sptb_init() {
        for (int i = 0; i < N; i++) {
            sptb[0][i] = A[i];
        }

        for (int i = 1; (1 << i) <= N; i++) {
            for (int j = 0; j + (1 << i) <= N; j++) {
                sptb[i][j] = max(sptb[i - 1][j], sptb[i - 1][j + (1 << (i - 1))]);
            }
        }
    }

    // [l, r)
    int sptb_query(int l, int r) {
        int k = floor(log2(double(r - l)));
        int res1 = sptb[k][l];
        int res2 = sptb[k][r - (1 << k)];
        return max(res1, res2);
    }

************************
模板驗證
************************

`poj3368 <../../poj/p3368.html>`_
