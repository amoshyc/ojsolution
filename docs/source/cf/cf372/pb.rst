#####################################
[cf372] B. Complete the Word
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/716/problem/B>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

窗口平移。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    string inp;
    int cnt[26];
    int ava = 0; // cnt of '?'

    bool ok() {
        int need = 0;
        for (int i = 0; i < 26; i++)
            if (cnt[i] == 0)
                need++;
        return ava >= need;
    }

    int solve() {
        if (inp.length() < 26)
            return -1;

        for (int i = 0; i < 26; i++) {
            if (inp[i] != '?')
                cnt[inp[i] - 'A']++;
            else
                ava++;
        }

        if (ok())
            return 0;

        for (int s = 1; s + 26 - 1 < int(inp.length()); s++) {
            if (inp[s - 1] != '?')
                cnt[inp[s - 1] - 'A']--;
            else
                ava--;

            if (inp[s + 26 - 1] != '?')
                cnt[inp[s + 26 - 1] - 'A']++;
            else
                ava++;

            if (ok())
                return s;
        }

        return -1;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> inp;

        int s = solve();
        if (s == -1) cout << "-1" << endl;
        else {
            // generate
            vector<char> need;
            for (int i = 0; i < 26; i++)
                if (cnt[i] == 0)
                    need.push_back('A' + i);
            for (int i = s; !need.empty(); i++) {
                if (inp[i] == '?') {
                    inp[i] = need.back();
                    need.pop_back();
                }
            }
            for (char &c : inp) {
                if (c == '?')
                    c = 'A';
            }

            cout << inp << endl;
        }

        return 0;
    }
