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

************************
模板驗證
************************

`poj3468 <http://codepad.org/faJWOZW4>`_
