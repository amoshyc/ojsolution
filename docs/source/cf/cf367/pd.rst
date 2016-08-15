#####################################
[cf367] D. Vasiliy's Multiset
#####################################

.. sidebar:: Tags

    - ``tag_trie``
    - ``tag_binary_search``
    - ``tag_multiset``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/706/problem/D>`_
******************************************************

假設你有一個 multiset A，一開始裡面只要一個元素 0。
請寫一個程式支援 Q 個操作，操作有三種::

    1. + x: 將 x 加入 A
    2. - x: 將 x 從 A 中移除
    3. ? x: 請問 max([x xor y] for y in A) 是多少？

************************
Specification
************************

::

    1 <= Q <= 200000

************************
分析
************************

.. note:: trie 的經典題

事實上這是 trie 的經典題，不過競賽時我沒想到（不太會寫 trie...）。
想到一個二分搜的做法，想法對了，可惜沒實作出來，賽後才 AC。

--------------------------

首先，我們可以用 map + set 來模擬 multiset A，至於為什麼不直接用 multiset 呢？
因為 c++ 的 multiset 不支援題目所要求的那種刪除。

當詢問 max([x xor y] for y in A) 時，
一個簡單的想法就是在 A 中找最 **接近** ~x （x 的 1's complement）的數，
接近的意思是從高位開始比，舉個例子：

如果 ~x = 0b10011，那 0b11100 會比 0b01100 更接近 ~x，寫成函式就是

.. code-block:: python

    def cmp(a, b, x):
        '''
        return a if a is closer to x else b
        '''
        for i in range(30, -1, -1):
            if (a & (1 << i)) != (b & (1 << i)):
                if ((~x) & (1 << i)): # this bit is 1
                    # return the one having 1 at this bit
                    return a if (a & (1 << i)) else b
                else:
                    return b if (b & (1 << i)) else a
        return a

很明顯示，這個數跟 x 做 xor 後的值會最大。那要怎麼找呢？

::

    1. 假設 ~x = 0b10101，可以利用 lower_bound 在 A 中找存不存在 0b1xxxx。
    2a. 如果找得到，那就看下一個 1 存不存在，即找存不存在 0b101xx
    2b. 如果找不到，那就改成找 0b0xxxx

不斷重覆上述過程，直到所有 1 都確認過為止，
最後找到的那個答案就是最接近 ~x 的數

如果看不懂，直接看 code 吧，還看不懂自己拿個紙筆推一下~

------------------------------------

這題另一個解法是用 trie，這也是 trie 的經典題，關於 trie 的應用可以參考：
`<https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems>`_
然後試了幾種 trie 的寫法，發現用靜態存，不遞迴的寫法最好寫，直接看 code~

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef unsigned int ui;

    int main() {
        int Q;
        scanf("%d", &Q);

        map<ui, int> cnt;
        set<ui> s;

        cnt[0]++;
        s.insert(0);

        while (Q--) {
            char cmd[10];
            ui x;
            scanf("%s %u", cmd, &x);

            if (cmd[0] == '+') {
                if (cnt[x] == 0)
                    s.insert(x);
                cnt[x]++;
                continue;
            }
            if (cmd[0] == '-') {
                cnt[x]--;
                if (cnt[x] == 0) {
                    s.erase(x);
                }
                continue;
            }

            ui mask = 0;
            for (int i = 30; i >= 0; i--) {
                if ((~x) & (1 << i)) {
                    ui q = ((mask >> (i + 1)) << (i + 1)) | (1 << i);
                    auto it = s.lower_bound(q);
                    if (it != s.end() && *it < (q + (1 << i))) {
                        mask = *it;
                    }
                }
            }

            printf("%u\n", (mask ^ x));
        }

        return 0;
    }


************************
AC Code (Trie)
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    struct Node {
        int cnt;
        Node* nxt[2];
        Node() {
            cnt = 0;
            fill(nxt, nxt + 2, nullptr);
        }
    };

    const int MAX_Q = 200000;
    int Q;

    int NN = 0;
    Node data[MAX_Q * 30];
    Node* root = &data[NN++];

    void insert(Node* u, int x) {
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            if (u->nxt[t] == nullptr) {
                u->nxt[t] = &data[NN++];
            }

            u = u->nxt[t];
            u->cnt++;
        }
    }

    void remove(Node* u, int x) {
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            u = u->nxt[t];
            u->cnt--;
        }
    }

    int query(Node* u, int x) {
        int res = 0;
        for (int i = 30; i >= 0; i--) {
            int t = ((x >> i) & 1);
            // if it is possible to go the another branch
            // then the result of this bit is 1
            if (u->nxt[t ^ 1] != nullptr && u->nxt[t ^ 1]->cnt > 0) {
                u = u->nxt[t ^ 1];
                res |= (1 << i);
            }
            else {
                u = u->nxt[t];
            }
        }
        return res;
    }

    int main() {
        scanf("%d", &Q);

        insert(root, 0);

        while (Q--) {
            char cmd[10]; int x;
            scanf("%s %d", cmd, &x);

            if (cmd[0] == '+') {
                insert(root, x);
                continue;
            }
            if (cmd[0] == '-') {
                remove(root, x);
                continue;
            }

            printf("%d\n", query(root, x));
        }

        return 0;
    }
