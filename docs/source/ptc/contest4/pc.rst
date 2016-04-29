###################################################
C. Unique elements
###################################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2


*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23648>`_
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

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N;
        while (cin >> N) {
            vector<int> data(N, 0);
            vector<int> ans;
            for (int i = 0; i < N; i++)
                cin >> data[i];

            sort(data.begin(), data.end());
            for (int i = 0; i < N; i++) {
                bool isalone = true;
                if (i + 1 < N && data[i + 1] == data[i])
                    isalone = false;
                if (i - 1 >= 0 && data[i - 1] == data[i])
                    isalone = false;

                if (isalone)
                    ans.push_back(data[i]);
            }

            if (ans.size() == 0) cout << "N/A\n";
            else {
                for (size_t i = 0; i < ans.size(); i++) {
                    if (i != 0) cout << " ";
                    cout << ans[i];
                }
                cout << endl;
            }
        }
        return 0;
    }
