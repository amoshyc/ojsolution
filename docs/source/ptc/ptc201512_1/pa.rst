###################################################
A. Constrained 0-1 Knapsack Problem
###################################################

.. sidebar:: Tags

    - ``tag_dfs``

.. contents:: TOC
    :depth: 2


****************************************************************************
`題目 <http://140.116.249.152/e-Tutor/mod/programming/view.php?id=26865>`_
****************************************************************************

給定 N 個物品的價值 v[i] 與重量 w[i], 與正整數 Wa, Wb, L，在滿足 Wa <= 總重 <= Wb 的條件下，選至少 L 個物品出來，
並使單位重量的價值 ceil(sum(w) / sum(v) 最大化。請輸出該值。

************************
Specification
************************

::

    1 <= N <= 20
    0 <= v[i]
    1 <= p[i]
    0 <= sum(v[i]) <= 10^9
    n <= sum(p[i]) <= 10^9


************************
分析
************************

.. note:: 因限制造成不能使用二分搜，改用 dfs

乍看之下很像可以用二分搜解的平均最大化的問題，但仔細分析可以發現，
新增加的條件會造成二分搜解法中的單調性消失，所以不能使用二分搜來解這題。

二分搜不能解，那要怎麼辦呢？重新看一次題目可以發現 N 竟然 <= 20，
這意味著我們可以 **暴搜** ，八筆測資時間也才 8 * O(2^N) <= 10*8，時限肯定足夠的。

實作就 DFS，寫法很多種…但記得題目指的平均是取 ceil 的。


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <cstdio>
    #include <algorithm>

    using namespace std;

    struct Item {
        int w, v;
    };

    int N, L;
    int Wa, Wb;
    Item items[20];

    int max_avg_value = -1;

    // first i+1 items
    void dfs(int i, int total_v, int total_w, int cnt) {
        if (total_w > Wb) return;

        if (i == N-1) {
            if (cnt >= L && Wa <= total_w && total_w <= Wb) {
                int avg = total_v / total_w + ((total_v % total_w == 0) ? 0 : 1);
                max_avg_value = max(max_avg_value, avg);
            }
            return;
        }

        dfs(i+1, total_v + items[i+1].v, total_w + items[i+1].w, cnt + 1);
        dfs(i+1, total_v, total_w, cnt);
    }

    int main() {
        while (scanf("%d %d %d %d", &N, &L, &Wa, &Wb)) {
            for (int i = 0; i < N; i++)
                scanf("%d %d", &items[i].v, &items[i].w);

            dfs(0, items[0].v, items[0].w, 1);
            dfs(0, 0, 0, 0);

            printf("%d\n", max_avg_value);

            int flag; scanf("%d", &flag);
            if (flag == -1) break;
            else max_avg_value = -1;
        }

        return 0;
    }
