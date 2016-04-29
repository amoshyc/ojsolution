###################################################
H. Weighted Binary Tree
###################################################

.. sidebar:: Tags

    - ``tag_huffman``
    - ``tag_priority_queue``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23654>`_
*******************************************************************************

待補

************************
Specification
************************

::

    1 <= N <= 10^6
    0 <= w[i] <= 10^5

************************
分析
************************

.. note:: Huffman Coding 變形

待補

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <queue>
    #include <algorithm>
    #include <cstring>
    using namespace std;

    typedef long long ll;

    int main() {
        int TC; cin >> TC;
        while (TC--) {
            int N; cin >> N;
            priority_queue< ll, vector<ll>, greater<ll> > pq;
            for (int i = 0; i < N; i++) {
                int inp; cin >> inp;
                pq.push(inp);
            }

            while (pq.size() > 1) {
                ll u = pq.top(); pq.pop();
                ll v = pq.top(); pq.pop();
                pq.push(max(u, v) + 1);
            }

            cout << pq.top() << "\n";
        }
        return 0;
    }
