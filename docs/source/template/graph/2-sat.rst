###################################################
2-SAT
###################################################

.. sidebar:: Tags

    - ``tag_2sat``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

:math:`(x_i \lor x_i)`，建邊 :math:`(\lnot x_i,\, x_j)`
:math:`(x_i \lor x_j)`，建邊 :math:`(\lnot x_i,\, x_j) ,\, (\lnot x_j,\, x_i)`

.. code-block:: cpp
    :linenos:

    // 建圖
    // (x1 or x2) and ... and (xi or xj)
    // (xi or xj) 建邊
    // ~xi -> xj
    // ~xj -> xi

    tarjan(V); // scc 建立的順序是倒序的拓璞排序
    for (int i = 0; i < 2 * N; i += 2) {
        if (belong[i] == belong[i ^ 1]) {
            // 無解
        }
    }
    for (int i = 0; i < 2 * N; i += 2) { // 迭代所有變數
        if (belong[i] < belong[i ^ 1]) { // i 的拓璞排序比 ~i 的拓璞排序大
            // i = T
        }
        else {
            // i = F
        }
    }

常用公式：

:math:`\lor` 的分配律：

.. math::

    &p \lor (q \land r)  \\
    &= ((p \land q) \lor (p \land r))

xor:

.. math::

    &p \oplus q   \\
    &= \lnot ( (p \land q) \lor (\lnot p \land \lnot q))     \\
    &= (\lnot p \lor \lnot q) \land (p \lor q)     \\

************************
原理
************************

2-SAT 是用來找出使邏輯式子

.. math::

    (x_1 \lor x_2) \land (\lnot x_2 \lor x_3) \land \cdots \land (x_i \lor x_j)

為 :math:`T` 的一個 assignment。

==================
轉成圖
==================

每個邏輯變數 :math:`x_i` 都產生兩個節點 :math:`x_i,\, \lnot x_i`

對於式子 :math:`(x_i \lor x_j)`
1. 如果 :math:`x_i` 是 :math:`F`，則 :math:`x_j` 一定是 :math:`T`
2. 如果 :math:`x_j` 是 :math:`F`，則 :math:`x_i` 一定是 :math:`T`

產生兩條有向邊：

.. math::

    (\lnot x_i,\, x_j) \\
    (\lnot x_j,\, x_i)

注意若出現 :math:`x_i \lor x_i`, 即 :math:`x_i` 一定要是 :math:`T`，則建邊：

.. math:: (\lnot x_i,\, x_i)

==================
跑 tarjan
==================

強連通單元分解，相同單元會有相同的值
若同一個單元中，有 :math:`x_i` 也有 :math:`\lnot x_i`，則此式子無解。

=======================================
利用拓璞排序找出一組解
=======================================

************************
模板驗證
************************

`uva10319 <../../uva/p10319.html>`_ （判有無矛盾）
`uva11294 <../../uva/p11294.html>`_ （判有無矛盾、求出解）
