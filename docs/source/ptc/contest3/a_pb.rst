###################################################
B. Multiples
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23188>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

直接排就可以了，N 很小。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <algorithm>
    #include <vector>

    using namespace std;

    int main() {
        ios::sync_with_stdio(false);

        int T;
        cin >> T;

        while(T--) {
            vector<int> data;

            int inp;
            while (cin >> inp) {
                if (inp == -1) break;
                data.push_back(inp);
            }

            sort(data.begin(), data.end());

            int N;
            cin >> N;

            int M = data.size();
            int cnt = 0;

            do {
                int value = 0;
                for (int i = 0; i < M; i++)
                    value = value * 10 + data[i];

                if (value % N == 0)
                    cnt++;
            } while (next_permutation(data.begin(), data.end()));

            cout << cnt << "\n";
        }

        return 0;
    }
