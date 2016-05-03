###################################################
A. The Shortest Path Problem on Hypercubes
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23186>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

考 hypercube，還記得的話這題很簡單，就是 xor

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>

    using namespace std;

    typedef long long ll;

    int main() {
        ios::sync_with_stdio(false);

        int T;
        cin >> T;

        while (T--) {
            ll n, s, t;
            cin >> n >> s >> t;

            ll r = s ^ t;
            //cout << "r: " << r << endl;
            int cnt = 0;
            while (r != 0) {
                cnt = cnt + (r & 1);
                r = r >> 1;
            }

            cout << cnt << "\n";
        }

        return 0;
    }
