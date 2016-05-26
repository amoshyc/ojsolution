#####################################
[cf354] B. Pyramid of Glasses
#####################################

.. sidebar:: Tags

    - ``tag_dp``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/676/problem/B>`_
******************************************************

N 層的酒杯金字塔，每秒從頂端倒容量為一酒杯的香檳。
當一個酒杯已經滿了，流到它的香檳會往左往右流，各 1/2 的流量，流至下一層的酒杯中。
流動視為瞬間發生的事情，請問倒 T 秒後，有多少酒杯是滿的？

************************
Specification
************************

::

    1 <= N <= 10
    1 <= T <= 10^4

************************
分析
************************

.. note:: dp

流動是瞬間的，所以題目可以視為有 T 杯的香檳從頂端流下，問有多少酒杯會滿。

對於第 r 層，左邊數來的第 c 個酒杯，如果流到它的香檳量是 dp[r][c]，那根據題意::

    dp[r + 1][c + 0] += (dp[r][c] - 1) / 2
    dp[r + 1][c + 1] += (dp[r][c] - 1) / 2

初使條件::

    dp[i][j] = 0
    dp[0][0] = T

轉移方程式會出現 Fraction…，所以 dp 每一項都要用 Fractioni 存。
用 c++ 不好實作，可以改用 python，有內建的 Fraction。

之後枚舉所有酒杯，看有多少酒杯是滿的即可。

-------------------------------

所以如果要用 c++ 實作，可以只記錄分子。
分母可由公式推出。見 code


************************
AC Code 1
************************

.. code-block:: python
    :linenos:

    from fractions import Fraction

    inp = input().split()
    N, T = int(inp[0]), int(inp[1])

    if T is 0:
        print(0)
        exit()

    dp = []
    for r in range(N + 1):
        dp.append([Fraction(0) for _ in range(N + 1)])

    dp[0][0] = Fraction(T)
    for r in range(N):
        for c in range(r + 1):
            if dp[r][c] >= Fraction(1):
                dp[r + 1][c + 0] += (dp[r][c] - Fraction(1)) / Fraction(2);
                dp[r + 1][c + 1] += (dp[r][c] - Fraction(1)) / Fraction(2);

    cnt = 0
    for r in range(N):
        for c in range(r + 1):
            if dp[r][c] >= Fraction(1):
                cnt += 1

    print(cnt)

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N, T; cin >> N >> T;

        ll dp[N + 1][N + 1]; // store the numerators only
        memset(dp, 0, sizeof(dp));

        // see my previous python3 AC code for clearness
        dp[0][0] = T;
        for (int r = 0; r < N; r++) {
            for (int c = 0; c <= r; c++) {
                if (dp[r][c] >= (1 << r)) { // (1 << r) is denominator
                    dp[r + 1][c + 0] += (dp[r][c] - (1 << r));
                    dp[r + 1][c + 1] += (dp[r][c] - (1 << r));
                }
            }
        }

        int cnt = 0;
        for (int r = 0; r < N; r++) {
            for (int c = 0; c <= r; c++) {
                if (dp[r][c] >= (1 << r)) {
                    cnt++;
                }
            }
        }

        cout << cnt << endl;

        return 0;
    }
