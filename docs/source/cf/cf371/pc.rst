#####################################
[cf371] C. Sonya and Queries
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/714/problem/C>`_
******************************************************

假設你有一個 multiset S，請寫一個程式支援 T 個操作，操作的種類為
1. ``+ x``: 將 x 加入 S
2. ``- x``: 將 x 從 S 移除（保證 S 中有 x）
3. ``? p``: 請輸出 S 中符合 pattern p 的元素有幾個？
p 為是一個 0, 1 組出的字串，0 代表偶數，1 代表奇數。
p 與一個數 x 比對時，是右對齊，如果 p 與 x 的長度不一樣，在較短的那個的左方補 0。
若 x 在十進位下的 digits 滿足 pattern p，則稱 x 符合 p。

************************
Specification
************************

::

    1 <= T <= 10^5
    1 <= x <= 2^18
    1 <= |p| <= 2^18

************************
分析
************************

.. note:: 水題

有兩個方法可以解決這個問題:
1. 將給定的 x, p 都先補滿到 18 位。然後視為整數，用陣列存次數。
2. 針對每一種長度都建一個 map 存各個數字 x 的次數。詢問 p 時，手動對齊。
3. 將給定的 x, p 反向存向 trie。

比賽時，我是想到第二種。
細節看 code 吧。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int cnt[1 << 19];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int T;
        cin >> T;

        while (T--) {
            string cmd, s;
            cin >> cmd >> s;

            for (char& c : s) {
                c = (c - '0') % 2 + '0';
            }

            int id = bitset<18>(s).to_ulong();

            if (cmd[0] == '+') {
                cnt[id]++;
            }
            if (cmd[0] == '-') {
                cnt[id]--;
            }
            if (cmd[0] == '?') {
                printf("%d\n", cnt[id]);
            }
        }

        return 0;
    }

************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    unordered_map<string, int> data[19];

    bool iszero(char c) {
        return c == '0';
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int Q;
        cin >> Q;
        while (Q--) {
            string cmd, s;
            cin >> cmd >> s;

            if (cmd[0] == '+') {
                for (char &c : s) {
                    c = int((c - '0') % 2) + '0';
                }

                data[s.length()][s]++;
            }
            if (cmd[0] == '-') {
                for (char &c : s) {
                    c = int((c - '0') % 2) + '0';
                }

                data[s.length()][s]--;
            }
            if (cmd[0] == '?') {
                int res = 0;
                int len = s.length();

                for (int i = 1; i <= len; i++) {
                    if (all_of(s.begin(), s.begin() + len - i, iszero)) {
                        string ss = s.substr(len - i, '0');
                        res += data[i][ss];
                    }
                }

                for (int i = len + 1; i <= 18; i++) {
                    string ss = string(i - len, '0') + s;
                    res += data[i][ss];
                }

                cout << res << endl;
            }
        }
        return 0;
    }
