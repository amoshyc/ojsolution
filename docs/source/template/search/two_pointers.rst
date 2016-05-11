###################################################
爬行法
###################################################

.. sidebar:: Tags

    - ``tag_two_pointers``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

::

    N 個人，每個人有其金錢 m 與友情值 f。
    從中選一些人，其中每一個人皆不允許有其它人的金錢比他多 D 以上（含）。
    請問在所有選取方式中，最大的友情值和是多少？
    依每個人的金錢由小排到大，然後在後在友情值上爬行。


.. code-block:: cpp
    :linenos:

    int st = 0, ed = 0, sum = 0, ans = -1;
    for (;;) {
        while (ed < N && data[end].m - data[start].m < D)
            sum += data[end++].f;
        ans = max(ans, sum);
        if (end == N) break;
        sum -= data[start++].f;
    }

---------------------------

::

    給定長度為 N 的序列 a[i] 與正整數 S，求總和 >= S 的區間的最短長度。

.. code-block:: cpp
    :linenos:

    int s = 0, it = 0, sum = 0, ans = N + 1;
    for (;;) {
        while (t < N && sum < S)
            sum = sum + data[t++];
        if (sum < S)
            break;
        if (t - s < ans)
            ans = t - s;
        sum = sum - data[s++];
    }

************************
模板驗證
************************

- `cf321b <http://codepad.org/jajc7GFZ>`_
- `poj3061 <http://codepad.org/JteHVjy7>`_
