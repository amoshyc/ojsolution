#####################################
[cf343] C. Famil Door and Brackets
#####################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_enumeration``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/629/problem/C>`_
******************************************************

給定一個長度為 m，只包含 '(' 或 ')' 的字串 s。
一個字串是平衡的是指所有括號都有其對應的相異括號。

請問有幾種相異的字串 pair (p, q) 使得 p + s + q 是一個長度為 n，且是平衡的字串。
因為這個數字太大了，請輸出 mod (10^9 + 7) 後的結果。

************************
Specification
************************

::

    1 <= m <= n <= 10^5
    n - m <= 2000
    s[i], p[i], q[i] in ['(', ')']


************************
分析
************************

.. note:: 括號題，dp + 枚舉，有一定難度

先定義一個字串 s 的平衡值為將該字串的左括號視為 +1，右括號視為 -1，整個字串的總和。
我們將之寫作 b(s)。而所謂平衡的字串就是平衡值為 0 的字串

存在 dp::

    dp[i][j] = 長度為 i，平衡值為 j 的字串有多少個 (j >= 0)
    dp[0][0] = 1，即空字串 ""

考慮 dp[i][j] 的這些字串中最後一個字元，不是左括號就是右括號，所以::

    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j + 1]
                    （左括號）          （右括號）

加上邊際條件::

    dp[i][j] = dp[i - 1][j + 1] if j is 0
    dp[i][j] = dp[i - 1][j + 1] + dp[i - 1][j + 1] if j is not 0

前面假設了 j >= 0，即左括號比右括號多或等於的情況，那如果左括號比較少，即平衡值是負的的情況呢？
答案是::

    dp[i][k] (k < 0) = dp[i][-k]

將 dp[i][k] 的各個字串括號互換（左括號變右括號，右括號變左括號），即為 dp[i][-k] 的情況。

------------------------------------

T = p + s + q，題目要求 len(T) = n， b(T) = 0， 且 b(T[0, i)) >= 0。
既然 len(T), len(s), b(T), b(s) 都是定值，我們可以只需枚舉 len(p), b(p)，
每枚舉出一個 (len(p), b(p))，(len(q), b(q)) 即被確定下來。其中::

    len(q) = len(T) - len(p) - len(s) = n - m - len(p)
    b(q) = b(T) - b(p) - b(s) = 0 - b(p) - b(s)

這時答案就可以加上::

    dp[len(p)][b(p)] * dp[len(q)][b(q)]

個。

-------------------------------------

但在枚舉的過程中，我們得滿足 b(T[0, i)) >= 0 這個性質。
把不符合這性質的 (len(p), b(p)) 都跳過，這不是答案的一部分。
（構建出來的字串不符題意）

這可以在 O(1) 的時間被處理，預先找到 min_bs = min(b(s[0, i)))，
之後枚舉時只要 b(p) + min_bs < 0，就是不滿足 b(T[0, i)) >= 0 的情況。
（此時存在 x, 字串 b(p) + s[0:x) 的平衡值 < 0，即右括號多於左括號，這不合題意）
（s[0, x) 就是發生 min_bs 的 s 的 prefix）

另外還有一個不是答案的清況，當::

    b(p) + b(s) > n - m

很明顯的，當這發生時，不存在平衡值為 -(b(p) + b(s)) 的字串 q，因為 len(q) 最大就是 n - m，
最小的平衡值為 -(n - m)

-------------------------------------


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int NM = 2000;
    const ll MOD = 1e9 + 7;

    int N, M;
    string inp;
    ll dp[NM + 1][NM + 1];

    int main() {
        ios::sync_with_stdio(false); cin.tie(0);

        cin >> N >> M;
        cin >> inp;

        dp[0][0] = 1;
        for (int i = 1; i <= N - M; i++) {
            dp[i][0] = dp[i - 1][1];
            for (int j = 1; j <= N - M; j++) {
                dp[i][j] = (dp[i - 1][j - 1] + dp[i - 1][j + 1]) % MOD;
            }
        }

        ll bs = 0, min_bs = 1e15;
        for (int i = 0; i < M; i++) {
            bs += ((inp[i] == '(') ? +1 : -1);
            min_bs = min(min_bs, bs);
        }

        ll ans = 0;
        for (int lp = 0; lp <= N - M; lp++) {
            for (int bp = 0; bp <= lp; bp++) {
                if (bp + min_bs < 0) continue;
                if (bp + bs > N - M) continue;

                ll lq = N - M - lp;
                ll bq = 0 - bp - bs;
                ans = (ans + dp[lp][bp] * dp[lq][-bq]) % MOD;
            }
        }

        cout << ans << "\n";

        return 0;
    }
