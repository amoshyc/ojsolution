###################################################
Gaussian Jordan Elimination
###################################################

.. sidebar:: Tags

    - ``tag_gaussian_jordan``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

所有操作都在 :math:`Z_{MOD}` 底下

.. code-block:: cpp
    :linenos:

    typedef long long ll;
    typedef vector<ll> vec;
    typedef vector<vec> mat;

    const ll MOD = 1e9 + 7;

    ll fast_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a;
        while (b) {
            if (b & 1)
                ans = ans * base % MOD;
            base = base * base % MOD;
            b >>= 1;
        }
        return ans;
    }

    ll inv(ll a) {
        return fast_pow(a, MOD - 2);
    }

    vec gauss_jordan(mat A) {
        int n = A.size(), m = A[0].size();
        for (int i = 0; i < n; i++) {
            // find min j s.t. A[j][i] is not 0
            int pivot = i;
            for (; pivot < n; pivot++) {
                if (A[pivot][i] != 0) {
                    break;
                }
            }

            swap(A[i], A[pivot]);
            if (A[i][i] == 0) { // 無解或無限多組解
                return vec();
            }

            ll divi = inv(A[i][i]);
            for (int j = i; j < m; j++) {
                A[i][j] = (A[i][j] * divi) % MOD;
            }

            for (int j = 0; j < n; j++) {
                if (j != i) {
                    for (int k = i + 1; k < m; k++) {
                        // A[j][k] -= A[j][i] * B[i][k];
                        ll p = (A[j][i] * A[i][k]) % MOD;
                        A[j][k] = (A[j][k] - p + MOD) % MOD;
                    }
                }
            }
        }

        vec x(n);
        for (int i = 0; i < n; i++)
        x[i] = A[i][m - 1];

        return x;
    }

************************
模板驗證
************************

`交大校內賽 2016 D <../../other/nctu_annual_2016/pd.html>`_
