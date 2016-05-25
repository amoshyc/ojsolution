#####################################
[cf353] A. Infinite Sequence
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/675/problem/A>`_
******************************************************

給定一等差數列（首項為 a，差為 c）與與整數 b，請問 b 在不在數列中。

************************
Specification
************************

::

    -10^9 <= a, b, c <= 10^9

************************
分析
************************

.. note:: 水題

即問存不存在非負整數 k，使得::

    b = a + k * c

成立，可用 mod 與除法來判斷，注意 c 為零的特殊情況。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    int main() {
        ll a, b, c;
        cin >> a >> b >> c;

        if (c == 0) {
            cout << ((a == b) ? "YES" : "NO") << endl;
        }
        else {
            ll r = (b - a) % c;
            ll k = (b - a) / c;
            if (r == 0 && k >= 0) {
                cout << "YES\n";
            }
            else {
                cout << "NO\n";
            }
        }

        return 0;
    }
