########################################
[cf256] C. Painting Fence
########################################

.. sidebar:: Tags

    - ``tag_d&c``
    - ``tag_sparse_table``

.. contents:: TOC
    :depth: 2

*********************************************************
`題目 <http://codeforces.com/contest/448/problem/C>`_
*********************************************************

給定 N 塊連在一起的矩形柵欄，第 i 塊高度為 A[i]，寬度都為 1。
每一次粉刷可以直著刷也可以橫著刷，可以一直刷任意長度，但不能改變方向。
請問要把柵欄每一個部份都粉刷到（單面），最少要粉刷幾次？

************************
Specification
************************

::
    1 <= N <= 5000
    1 <= A[i] <= 10^9

************************
分析
************************

.. note:: D&C

終於有一題分治法的題目了。

設 solve(i, j) 回傳粉刷 A[i, j) 所需的最少粉刷次數，並且 A[i - 1], A[j] 這兩個位置要嘛是邊界，要嘛是高度 0。這個值為以下兩種情況的較小值：

第一種為全部直著刷，粉刷次數為 j - i。

第二種是底部橫著刷（高度 0 ~ 高度 min(A[i, j))），如果把粉刷過的部份刪掉::

    h = min(A[i, j))
    for x in range(i, j):
        A[x] -= h

剩下的部份被分割成一個或多個不連通的區塊，分別遞迴，並加總答案，這個總和再加上底部橫著刷的次數 h，即為這種情形的答案。

可以用反證法證這兩種情況會考慮到所有情況。整體的答案即為 solve(0, N)

-----------------------

把遞迴過程展開成一棵樹，可以發現上述做法最差情況樹高 N 層（給定的 A[i] 每個都不一樣），整體時間 O(N^2)，事實上是可以再優化的。

如果我們可以快速的挑一個 A[i, j) 最小值發生的位置，然後對兩邊遞迴，時間可以優化到 O(NlgN)。

設 solve(i, j, b) 回傳 A[i, j) 所需的最少粉刷次數，b 代表前一次橫向粉刷到的高度，並且 A[i - 1], A[j] 這兩個位置要嘛是邊界，要嘛是高度 <= b。當我們找到一個最小值發生的位置 m，兩邊遞迴 solve(i, m, A[m]), solve(m + 1, j, A[m])，一樣加總再加上橫向粉刷的次數（這次是 A[m] - b），最後別忘記檢查全部直著刷會不會更好。

找最小值所在的部份可以用 sparse table 預建好。這樣寫法的整體時間複雜度跟 merge sort 是一樣的，細節看程式碼吧。

************************
AC Code 1
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_N = 5000;
    const ll INF = 1e18;

    int N;
    ll A[MAX_N];

    ll solve(int l, int r) { // [l, r)
        ll vert = r - l;

        ll h = *min_element(A + l, A + r);
        for (int i = l; i < r; i++)
            A[i] -= h;

        ll hori = h;
        int s = l, t;
        for (;;) {
            while (s < r && A[s] == 0) s++;
            if (s >= r) break;
            t = s;
            while (t < r && A[t] != 0) t++;
            hori += solve(s, t);
            s = t;
        }

        return min(vert, hori);
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N;
        for (int i = 0; i < N; i++)
            cin >> A[i];

        cout << solve(0, N) << endl;

        return 0;
    }


************************
AC Code 2
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int MAX_LOG_N = 13;
    const int MAX_N = 5000;
    const ll INF = 1e18;

    int N;
    ll A[MAX_N];

    int sptb[MAX_LOG_N][MAX_N];

    void build() {
        for (int i = 0; i < MAX_LOG_N; i++)
            fill(sptb[i], sptb[i] + N, -1);
        for (int i = 0; i < N; i++)
            sptb[0][i] = i;
        for (int i = 1; (1 << i) <= N; i++) {
            for (int j = 0; j + (1 << i) <= N; j++) {
                ll a = sptb[i - 1][j];
                ll b = sptb[i - 1][j + (1 << (i - 1))];
                sptb[i][j] = ((A[a] < A[b]) ? a : b);
            }
        }
    }

    int query(int l, int r) { // [l, r)
        int k = floor(log2(r - l));
        int a = sptb[k][l];
        int b = sptb[k][r - (1 << k)];
        return ((A[a] < A[b]) ? a : b);
    }

    ll solve(int l, int r, int b = 0) { // [l, r), b: bottom
        if (r - l <= 0) return 0;
        int m = query(l, r);
        int h = A[m];
        ll vert = r - l;
        ll hori = (h - b) + solve(l, m, h) + solve(m + 1, r, h);
        return min(vert, hori);
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N;
        for (int i = 0; i < N; i++)
            cin >> A[i];

        build();
        cout << solve(0, N) << endl;

        return 0;
    }
