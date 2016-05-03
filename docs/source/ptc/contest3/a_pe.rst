###################################################
E. The Degrees of Separation in Social Networks
###################################################

.. sidebar:: Tags

    - ``tag_bfs``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23195>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

問圖的直徑，有沒有 O(N) 解法不確定，原本我以為有，但我找到反例。
O(N^2) 解法就是從每個點做 bfs，看離該點最遠是哪個點，在 level 多少。
注意有孤立點時要輸出 -1。

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

    typedef pair<int, int> pii;

    vector<int> graph[100];
    int N;

    int BFS(int s) {
        queue<pii> q;
        vector<bool> added(N, false);

        q.push(pii(s, 0));
        added[s] = true;

        int maximum = 0;
        while (!q.empty()) {
            pii top = q.front();
            q.pop();

            maximum = max(maximum, top.second);

            for (size_t i = 0; i < graph[top.first].size(); i++) {
                int v = graph[top.first][i];
                if (!added[v]) {
                    q.push(pii(v, top.second + 1));
                    added[v] = true;
                }
            }
        }

        for (vector<bool>::iterator it=added.begin(); it != added.end(); it++)
            if (*it == false)
                return -1;

        return maximum;
    }

    int main() {
        ios::sync_with_stdio(false);

        int T;
        cin >> T;

        while (T--) {
            cin >> N;

            for (int from = 0; from < N; from++) {
                graph[from].clear();

                string inp;
                cin >> inp;
                for (int to = 0; to < N; to++) {
                    if (inp[to] == '1')
                        graph[from].push_back(to);
                }
            }

            int maximum = BFS(0);
            if (maximum == -1) {
                cout << "-1\n";
                continue;
            }

            for (int s = 1; s < N; s++) {
                maximum = max(maximum, BFS(s));
            }

            cout << maximum << "\n";
        }

        return 0;
    }
