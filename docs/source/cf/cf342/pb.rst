#####################################
[cf342] B. War of the Corporations
#####################################

.. sidebar:: Tags

    - ``tag_kmp``
    - ``tag_constructive_algorithm``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/625/problem/B>`_
******************************************************

給定字串 S, P，求 P 在 S 中的 non-overlapping occurrence

************************
Specification
************************

::

    1 <= len(S) <= 10^5
    1 <= len(P) <= 30


************************
分析
************************

.. note:: 水題

要計算 non-overlapping occurrence，一個簡單的想法，當找到一個匹配 S[m, m + len(P)) 時，
將最後一個字元換成特殊符號，下次搜尋時，從 S[m + len(P)] 開始。

以下給出用 KMP 的版本，雖然說這題用 O(len(S) * len(P)) 的原始方法也可以過。

另外官方題解中有提到，python 有內建這個功能了::

    print(s.count(p))

就會輸出 non-overlapping occurrence 了…



************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int KMP(const string P, string S) {
        int cnt = 0;
        const int Np = P.length();
        const int Ns = S.length();

        // failure function
        vector<int> F(Np, 0);
        int i = 0, j = -1; F[0] = -1;
        while (i < Np) {
            while (j >= 0 && P[i] != P[j]) j = F[j];
            i++; j++;
            F[i] = j;
        }

        i = 0, j = 0;
        while (i < Ns) {
            while (j >= 0 && S[i] != P[j]) j = F[j];
            i++; j++;
            if (j == Np) {
                cnt++;
                j = F[j];

                S[i - 1] = '#';
                i--;
            }
        }

        return cnt;
    }

    int main() {
        string s, p;
        cin >> s >> p;
        cout << KMP(p, s) << "\n";

        return 0;
    }
