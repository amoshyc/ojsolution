###################################################
[cfedu12] A. Buses Between Cities
###################################################

.. sidebar:: Tags

    - ``tag_implementation``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/665/problem/A>`_
******************************************************

A, B 之間只有一條路，其中有公車來往。從每天早上五點開始，最晚 23:59 發車。
每 fa 分鐘有一臺公車從 A 出發，經過 ta 時間到 B。
每 fb 分鐘有一臺公車從 B 出發，經過 tb 時間到 A。

請問某一班從 A 往 B 的公車在路上會與多少臺從 B 往 A 的公車交錯。
在起點與終點遇到的不算。

************************
Specification
************************

::

    1 <= fa, ta <= 120
    1 <= fb, tb <= 120


************************
分析
************************

.. note:: 分段討論

轉成以分鐘為單位後，枚舉從 B 往 A 的車次。

如果許多區間的題目一樣，我們對區間結尾進行討論，有以下三種情況::

    1. b_ed < a_st
    2. b_ed in [a_st, a_ed)
    3. b_ed >= a_ed

稍為想一下，即可知各種情況中有哪些車次是算的。細節看 code。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int fa, ta, fb, tb;
        int h, m;
        scanf("%d %d %d %d", &fa, &ta, &fb, &tb);
        scanf("%d:%d", &h, &m);

        int a_st = h * 60 + m;
        int a_ed = a_st + ta;

        int cnt = 0;
        for (int i = 0; ; i++) {
            int b_st = 5 * 60 + i * fb;
            int b_ed = b_st + tb;

            if (b_st >= 24 * 60) break;

            if (b_ed <= a_st) continue;
            if (b_ed <= a_ed) cnt++;
            else {
                if (b_st < a_ed)
                    cnt++;
            }
        }

        printf("%d\n", cnt);

        return 0;
    }
