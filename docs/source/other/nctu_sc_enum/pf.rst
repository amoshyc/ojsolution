#####################################
[cf102] A. Ksusha and Array
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


**********************************************************
`題目 <http://codeforces.com/problemset/problem/299/A>`_
**********************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 枚舉

水題，求出所有數的最大公因數，最後看此數是不是陣列的某一項。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int gcd(int a, int b) {
        while (b) {
            int temp = a % b;
            a = b;
            b = temp;
        }
        return a;
    }

    int main() {
        int N;
        cin >> N;
        vector<int> v(N, 0);
        for (int i = 0; i < N; i++)
            cin >> v[i];

        int g = v[0];
        for (int i = 1; i < N; i++)
            g = gcd(g, v[i]);

        if (find(v.begin(), v.end(), g) == v.end())
            cout << "-1" << endl;
        else
            cout << g << endl;

        return 0;
    }
