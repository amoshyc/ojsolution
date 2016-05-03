###################################################
D. Counting Territory in Go
###################################################

.. sidebar:: Tags

    - ``tag_dfs``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23228>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

簡單 dfs。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <cstdio>
    #include <cstdlib>
    #include <cmath>
    #include <cstring>
    #include <ctime>
    #include <iostream>
    #include <algorithm>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map>
    #include <set>
    #include <deque>
    #include <list>
    #include <sstream>
    #include <iomanip>
    #include <functional> // greater<T>
    #include <numeric> // for accumulate

    int a[11][11];
    bool vis[11][11];

    int cnt;
    bool visb, visw;

    void dfs(int i, int j) {
        if (i < 0 || i >= 9) return;
        if (j < 0 || j >= 9) return;

        if (a[i][j] == 1) {
            visb = true;
            return;
        }
        if (a[i][j] == 2) {
            visw = true;
            return;
        }

        if (vis[i][j]) return;

        cnt++;
        vis[i][j] = true;
        dfs(i + 1, j);
        dfs(i - 1, j);
        dfs(i, j + 1);
        dfs(i, j - 1);
    }


    int main()
    {
        int n;
        scanf("%d", &n);
        while(n--)
        {

            int x = 0, y = 0,bb = 0, ww = 0;
            memset(vis, 0, sizeof(vis));

            for (int i = 0; i < 9; i++)
                for (int j = 0; j < 9; j++)
                    scanf(" %d", &a[i][j]);

            for (int i = 0; i < 9; i++)
            {
                for (int j = 0; j < 9; j++)
                {
                    if (a[i][j] == 1)
                        bb++;
                    if (a[i][j] == 2)
                        ww++;

                    if (!vis[i][j] && a[i][j] == 0) {
                        visb = visw = false;
                        cnt = 0;
                        dfs(i, j);

                        if (visb && visw) continue;
                        if (visb) x += cnt;
                        else y += cnt;
                    }
                }
            }

            printf("%d %d\n", x + bb, y + ww);

        }
        return 0;
    }
