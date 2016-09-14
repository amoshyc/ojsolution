#####################################
[cf371] A. Meeting of Old Friends
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/714/problem/A>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

可以用求兩個矩形重疊面積的手法做，也可以分情況討論（五種情況）

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    bool inside(ll a, ll b, ll c) {
        return (b <= a && a <= c);
    }

    int main() {
        ll l1, r1, l2, r2, k;
        cin >> l1 >> r1 >> l2 >> r2 >> k;

        // [l1, r1], [l2, r2]

        ll l = max(l1, l2);
        ll r = min(r1, r2);

        ll overlap = r - l + 1;
        if (l <= k && k <= r)
            overlap--;

        cout << max(overlap, 0ll) << endl;

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

    bool inside(ll a, ll b, ll c) {
        return (b <= a && a <= c);
    }

    int main() {
        ll l1, r1, l2, r2, k;
        cin >> l1 >> r1 >> l2 >> r2 >> k;

        // [l1, r1], [l2, r2]

        if (inside(l2, l1, r1) && inside(r2, l1, r1)) {
            cout << (r2 - l2 + 1 - inside(k, l2, r2)) << endl;
            return 0;
        }

        if (inside(l1, l2, r2) && inside(r1, l2, r2)) {
            cout << (r1 - l1 + 1 - inside(k, l1, r1)) << endl;
            return 0;
        }

        if (r2 < l1 || l2 > r1) {
            cout << 0 << endl;
            return 0;
        }

        if (inside(l2, l1, r1) && !inside(r2, l1, r1)) {
            cout << (r1 - l2 + 1 - inside(k, l2, r1)) << endl;
            return 0;
        }

        cout << (r2 - l1 + 1 - inside(k, l1, r2)) << endl;

        return 0;
    }
