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

    const int MAX_Np = ...;
    int F[MAX_Np];

    void fail_func(const string& P) {
        const int Np = P.length();
        memset(F, 0, sizeof(F));
        int i = 0, j = -1; F[0] = -1;
        while (i < Np) {
            while (j >= 0 && P[i] != P[j]) j = F[j];
            i++; j++;
            F[i] = j;
        }
    }

    int KMP(const string& P, const string& S) {
        int cnt = 0;
        const int Np = P.length();
        const int Ns = S.length();

        failure_function(P);

        int i = 0, j = 0;
        while (i < Ns) {
            while (j >= 0 && S[i] != P[j]) j = F[j];
            i++; j++;
            if (j == Np) {
                cnt++;
                j = F[j];
            }
        }

        return cnt;
    }


Code rewritten from CLRS:

.. code-block:: cpp
    :linenos:

    vector<int> get_fail(const string& s) {
        int len = s.length();
        vector<int> F(len, 0);

        F[0] = 0;
        int k = 0; // candidate len
        for (int i = 1; i < len; i++) {
            while (k > 0 && s[k] != s[i])
                k = F[k - 1]; // since we're 0-based
            if (s[k] == s[i])
                k++;
            F[i] = k;
        }

        return F;
    }

************************
模板驗證
************************

`poj3468 <http://codepad.org/faJWOZW4>`_
