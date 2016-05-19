#####################################
[cf346] B. Qualifying Contest
#####################################

.. sidebar:: Tags

    - ``tag_priority_queue``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/659/problem/B>`_
******************************************************

N 個人參加分為 M 區的競賽，每個人有其權重，保證每區的參數人數有至少兩個人。
現給定每個人的賽區與權重，請問可否唯一地決定出每個賽區權重最大與次大的人。
即該分區內沒有其它人的權重與權重第二大的人相同。

************************
Specification
************************

::

    1 <= M <= 10^4
    2*M <= N <= 10^5

************************
分析
************************

.. note:: 題意理解

如果讀懂題意的話就簡單了，用 sorting 或 priority queue
把分區內權重最大、次大、第三大的人拿出來看看，看能不能唯一決定出前兩大。

注意有個 special case: 分區只有兩人，這時這兩人就是答案了。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    const int max_m = 10000;

    typedef pair<int, string> pis;

    priority_queue<pis> cnt[max_m];

    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);

        int N, M;
        cin >> N >> M;
        for (int i = 0; i < N; i++) {
            string name; int r, s;
            cin >> name >> r >> s;
            cnt[r - 1].push(pis(s, name));
        }

        for (int i = 0; i < M; i++) {
            if (cnt[i].size() == 2) {
                pis p1 = cnt[i].top(); cnt[i].pop();
                pis p2 = cnt[i].top(); cnt[i].pop();
                cout << p1.second << " " << p2.second << "\n";
            }
            else {
                pis p1 = cnt[i].top(); cnt[i].pop();
                pis p2 = cnt[i].top(); cnt[i].pop();
                pis p3 = cnt[i].top(); cnt[i].pop();

                if (p2.first == p3.first) {
                    cout << "?\n";
                }
                else {
                    cout << p1.second << " " << p2.second << "\n";
                }
            }
        }

        return 0;
    }
