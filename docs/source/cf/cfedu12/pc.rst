###################################################
[cfedu12] C. Simple Strings
###################################################

.. sidebar:: Tags

    - ``tag_greedy``
    - ``tag_easy``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/665/problem/C>`_
******************************************************

定義字串為 simple 若且唯若任兩鄰字元是不同的。
現給定一字串 s，請最小化置換字元的次數，請問最後置換後的字串。
有多重解，可輸出任一。

************************
Specification
************************

::

    1 <= |s| <= 2 * 10^5

************************
分析
************************

.. note:: 水題

從左至右，枚舉所有字元 s[i]，若 s[i] = s[i - 1]，
則將 s[i] 換成一個不同於 s[i - 1]，也不同於 s[i + 1] 的字元。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        string s;
        cin >> s;

        s += "$";

        for (size_t i = 1; i < s.length() - 1; i++) {
            if (s[i] == s[i - 1]) {
                if (s[i + 1] != 'z' && s[i - 1] != 'z') s[i] = 'z';
                else if (s[i + 1] != 'y' && s[i - 1] != 'y') s[i] = 'y';
                else s[i] = 'x';
            }
        }

        for (size_t i = 0; i < s.length() - 1; i++)
            cout << s[i];
        cout << "\n";

        return 0;
    }
