#####################################
[cf181] C. Beautiful Numbers
#####################################

.. sidebar:: Tags

    - ``tag_enum``
    - ``tag_combination``

.. contents:: TOC
    :depth: 2


**********************************************************
`題目 <http://codeforces.com/contest/300/problem/C>`_
**********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 枚舉

簡單可得出解為::

    sum(C(N, x) % M for x in [0, N]) % M

那如何計算組合數 C(a, b) % M 呢？利用 mod inverse 可以做。
因為::

    if M is prime, then a^(-1) ≡ a^(M-2) (mod M)

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int MAX_N = 1e6;
    const ll M = 1e9 + 7;

    int A, B, N;
    ll fac[MAX_N + 1];

    ll fast_pow(ll a, ll b) {
        ll ans = 1;
        ll base = a % M;
        while (b) {
            if (b & 1)
                ans = ans * base % M;
            base = base * base % M;
            b >>= 1;
        }
        return ans;
    }

    void fac_init() {
        fac[0] = 1;
        for (int i = 1; i <= N; i++)
            fac[i] = fac[i - 1] * i % M;
    }

    ll inv(ll n) {
        return fast_pow(n, M - 2) % M;
    }

    ll C(ll a, ll b) {
        // a! / (b! * (a-b)!)
        return fac[a] * inv(fac[b] * fac[a - b]) % M;
    }

    bool is_good(int n) {
        while (n) {
            if ((n % 10) != A && (n % 10) != B)
                return false;
            n /= 10;
        }
        return true;
    }

    int main() {
        scanf("%d %d %d", &A, &B, &N);

        fac_init();

        ll ans = 0;
        for (int i = 0; i <= N; i++) {
            // i 個 A, n - i 個 B
            int sum_digit = i * A + (N - i) * B;
            if (is_good(sum_digit)) {
                ans = (ans + C(N, i)) % M;
            }
        }

        printf("%lld\n", ans);

        return 0;
    }
