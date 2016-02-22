########################################
[cfedu8] C. Bear and String Distance
########################################

.. sidebar:: Tags

    - ``tag_constructive_algorithm``
    - ``tag_greedy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/628/problem/C>`_
******************************************************

定義二個字串的 distance 為每一項字母的差的總和，例如 dist('abc', 'aae') =
dist('a', 'a') + dist('b', 'a'), dist('c', 'e') = 0 + 1 + 2 = 3。
現給定一長度為 n 的字串 s，與一整數 k，請輸出任一個與 s 的 distance 為 k 的字串。
如果解不存在請輸出 -1。

************************
Specification
************************

::

    1 <= n <= 10^5
    0 <= k <= 10^6


************************
分析
************************

.. note:: 建構式演算法，直接 greedy

針對 s[i]，盡量讓他發揮作用，即 dist(s[i], s'[i]) 越大越好，直到我們把 k 耗盡。
要讓 dist(s[i], s'[i]) 越大越好，直覺的可以想到，就讓 s[i] 變成 'a' 或 'z'，
看變成哪個的 distance 比較大就變哪個。

-1 發生在把 s 的每一項都極大化後，k 仍未被耗盡。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_n = 100000;
    int n, k;
    char input[max_n + 1];
    char ans[max_n + 1];

    int main() {
        scanf("%d %d", &n, &k);
        scanf("%s", input);

        for (int i = 0; i < n; i++) {
            if (k <= 0) {
                ans[i] = input[i];
                continue;
            }

            int diff1 = input[i] - 'a';
            int diff2 = 'z' - input[i];

            if (diff1 > diff2) {
                if (diff1 <= k) {
                    ans[i] = 'a';
                    k -= diff1;
                }
                else {
                    ans[i] = input[i] - k;
                    k = 0;
                }
            }
            else {
                if (diff2 <= k) {
                    ans[i] = 'z';
                    k -= diff2;
                }
                else {
                    ans[i] = input[i] + k;
                    k = 0;
                }
            }
        }

        if (k > 0) puts("-1");
        else printf("%s\n", ans);

        return 0;
    }
