#####################################
[cf354] C. Vasya and String
#####################################

.. sidebar:: Tags

    - ``tag_binary_search``
    - ``tag_two_pointers``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/676/problem/C>`_
******************************************************

字串只由 'a', 'b' 組成。
定義一個字串的 beauty 為該字串中最大的「連續相同字元的長度」。
一次轉換定義為將字串中的某個字元轉成另一個。
現給定一長度為 N 的字串，不使用超過 K 次轉換，請問可能達到的最大 beauty 是多少？

************************
Specification
************************

::

    1 <= N <= 10^5
    0 <= K <= N

************************
分析
************************

.. note:: 二分搜或爬行法

這種題目有點寫爛了…以下給出使用二分搜與使用爬行法的程式碼。

跟之前比較不一樣的是，題目沒說要轉 'a' 還是轉 'b'，而是問最長相同的。
那就 'a' 跑一次，'b' 跑一次，如果是用爬行法。
二分搜的解法也一樣，轉 'a' 看看，轉 'b' 看看。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int N, K;
    string s;

    int solve(char c) { // 回傳 max len of beautiful substring (all c)
        // two pointers
        // 當區間左端點固定時，右端點也是確定的。
        // 隨著左端點右移，右端點保證不會左移。
        // [st, ed)
        int st = 0, ed = 0;
        int cnt = 0, best = -1;
        for (;;) {
            while (ed < N && cnt + (s[ed] != c) <= K) {
                cnt += (s[ed] != c);
                ed++;
            }

            int len = ed - st;
            best = max(best, len);

            if (ed >= N)
                return best;

            cnt -= (s[st] != c);
            st++;
        }

        return -1;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N >> K >> s;
        cout << max(solve('a'), solve('b')) << endl;
        // solve('a');

        return 0;
    }

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    const int INF = 0x3f3f3f3f;

    int N, K;
    string s;

    // 可不可以只使用最多 K 次轉換，就產生長度為 len 的 beautiful substring (都為 c)
    // 即產生長度為 len 的 beautiful substring (都為 c) 所需要的轉換次數，是否 <= K
    bool C(int len, char c) {
        int ans = INF;
        int cnt = 0;

        // initial (st = 0)
        for (int i = 0; i < len; i++) {
            if (s[i] != c) {
                cnt++;
            }
        }
        ans = cnt;
        // window moves to [st, ed]
        for (int st = 1; st + len - 1 < N; st++) {
            int ed = st + len - 1;
            if (s[st - 1] != c) cnt--;
            if (s[ed] != c) cnt++;

            ans = min(ans, cnt);
        }

        return ans <= K;
    }

    int solve() {
        // binary search on the length of beautiful substring
        // 1 1 1 1 0 0 0
        int lb = 1, ub = N;
        // if (!C(lb, 'a') && !C(lb, 'b')) impossible;
        if (C(ub, 'a') || C(ub, 'b')) return N;

        while (ub - lb > 1) {
            int mid = (lb + ub) / 2;
            if (C(mid, 'a') || C(mid, 'b')) lb = mid;
            else ub = mid;
        }

        return lb;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N >> K >> s;
        cout << solve() << endl;

        return 0;
    }
