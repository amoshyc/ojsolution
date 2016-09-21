###################################################
矩陣快速冪
###################################################

.. sidebar:: Tags

    - ``tag_matrix``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    typedef long long ll;
    typedef vector<ll> vec;
    typedef vector<vec> mat;

    int MOD;

    mat mul(const mat& a, const mat& b) {
        mat res(a.size(), vec(b[0].size(), 0));
        for (int r = 0; r < int(a.size()); r++)
            for (int c = 0; c < int(b[0].size()); c++)
                for (int k = 0; k < int(b.size()); k++)
                    res[r][c] = (res[r][c] + a[r][k] * b[k][c] % MOD) % MOD;

        return res;
    }

    mat pow(const mat& a, int b) {
        mat ans(a.size(), vec(a.size(), 0));
        for (int i = 0; i < int(a.size()); i++)
            ans[i][i] = 1;

        mat base = a;
        while (b) {
            if (b & 1) ans = mul(ans, base);
            base = mul(base, base);
            b >>= 1;
        }

        return ans;
    }

************************
模板驗證
************************

`poj2739 <../../poj/p3233.html>`_
