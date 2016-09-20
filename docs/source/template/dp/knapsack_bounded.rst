###################################################
多重背包
###################################################

.. sidebar:: Tags

    - ``tag_dp``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

二進制拆解轉 01 背包。
例如 22 是拆成 [1, 2, 4, 8, 7]
前四個 [1, 2, 4, 8] 可以組出 [0, 15] 中的任何數
再加一個 7，可以組出 [0, 22] 中的任何數

.. code-block:: cpp
    :linenos:

    typedef long long ll;

    struct Item {
        ll v, w;
    };

    vector<Item> items;
    for (int i = 0; i < N; i++) {
        ll val, w, num;
        for (ll k = 1; k <= num; k *= 2) {
            items.push_back((Item) {k * val, k * w});
            num -= k;
        }
        if (num > 0) {
            items.push_back((Item) {num * val, num * w});
        }
    }

    printf("%lld\n", knapsack01(items, W));



************************
模板驗證
************************

`poj1276 <../../poj/p1276.html>`_
