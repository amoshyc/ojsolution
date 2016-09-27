###################################################
Matrix Determinant
###################################################

.. sidebar:: Tags

    - ``tag_gaussian``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

整數版本，不使用浮點數。
求 determinant，教學請參
`<http://blog.csdn.net/zhoufenqin/article/details/7779707>`_

用 Gaussian elimination （不是 Gaussian-Jordan elimination）把矩陣上三角化，主對角線的項的乘積即為答案

.. code-block:: cpp
    :linenos:

    typedef long long ll;
    typedef vector<ll> vec;
    typedef vector<vec> mat;

    ll determinant(mat m) { // square matrix
        const int n = m.size();
        ll det = 1;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int a = i, b = j;
                while (m[b][i]) {
                    ll q = m[a][i] / m[b][i];
                    for (int k = 0; k < n; k++)
                        m[a][k] = m[a][k] - m[b][k] * q;
                    swap(a, b);
                }

                if (a != i) {
                    swap(m[i], m[j]);
                    det = -det;
                }
            }

            if (m[i][i] == 0)
                return 0;
            else
                det *= m[i][i];
        }
        return det;
    }

************************
模板驗證
************************

`uva684 <../../uva/p684.html>`_
