###################################################
[cfedu9] B. Alice, Bob, Two Teams
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/632/problem/B>`_
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

    const int MAX_N = 500000;
    int N;
    ll s[MAX_N];
    string flag;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N;
        for (int i = 0; i < N; i++)
            cin >> s[i];
        cin >> flag;

        ll ans = 0; // flipping nothing
        for (int i = 0; i < N; i++)
            if (flag[i] == 'B')
                ans += s[i];

        ll val1 = ans;
        ll val2 = ans;

        // pref
        for (int i = 0; i < N; i++) {
            if (flag[i] == 'A') { // flipped to B
                val1 += s[i];
            }
            else { // B been flipped to A
                val1 -= s[i];
            }

            ans = max(ans, val1);
        }

        // suff
        for (int i = N - 1; i >= 0; i--) {
            if (flag[i] == 'A') {
                val2 += s[i];
            }
            else {
                val2 -= s[i];
            }

            ans = max(ans, val2);
        }

        cout << ans << endl;


        return 0;
    }
