#####################################
[cf285] C. Building Permutation
#####################################

.. sidebar:: Tags

    - ``tag_sorting``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/285/problem/C>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        int N;
        scanf("%d", &N);

        vector<ll> v(N, 0);
        for (ll& i : v)
            scanf("%lld", &i);

        sort(v.begin(), v.end());

        ll ans = 0;
        for (int i = 0; i < N; i++) {
            ans += abs((i + 1) - v[i]);
        }

        printf("%lld\n", ans);

        return 0;
    }
