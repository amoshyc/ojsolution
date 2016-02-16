#####################################
[cf342] A. Guest From the Past
#####################################

.. sidebar:: Tags

    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/625/problem/A>`_
******************************************************

有二種都是一公升的瓶子，塑膠瓶每瓶 a 元；
玻璃瓶每瓶 b 元，並且喝完後，返還玻璃瓶可得 c 元，
現有 n 元，求最多能喝多升公升的水。

************************
Specification
************************

::

    1 <= n <= 10^18
    1 <= a <= 10^18
    1 <= c < b <= 10^18


************************
分析
************************

.. note:: 把不等式列出來比較好想

從範例可以得知，難點是在處理買塑膠瓶的情況。於是我們分二種情況討論

#. n < b: 不用說，全部買塑膠瓶。

#. n >= b: 可以買塑膠瓶也可以買玻璃瓶，不知道哪個比較好，再分段討論。

    #. 什麼時候會發生買塑膠瓶比較好呢？答案為當 a < (b - c) 時，
       這時買塑膠瓶比較划算，而買剩下的錢也不夠買玻璃瓶，因 a < (b - c) < b，
       所以直接輸出 floor(n / a)

    #. 那當 a >= (b - c) 呢？當然就買玻璃瓶，從不等式 ::

           n >= cnt * b - (cnt - 1) * c
           cnt 為買到的玻璃瓶的數量。
           最後一次購買，返還的 c 元無法再買下次的玻璃瓶，所以是 (cnt - 1) * c

           移項得
           n - c >= cnt * (b - c)

       可以得知，cnt 的最大值為 floor((n - c) / (b - c))，而剩下的錢，
       既然買不了玻璃瓶，當然就全部去買塑膠瓶啦，所以數量再加上 floor(rest / a)

整體時間複雜度為 O(1)

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    ll money, a, b, c;
    ll cnt = 0;

    int main() {
        cin >> money >> a >> b >> c;

        if (money < b) {
            cout << money / a << "\n";
        }
        else { // money >= b
            if (a < b - c) cout << money / a << "\n";
            else {
                ll cnt = (money - c) / (b - c);
                ll rest = money - cnt * (b - c);
                cnt += rest / a;

                cout << cnt << "\n";
            }
        }

        return 0;
    }
