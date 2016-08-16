############################################
[cfedu6] D. Professor GukiZ and Two Arrays
############################################

.. sidebar:: Tags

    - ``tag_enum``
    - ``tag_half_enum``
    - ``tag_binary_search``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/620/problem/D>`_
******************************************************

給定一個長度為 N 的序列 A，與一長度為 M 的序列 B。
定義一次交換為將 A 中的某一項與 B 中的某一項交換。
定義兩序列的和的差值為 v，即 ``|sum(A) - sum(B)|``

請問在最多交換二次的情況下，v 值的最小可能是多少？
同時，你必需輸出你是交換了哪些項。

************************
Specification
************************

::

    1 <= N, M <= 2000
    -10^9 <= A[i], B[j] <= 10^9

************************
分析
************************

.. note:: 枚舉交換次數

枚舉交換次數，看各個交換次數哪個最好。

零次時，v = sum(A) - sum(B)。

一次時，枚舉 A 中的每一項，並枚舉 B 中的每一項，看交換後的結果怎樣最好

::

    sumA = sum(A)
    sumB = sum(B)
    v = INF
    for a in A:
        for b in B:
            new_sumA = sumA - a + b
            new_sumB = sumB - b + a
            v = min(v, abs(new_sumA - new_sumB))

二次時，使用折半列舉的技巧，首先，先建表::

    Let AA = [sumB + 2 * (A[i] + A[j]) for i in [0, N) for j in [i + 1, N)]
        BB = [sumA + 2 * (B[i] + B[j]) for i in [0, M) for j in [i + 1, M)]

    sort(BB)

之後枚舉 AA 中的每一項 AA[i]，看 BB 中最 **接近** AA[i] 的是多少，交換看看。
這麼做是正確的是因為新的總和變成::

    假設是選到 AA[a], BB[b] 這兩項
    new_sumA = sumA - AA[a] + BB[b]
    new_sumB = sumB - BB[b] + AA[a]

    v = sumA - 2 * AA[a] - sumB + 2 * BB[b]
      = (sumA + 2 * BB[b]) - (sumB + 2 * AA[a]) -> 0

而在實作中，最接近（差值最小）這次事情可以透過 lower_bound 並枚舉較大與較小的那兩項來達成。

下邊給出的程式碼，我 AA, BB 剛好反過來了…
程式碼中是枚舉 BB ，看 AA 中的最接近項，不好意思~
題解太晚打了，都忘了那時 code 是怎麼寫的，重想了一遍…結果不太一樣，又懶得重寫，就將就吧。

這題我覺得出得不錯~

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    typedef pair<int, int> pii;

    const int max_n = 2000;
    const int max_m = 2000;
    int n, m;
    ll a[max_n];
    ll b[max_m];

    ll ans;
    pii swap1 = pii(-1, -1);
    pii swap2 = pii(-1, -1);

    ll Sa;
    ll Sb;
    map<ll, pii> mm;

    void update(int i, int j, int k, int l) {
        ll nSa = Sa - a[i] - a[j] + b[k] + b[l];
        ll nSb = Sb - b[k] - b[l] + a[i] + a[j];
        ll new_ans = abs(nSa - nSb);

        if (new_ans < ans) {
            ans = new_ans;
            swap1 = pii(i, k);
            swap2 = pii(j, l);
        }
    }

    void solve() {
        Sa = accumulate(a, a + n, 0ll);
        Sb = accumulate(b, b + m, 0ll);

        // 0 swap
        ans = abs(Sa - Sb);

        // 1 swap
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                ll nSa = Sa - a[i] + b[j];
                ll nSb = Sb - b[j] + a[i];
                ll new_ans = abs(nSa - nSb);
                if (new_ans < ans) {
                    ans = new_ans;
                    swap1 = pii(i, j);
                }
            }
        }

        // 2 swaps
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                mm[Sb + 2 * (a[i] + a[j])] = pii(i, j);
            }
        }

        for (int k = 0; k < m - 1; k++) {
            for (int l = k + 1; l < m; l++) {
                ll val = Sa + 2 * (b[k] + b[l]);
                auto it = mm.lower_bound(val);

                if (it != mm.end())
                    update((it->second).first, (it->second).second, k, l);
                if (it != mm.begin()) {
                    it--;
                    update((it->second).first, (it->second).second, k, l);
                }
            }
        }
    }

    int main() {
        scanf("%d", &n);
        for (int i = 0; i < n; i++)
            scanf("%lld", &a[i]);
        scanf("%d", &m);
        for (int i = 0; i < m; i++)
            scanf("%lld", &b[i]);

        solve();

        printf("%lld\n", ans);
        // printf("%I64d\n", ans);
        if (swap1.first == -1) {
            puts("0");
        }
        else if (swap2.first == -1) {
            puts("1");
            printf("%d %d\n", swap1.first + 1, swap1.second + 1);
        }
        else {
            puts("2");
            printf("%d %d\n", swap1.first + 1, swap1.second + 1);
            printf("%d %d\n", swap2.first + 1, swap2.second + 1);
        }

        return 0;
    }
