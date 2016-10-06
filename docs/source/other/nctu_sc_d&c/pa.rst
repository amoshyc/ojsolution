#####################################
[cf321] B. Kefa and Company
#####################################

.. sidebar:: Tags

    - ``tag_two_pointers``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/580/problem/B>`_
******************************************************

N 個人，每個人有友情值 f[i] 與金錢 m[i]，請選出人的子集 S，S 中金錢的最小值與最大值相差 d 以內，並最大化 S 的友情值總和。

************************
Specification
************************

::

    1 <= N <= 10^5
    1 <= d <= 10^9
    1 <= f[i], m[i] <= 10^9

************************
分析
************************

.. note:: 爬行法

將人按金錢值排序，由小到大。用爬行法在這個序列上找一個最大區間 [s, t], m[s] - m[t] <= d

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    struct P {
        ll m, f;
    };

    int N, D;
    vector<P> a;

    int main() {
        scanf("%d %d", &N, &D);
        for (int i = 0; i < N; i++) {
            ll f, m; scanf("%lld %lld", &f, &m);
            a.push_back((P) {f, m});
        }

        sort(a.begin(), a.end(), [](const P& p1, const P& p2) {
            return p1.m < p2.m;
        });

        int s = 0, t = 0;
        ll ans = -1;
        ll fsum = 0;
        for (;;) {
            while (t < N && a[t].m - a[s].m < D) {
                fsum += a[t++].f;
            }
            ans = max(ans, fsum);

            if (t == N) break;

            fsum -= a[s++].f;
        }

        printf("%lld\n", ans);

        return 0;
    }
