#####################################
[cfother] B. Replacing Digits
#####################################

.. sidebar:: Tags

    - ``tag_greedy``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <http://codeforces.com/problemset/problem/169/B>`_
**********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: Greedy

一個貪心的想法::

    從高位開始，看看每個 digit 能不能換成更大的數

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int cnt[10];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        string a, b;
        cin >> a >> b;

        for (char c : b) {
            cnt[c - '0']++;
        }

        for (char& c : a) {
            for (int i = 9; i > int(c - '0'); i--) {
                if (cnt[i] > 0) {
                    c = char('0' + i);
                    cnt[i]--;
                    break;
                }
            }
        }

        cout << a << "\n";

        return 0;
    }
