#####################################
[cf128] C. Photographer
#####################################

.. sidebar:: Tags

    - ``tag_easy``

.. contents:: TOC
    :depth: 2

******************************************************
`題目 <http://codeforces.com/contest/203/problem/C>`_
******************************************************

************************
Specification
************************

************************
分析
************************

.. note:: 水題

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;

    struct P {
        int id;
        ll need;
    };


    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0);
        cout.tie(0);

        int N;
        ll A, B, D;
        cin >> N >> D;
        cin >> A >> B;

        vector<P> inp(N);
        for (int i = 0; i < N; i++) {
            int x, y; cin >> x >> y;
            inp[i].id = i + 1;
            inp[i].need = x * A + y * B;
        }

        sort(inp.begin(), inp.end(), [](const P& a, const P& b) {
            return a.need < b.need;
        });

        vector<int> ans;
        for (P p : inp) {
            if (D < p.need) break;
            D -= p.need;
            ans.push_back(p.id);
        }

        cout << ans.size() << endl;
        for (int id : ans)
            cout << id << " ";
        cout << endl;

        return 0;
    }
