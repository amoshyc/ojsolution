###################################################
B. 各位數和的排序
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23647>`_
*******************************************************************************


************************
分析
************************

.. note:: 水題


************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <algorithm>
    #include <vector>
    using namespace std;

    int get_digit(int n) {
        int cnt = 0;
        while (n) {
            cnt += n % 10;
            n /= 10;
        }
        return cnt;
    }

    bool cmp(const int& a, const int& b) {
        int da = get_digit(a);
        int db = get_digit(b);
        if (da == db) return a < b;
        return da < db;
    }

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int TC; cin >> TC;
        while (TC--) {
            int N; cin >> N;
            vector<int> data(N, 0);
            for (int i = 0; i < N; i++) {
                cin >> data[i];
            }

            sort(data.begin(), data.end(), cmp);

            for (int i = 0; i < N; i++) {
                if (i != 0) cout << " ";
                cout << data[i];
            }
            cout << "\n";
        }

        return 0;
    }
