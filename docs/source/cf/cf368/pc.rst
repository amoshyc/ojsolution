#####################################
[cf368] C. Pythagorean Triples
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/707/problem/C>`_
******************************************************

勾股數定義為 tuple (a, b, c)，其中 a, b, c > 0, a^2 + b^2 = c^2
現給定一正整數 m，請輸出任一個包含 m 的勾股數的另外兩個數 n, k
注意，m 可為直角三角形的股或斜邊。

************************
Specification
************************

::

    1 <= m <= 10^9
    1 <= n, k <= 10^18

************************
分析
************************

.. note:: 勾股數公式

我是知道勾股數公式的:

.. math::

    (i &> j > 0) \\
    a &= i^2 - j^2 \\
    b &= 2ij \\
    c &= i^2 + j^2 \\

我也想到了，如果給定的 m 是偶數的話，可以直接視為 m 為 b，然後取 j = 1, i = n/2
但我沒想到如果 m 是奇數的話，也可以推出公式。
視 m 為 a，取 j = (a - 1) / 2, i = j + 1 即可，可惜。

比賽中我想不到好的方法，就枚舉 j 了，然後也就 TLE 了，死在大質數。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        ll a;
        scanf("%lld", &a);

        if (a <= 2) {
            puts("-1");
            return 0;
        }

        if (a & 1) {
            // a = m^2 - n^2 (只求一組解)
            // 1 * a = (m + n)(m - n)
            // n = (a - 1) / 2
            // m = n + 1
            ll n = (a - 1) / 2;
            ll m = n + 1;

            ll b = 2 * m * n;
            ll c = m * m + n * n;
            printf("%lld %lld\n", b, c);
        }
        else {
            // a = 2 * m * n (只求一組解，取 n = 1)
            // n = 1
            // m = a / 2
            ll n = 1;
            ll m = a / 2;

            ll b = m * m - n * n;
            ll c = m * m + n * n;
            printf("%lld %lld\n", b, c);
        }

        return 0;
    }

************************
TLE Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    typedef long double ldb;

    int main() {
        ll a; scanf("%lld", &a);

        const ll mx = 1e9;

        if (a < 3) {
            puts("-1");
            return 0;
        }

        if (a % 2 == 0) {
            ll n = 1;
            ll m = a / 2 / n;
            if (m * n * 2 == a) {
                if (m < n) swap(m, n);
                ll b = m * m - n * n;
                ll c = m * m + n * n;

                printf("%lld %lld\n", b, c);
                return 0;
            }
        }

        for (ll n = 1; n <= mx; n++) {
            { // 邊
                ll m_sq = a + n * n;

                ll m = sqrt(ldb(m_sq));
                if (m * m == m_sq) {
                    if (m < n) swap(m, n);

                    ll b = 2 * m * n;
                    ll c = m * m + n * n;

                    printf("%lld %lld\n", b, c);
                    return 0;
                }
            }
            { // 斜邊
                ll m_sq = a - n * n;

                if (m_sq == 0) continue;

                ll m = sqrt(ldb(m_sq));
                if (m * m == m_sq) {
                    if (m < n) swap(m, n);

                    ll b = 2 * m * n;
                    ll c = m * m - n * n;

                    printf("%lld %lld\n", b, c);
                    return 0;
                }
            }
        }

        puts("-1");

        return 0;
    }
