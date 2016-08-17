###################################################
[cfedu9] A. Grandma Laura and Apples
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/632/problem/A>`_
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

    const int MAX_N = 40;
    int N;
    ll P;
    int data[MAX_N];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N >> P;
        for (int i = 0; i < N; i++) {
            string inp; cin >> inp;
            if (inp == "half")
                data[i] = 1;
            else
                data[i] = 2;
        }

        ll num = 0;
        for (int i = N - 1; i >= 0; i--) {
            if (data[i] == 2) num = num * 2 + 1;
            else num = num * 2;
        }

        ll money = 0;
        for (int i = 0; i < N; i++) {
            if (data[i] == 1) {
                money += num / 2 * P;
            }
            else {
                money += (num / 2) * P + P / 2;
            }
            num /= 2;
        }

        cout << money << endl;

        return 0;
    }
