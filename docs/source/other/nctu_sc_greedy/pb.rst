#####################################
[cf252] B. Valera and Fruits
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <http://codeforces.com/problemset/problem/441/B>`_
**********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: Greedy

一個貪心的想法::

    先採完昨天沒採完的水果，如果還有餘額，再採今天成熟的水果

注意要算到第 3001 天，第 3000 成熟的水果可以採到第 30001 天

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int D = 3000;
    int N, V;
    int cnt[D + 1]; // 1-based

    int main() {
        scanf("%d %d", &N, &V);
        for (int i = 0; i < N; i++) {
            int a, b; scanf("%d %d", &a, &b);
            cnt[a] += b;
        }

        int ans = 0;
        for (int i = 1; i <= D + 1; i++) {
            int c = 0;

            if (cnt[i - 1] > 0) {
                int n = min(cnt[i - 1], V);
                c += n;
                cnt[i - 1] -= n;
            }

            if (i <= D && c < V) {
                int n = min(cnt[i], V - c);
                c += n;
                cnt[i] -= n;
            }

            ans += c;
        }

        printf("%d\n", ans);

        return 0;
    }
