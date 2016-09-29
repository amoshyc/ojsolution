#####################################
[cfother] D. Secret Polynomial
#####################################

.. sidebar:: Tags

    - ``tag_gaussian_jordan``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <https://bitbucket.org/mzshieh/nctu-annual-2016>`_
**********************************************************

多項式 :math:`f(x) = c_0 + c_1 x + c_2 x^2 + \dots + c_{n-1} x^{n-1}`
保證 :math:`c_i < 10^9+7`，給定 n 個序對 :math:`<x, f(x) \mod (1e9 + 7)>`，請輸出 :math:`c_i`

************************
Specification
************************

************************
分析
************************

.. note:: 聯立方程

把 x 代入得到聯立方程，Gaussian Jordan 求解。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

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

    int main() {
        int TC;
        scanf("%d", &TC);
        while (TC--) {
            int N;
            scanf("%d", &N);

            mat A(N, vec(N + 1, 0));
            for (int r = 0; r < N; r++) {
                ll x, v; scanf("%lld %lld", &x, &v);
                for (int c = 0; c < N; c++) {
                    A[r][c] = fast_pow(x, c);
                }
                A[r][N] = v % MOD;
            }

            vec coef = gauss_jordan(A);
            for (int i = 0; i < N; i++) {
                if (i != 0) printf(" ");
                printf("%lld", coef[i]);
            }
            puts("");
        }

        return 0;
    }
