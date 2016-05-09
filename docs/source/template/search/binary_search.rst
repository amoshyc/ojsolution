###################################################
Binary Search
###################################################

.. sidebar:: Tags

    - ``tag_binary_search``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    int binary_search() {
        // 0 0 0 0 1 1 1
        int lb = ..., ub = ...;
        if (C(lb) == true) return ...;
        if (C(ub) == false) return ...;

        while (ub - lb > 1) {
            int mid = (lb + ub) / 2;
            if (C(mid)) ub = mid;
            else lb = mid;
        }
        return ub;
    }

    int binary_search() {
        // 1 1 1 1 0 0 0
        int lb = ..., ub = ...;
        if (C(lb) == false) return ...;
        if (C(ub) == true) return ...;

        while (ub - lb > 1) {
            int mid = (lb + ub) / 2;
            if (C(mid)) lb = mid;
            else ub = mid;
        }
        return lb;
    }


************************
模板驗證
************************

`2015 桂冠賽 I <../../../build/html/ptc/contest4/pi.html>`_
