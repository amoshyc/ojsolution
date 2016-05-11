###################################################
折半完全列舉
###################################################

.. sidebar:: Tags

    - ``tag_half_enum``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

::

    給定大小為 N (N < 35) 的集合，請問該集合所有的非空子集中，總和的絕對值最小是多少？
    記錄所有第 i ~ 第 N//2 元素能產生的所有 <總和, 個數> 於 S1 中，
    記錄所有第 N//2 + 1 ~ 第 N -1 元素能產生的所有 <總和, 個數> 於 S2 中。
    對 S2 排序，枚舉 S1 的元素 <s, n>，看 S2 中最接近 -s 的兩個元素與 s 加起來的總合，
    有沒有比目前最佳解更好。最佳解也有可能單獨發生在 s1 或 s2 中。

.. code-block:: cpp
    :linenos:

    dfs(s1, 0, N / 2, 0, 0); // enum first half
    dfs(s2, N / 2, N, 0, 0); // enum last half

    for (it_t it = s1.begin(); it != s1.end(); ++it)
        update(it->st, it->nd);
    for (it_t it = s2.begin(); it != s2.end(); ++it)
        update(it->st, it->nd);

    sort(s2.begin(), s2.end());
    for (it_t i = s1.begin(); i != s1.end(); ++i) {
        it_t j = lower_bound(s2.begin(), s2.end(), pli(-(i->st), 0));
        if (j != s2.end())
            update((i->st) + (j->st), (i->nd) + (j->nd));
        if (j-- != s2.begin())
            update((i->st) + (j->st), (i->nd) + (j->nd));
    }

************************
模板驗證
************************

- `poj3977 <http://codepad.org/rwjFjNsv>`_
