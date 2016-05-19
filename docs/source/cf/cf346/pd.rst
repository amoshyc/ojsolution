#####################################
[cf346] D. Bicycle Race
#####################################

.. sidebar:: Tags

    - ``tag_geometry``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/659/problem/D>`_
******************************************************

在平面格子上，Maria 順時針繞著湖走。給定所有的轉彎點 (x[i], y[i])，共有 N 個。
請問有幾個轉彎點如果沒轉彎成功會掉進湖裡？

************************
Specification
************************

::

    4 <= N <= 10^4
    -10^4 <= x[i], y[i] <= 10^4

************************
分析
************************

.. note:: Greedy

沒轉成功就掉進湖裡只發生在::

    1. →↑
    2. ←↓
    3. ↑←
    4. ↓→

直接線性時間掃過所有相鄰的三個轉彎點即可。細節看 code。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int RIGHT = 0;
    int DOWN = 1;
    int LEFT = 2;
    int UP = 3;

    int getdir(int x1, int y1, int x2, int y2) {
        if (x1 < x2) return RIGHT;
        if (x1 > x2) return LEFT;
        if (y1 < y2) return UP;
        if (y1 > y2) return DOWN;
        return -1;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N;
        cin >> N;
        N++;
        vector<int> x(N, 0);
        vector<int> y(N, 0);
        for (int i = 0; i < N; i++) {
            cin >> x[i];
            cin >> y[i];
        }

        int cnt = 0;
        for (int i = 1; i < N - 1; i++) {
            int dir1 = getdir(x[i-1], y[i-1], x[i], y[i]);
            int dir2 = getdir(x[i], y[i], x[i+1], y[i+1]);

            if (dir1 == RIGHT && dir2 == UP)
                cnt++;
            if (dir1 == LEFT && dir2 == DOWN)
                cnt++;
            if (dir1 == UP && dir2 == LEFT)
                cnt++;
            if (dir1 == DOWN && dir2 == RIGHT)
                cnt++;
        }

        cout << cnt << "\n";

        return 0;
    }
