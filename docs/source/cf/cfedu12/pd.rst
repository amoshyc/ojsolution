###################################################
[cfedu12] D. Simple Subset
###################################################

.. sidebar:: Tags

    - ``tag_prime``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/665/problem/D>`_
******************************************************

定義一個序列 a[] 為 simple 若且唯若序列中任兩項相加皆為質數。
現給定一個序列 S[]，請輸出 S 的最長的 simple subsequence，

************************
Specification
************************

::

    1 <= |S| <= 1000
    1 <= S[i] <= 10^6

************************
分析
************************

.. note:: 奇偶

一個簡單的觀察，除 1 以外的數::

    二個偶數或二個奇數相加必不是質數

所以關於結果序列，可以得出以下幾個 case（序列由長到短）::

    1. 一個以上的 1，一個偶數 e，e + 1 是質數
    2. 二個以上的 1
    3. 一個奇數與一個偶數，其和是質數
    4. 一個數

其中 ``3`` 是 O(N^2)

實作時先建質數表，即可在 O(1) 時間判斷是不是質數。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    const ll MAX_NUM = 2 * 1000000;
    bool is_prime[MAX_NUM];
    void init_primes() {
        fill(is_prime, is_prime + MAX_NUM, true);
        is_prime[0] = is_prime[1] = false;
        for (ll i = 2; i < MAX_NUM; i++) {
            if (is_prime[i]) {
                for (ll j = i * i; j < MAX_NUM; j += i)
                    is_prime[j] = false;
            }
        }
    }

    void solve(vector<int>& v) {
        vector<int> odd, even;
        int cnt1 = 0;
        for (int i : v) {
            if (i & 1) odd.push_back(i);
            else even.push_back(i);

            if (i == 1) cnt1++;
        }

        // 2+ even number is impossible
        // 2+ odd number is impossible

        // Case 1: 1+, one even, sum is prime
        if (cnt1 >= 1) {
            for (int e : even) {
                if (is_prime[e + 1]) {
                    cout << cnt1 + 1 << "\n";
                    for (int i = 0; i < cnt1; i++)
                        cout << 1 << " ";
                    cout << e << "\n";
                    return;
                }
            }
        }

        // Case 2: all 1
        if (cnt1 >= 2) {
            cout << cnt1 << "\n";
            for (int i = 0; i < cnt1; i++)
                cout << 1 << " ";
            cout << "\n";
            return;
        }

        // Case 3: one odd, one even, sum is prime
        for (int o : odd) {
            for (int e : even) {
                if (is_prime[o + e]) {
                    cout << 2 << "\n";
                    cout << o << " " << e << " " << "\n";
                    return;
                }
            }
        }

        // Case 4: one number
        cout << 1 << "\n";
        cout << v[0] << "\n";
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        init_primes();

        int N; cin >> N;
        vector<int> data(N, 0);
        for (int i = 0; i < N; i++)
            cin >> data[i];

        solve(data);

        return 0;
    }
