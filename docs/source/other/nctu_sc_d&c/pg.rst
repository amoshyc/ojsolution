#####################################
[cfother] B. Chat Order
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/637/problem/B>`_
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

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);
        cout.tie(0);

        int N;
        cin >> N;

        vector<string> A(N);
        for (int i = 0; i < N; i++)
            cin >> A[i];

        reverse(A.begin(), A.end());

        set<string> s;
        for (string name : A) {
            if (s.find(name) == s.end()) {
                cout << name << endl;
                s.insert(name);
            }
        }

        return 0;
    }
