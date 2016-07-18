#####################################
[cf131] B. Hometask
#####################################

.. sidebar:: Tags

    - ``tag_greedy``
    - ``tag_implementation``

.. contents:: TOC
    :depth: 2

**********************************************************
`題目 <http://codeforces.com/problemset/problem/214/B>`_
**********************************************************

************************
Specification
************************


************************
分析
************************

.. note:: Greedy

一個貪心的想法::

    大的 digit 放左邊

首先，要讓產生的數字 s 是 2 且是 5 的位數，那給定的序列裡一定要有 0，否則無解。
當我們用上述貪心想法構建出 s 後，如何讓 s 也是 3 的倍數，我們分段討論。

如果 s 的數字和（sum of digits）是 3n，那 s 就是解，輸出。
如果 s 的數字和（sum of digits）是 3n + 1，那我們得去除掉::

    1. 1 個除 3 餘 1 的 digit，來扺消多出來的那個 1，或
    2. 2 個除 3 餘 2 的 digit（因為 (3n+2) * 2 = 3n' + 1）

選項 1 較為優先，顯而易見的，去除的數越少，產生的數 s 會越大。
如果選項 1, 選項 2 都沒有，那就是無解。

如果 s 的數字和（sum of digits）是 3n + 1，同樣地去除::

    1. 1 個除 3 餘 2 的 digit，來扺消多出來的那個 1，或
    2. 2 個除 3 餘 1 的 digit（因為 (3n+1) * 2 = 3n' + 2）

除此之外無解。

實作不好寫，另外要記得 s 是 "00..000" 時，要改為輸出 0

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int N;
    int cnt[10];

    void output() {
        string res = "";
        for (int i = 9; i >= 1; i--) {
            res += string(cnt[i], char('0' + i));
        }

        if (res == "") res = "0";
        else {
            res += string(cnt[0], '0');
        }

        cout << res << "\n";
    }

    void solve() {
        if (cnt[0] == 0) {
            cout << "-1\n";
            return;
        }

        int sum = 0;
        for (int i = 1; i < 10; i++) {
            sum += cnt[i] * i;
        }

        if (sum % 3 == 1) {
            // remove 1 number which mod 3 is 1
            for (int i = 1; i < 10; i += 3) {
                if (cnt[i] > 0) {
                    cnt[i]--;
                    output();
                    return;
                }
            }

            // remove 2 number which mod 3 is 2
            int n = 2;
            for (int i = 2; i < 10; i += 3) {
                if (n > 0 && cnt[i] > 0) {
                    int val = min(cnt[i], n);
                    cnt[i] -= val;
                    n -= val;
                }
            }

            if (n > 0)
                cout << "-1\n";
            else
                output();
            return;
        }
        if (sum % 3 == 2) {
            // remove 1 number which mod 3 is 2
            for (int i = 2; i < 10; i += 3) {
                if (cnt[i] > 0) {
                    cnt[i]--;
                    output();
                    return;
                }
            }

            // remove 2 number which mod 3 is 1
            int n = 2;
            for (int i = 1; i < 10; i += 3) {
                if (n > 0 && cnt[i] > 0) {
                    int val = min(cnt[i], n);
                    cnt[i] -= val;
                    n -= val;
                }
            }

            if (n > 0)
                cout << "-1\n";
            else
                output();
            return;
        }

        output();
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N;
        for (int i = 0; i < N; i++) {
            int inp; cin >> inp;
            cnt[inp]++;
        }

        solve();

        return 0;
    }
