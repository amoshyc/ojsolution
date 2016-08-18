###################################################
[cfedu9] D. Longest Subsequence
###################################################

.. sidebar:: Tags

    - ``tag_enum``
    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/632/problem/D>`_
******************************************************

給定長度為 N 的序列 A，與一整數 M。
定義一個子序列的 l 值為子序列所有元素的 lcm。
請問 A 所有滿足 l 值 <= M 的子序列中，最長的那個子序列 S 是誰？
無解時，視 l 為 1 輸出。

************************
Specification
************************

::

    1 <= N, M <= 10^6
    1 <= A[i] <= 10^9

************************
分析
************************

.. note:: 枚舉 lcm

如果能快速地判斷在 lcm 為某個的子序列存不存在，就可以枚舉 lcm 來找答案。

如果我們已經先建表::

    cnt[x] = A 中有多少元素是 x 的因數 (x 是這些數的 lcm)

假是 cnt 陣列的最大值是 cnt[k]，那 lcm 就是 k，S 的長度就是 cnt[k]。

而這個表可以在 O(MlgM) 的時間建起來，方法如下：

    1. 統計 A 中各元素的值與出現次數：freq[i] = 數字 i 的出現次數
       freq 陣列最多只有 M + 1 項（0 ~ M），我們不需考慮大於 M 的數字。

    2. 使用 freq[i] 建出 cnt::

        for (int i = 1; i <= M; i++) {
            if (freq[i] > 0) {
                for (int j = i; j <= M; j += i) {
                    cnt[j] += freq[i];
                }
            }
        }

        第二個迴圈的時間複雜度為 M + M/2 + M/3 + ... + 1 = O(lgM)。

先建表，枚舉 lcm，找最佳解，整體時間為 O(MlgM + M) 那要如何還原 S 呢？
當我們知道 lcm 是多少時，我們只需依序看看 A 序列的每個元素是不是 lcm 的因數。
是的話，該元素就在 S 中。

小心測資::

    5 1
    1 1 1 1 1

與測資::

    1 1
    2

答案應為::

    1 5
    1 1 1 1 1

與::

    1 0
    (empty)


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    int main() {
        int N, M;
        scanf("%d %d", &N, &M);

        auto A = vector<int>(N, 0);
        // freq[i] = occurrence of i in A
        auto freq = vector<int>(M + 1, 0);
        // cnt[i] = number of items in A that is a factor of i
        auto cnt = vector<int>(M + 1, 0);

        for (int i = 0; i < N; i++) {
            scanf("%d", &A[i]);
        }

        for (int a : A) {
            if (a <= M) {
                freq[a]++;
            }
        }

        for (int i = 1; i <= M; i++) {
            if (freq[i] > 0) {
                for (int j = i; j <= M; j += i) {
                    cnt[j] += freq[i];
                }
            }
        }

        int lcm = -1;
        int len = 0;

        for (int i = 1; i <= M; i++) {
            if (cnt[i] > len) {
                len = cnt[i];
                lcm = i;
            }
        }

        if (lcm == -1) { // 無解
            puts("1 0");
            puts("");
        }
        else {
            printf("%d %d\n", lcm, len);
            for (int i = 0; i < N; i++) {
                if (lcm % A[i] == 0) {
                    printf("%d ", i + 1);
                }
            }
            puts("");
        }

        return 0;
    }
