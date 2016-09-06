###################################################
KMP
###################################################

.. sidebar:: Tags

    - ``tag_kmp``
    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    // F[i] = max len of prefix of P such that it is suffix of Pi
    // F[i] = the 0-based index to go to if mismath occur at (i + 1)
    //    0123456
    // P: ababaca
    //       ^ ^
    //       k i
    // F: 0012301
    // mismatch at i=5, k=3, we should move k to F[k - 1]
    vector<int> get_fail(const string& P) {
        int m = P.length();
        vector<int> F(m, 0); // F[0] = 0

        int k = 0; // 0-based index of 1st pointer
        for (int i = 1; i < m; i++) { // 0-based index of 2nd pointer
            while (k > 0 && P[k] != P[i]) // mismatch at k, i
                k = F[k - 1];
            if (P[k] == P[i])
                k++;
            F[i] = k;
        }

        return F;
    }

    //            i
    //            v
    //    0123456789ABCDE
    // S: bacbababaabcbab
    // P:    ababaca
    //       0123456
    //            ^
    //            q
    // F:    0012301
    // mismatch occurs at i=8, q=5, we should move q to F[q - 1]

    int KMP(const string& S, const string& P) {
        int n = S.length();
        int m = P.length();
        vector<int> F = get_fail(P);

        int cnt = 0;
        int q = 0; // 0-based index on P
        for (int i = 0; i < n; i++) { // 0-based index on S
            while (q > 0 && P[q] != S[i]) // find transition P[q]
                q = F[q - 1];
            if (P[q] == S[i]) // found
                q++;
            if (q == m) { // match at (i - m + 1)
                cnt++;
                q = F[q - 1];
            }
        }

        return cnt;
    }

************************
模板驗證
************************

`poj3461 <http://codepad.org/wCW8ycvu>`_
