##############################################
[cf354] E. The Last Fight Between Human and AI
##############################################

.. sidebar:: Tags

    - ``tag_game``
    - ``tag_math``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/676/problem/E>`_
******************************************************

有一個 degree 為 N 的多項式 P，係數為 a[] ，一開始所有係數都是未知的::

    P = a[n] * x^N + a[n - 1] * x^(N - 1) + ... + a[0]

人類與電腦輪流將未知的係數賦值，可以賦予任意值（有理數），遊戲一開始電腦先行。
如果所有係數都有值後，整個多項式是 x - K 的倍式，則人類贏了。要不然人類就輸了。

現在遊戲進從到某個階段，a[] 中已經有一些項是有值的，
給定這些值與非負整數 K，請問在雙方都採取最優策略下，人類會不會贏？

************************
Specification
************************

::

    1 <= N <= 10^5
    0 <= |K| <= 10^4
    0 <= a[i] <= 10^4 if a[i] 是已經有值的。

************************
分析
************************

.. note:: 分段討論，誰最後賦值誰就贏了

有兩個引理必需意識到::

    1. P 是 x - K 的倍式，若且唯若 P(K) = 0
    2. 在 K ≠ 0，有未知係數的情況下，誰最後賦值誰就贏了

前者顯而易見，這是高中數學。
後者也不難想，因為賦值可以賦予任何值，
那最後賦值的那個人，一定可以在最後未知的那項上賦予某個值，
使目前多項式的值加上該項的值後，變為 0。

我們可以將遊戲分為幾種狀態來討論::

    1. K = 0
        此時 P 是否為零只跟 a[0] 有關了。（引理 1，把 0 代入 P 即可知）。
        a) 如果 a[0] 是已知的，那答案就看 a[0] 是否為 0。
        b) 如果 a[0] 是未知的，那現在輪到誰，誰就贏了，因為雙方都採最優策略。
            輪到電腦，它只要把 a[0] 設為非 0 的數，那人類之後不管怎麼賦值，P 都不可能為 0。
            輪到人數，把 a[0] 設為 0，那之後電腦怎麼賦值，P 都會是 0。

    2. K ≠ 0
        a) 沒有未知數，那就看 P(K) 是否為 0（引理 1）
        b) 有未知數，那誰最後賦值誰就贏了（引理 2）

關於 (2a） 的計算，因為 P(K) 值太大了，用 double 存一定會有誤差，有二種解決方式::

    1. 用 long long，計算時 mod 某個數 M。
       因為 P(K) = 0 iff P(K) mod M = 0，
       如果 M 枚舉的夠多，則所有的 P(K) mod M 都為 0，可以大致保證 P(K) = 0
       （實作上，最好選一些大質數，但因為是 cf，選固定值很容易被 hacked，所以用隨機數較好）

    2. 用 long long，計算時只要值超出 1e9，那就是錯的

``(2)`` 是因為以下原因，計算多項式，我們通常是用 horner's method 寫成

.. code-block:: cpp
    :linenos:

    ll s = 0;
    for (int i = N; i >= 0; i--) {
        s = s * K + a[i];
    }

過程中，s 的值是::

    0.   a[N]
    1.   a[N] * K + a[N - 1]
    2.   a[N] * K^2 + a[N - 1] * K + a[N - 2]
    ...
    N-1. a[N] * K*(N-1) + a[N-1] * K^(N-2) + ... + a[1]
    N.   a[N] * K^N + a[N - 1] * K^(N-1) + ... + a[0] = P(K)

若 ``式子 N`` == 0，則::

    a[N] * K^N + a[N - 1] * K^(N-1) + ... + a[1] * K = -a[0]

因為題目有說::

    abs( a[i] ) <= 10^4

所以可寫成不等式::

    abs( a[N] * K^N + a[N - 1] * K^(N-1) + ... + a[1] * K ) <= 10^4

同除 abs(K)::

    abs( a[N] * K*(N-1) + a[N-1] * K^(N-2) + ... + a[1] ) <= 10^4 / abs( K )

右式，最大為 K = 1 時（K 為整數，且在我們現在討論的這個遊戲狀態，K ≠ 0），所以::

    abs( a[N] * K*(N-1) + a[N-1] * K^(N-2) + ... + a[1] ) <= 10^4
    即
    abs( 式子 (N-1) ) <= 10^4

再重覆上述過程，移項（接下來是 a[1]），同除 K，可得::

    abs( 式子 (N-2) ) <= 10^4 * 2
    abs( 式子 (N-3) ) <= 10^4 * 3
    ...
    abs( 式子 0 ) <= 10^4 * N

N 最大是 10^5，所以::

    abs( 式子 0 ) <= 10^9

其它式子也有這個性質::

    abs( 式子 i ) <= 10^9

根據 (P -> Q) ≡ (¬Q -> ¬P)，所以::

    if abs( 式子 i ) > 10^9
    then (式子 N) ≠ 0

有了這個限制，就保證了計算時 s 不會超出 long long 的範圍。
因為，在計算 P(K) 時，只要 abs(s 的值) > 10^9，那 P(K) 就不可能為 0。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const int INF = 0x3f3f3f3f;
    const int H = 0; // Human
    const int C = 1; // Computer

    const int MAX_N = 100000;
    int N, K;
    int a[MAX_N + 1];

    bool iszero() {
        ll s = 0;
        for (int i = N; i >= 0; i--) {
            s = s * K + a[i];

            if (abs(s) > ll(1e9))
                return false;
        }
        return (s == 0);
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        cin >> N >> K;

        int this_turn = -1;
        int last_turn = -1;
        int unkown_cnt = 0;
        int known_cnt = 0;

        for (int i = 0; i <= N; i++) {
            string inp; cin >> inp;
            if (inp[0] == '?') a[i] = INF;
            else {
                known_cnt++;
                a[i] = stoi(inp);
            }
        }

        unkown_cnt = N + 1 - known_cnt;
        this_turn = (((known_cnt + 1) % 2 == 1) ? C : H);
        last_turn = (((N + 1) % 2 == 1) ? C : H);

        int win = -1;
        if (K == 0) {
            if (a[0] != INF)
                win = ((a[0] == 0) ? H : C);
            else
                win = ((this_turn == H) ? H : C);
        }
        else {
            if (unkown_cnt == 0)
                win = ((iszero()) ? H : C);
            else
                win = ((last_turn == H) ? H : C);
        }

        cout << ((win == H) ? "YES" : "NO") << endl;

        return 0;
    }
