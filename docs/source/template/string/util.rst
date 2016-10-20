###################################################
Utility
###################################################

.. sidebar:: Tags

    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    #define sz(x) (int(x.size()))

    vector<int> get_fail(const string& P) {
        int szP = sz(P);
        vector<int> F(szP, 0);
        int k = 0;
        for (int i = 1; i < szP; i++) {
            while (k > 0 && P[k] != P[i]) k = F[k - 1];
            if (P[k] == P[i]) k++;
            F[i] = k;
        }
        return F;
    }

    int kmp(const string& S, const string& P, const vector<int>& F, int pos = 0) {
        int szS = sz(S), szP = sz(F);
        int q = 0; // search
        for (int i = pos; i < szS; i++) { // 0-based index on S
            while (q > 0 && P[q] != S[i]) q = F[q - 1];
            if (P[q] == S[i]) q++;
            if (q == szP) return (i - szP + 1);
        }
        return -1;
    }

    vector<string> split1(const string& inp, const string& d) {
        vector<string> res;
        int idx = 0, pos = 0, len = sz(inp);
        // vector<int> f = get_fail(d);
        // while (idx < len && (pos = kmp(inp, d, f, idx)) != -1) {
        while (idx < len && (pos = inp.find(d, idx)) != -1) {
            res.push_back(inp.substr(idx, pos - idx));
            idx = pos + sz(d);
        }
        if (idx < len) {
            res.push_back(inp.substr(idx, len - idx));
        }
        return res;
    }

    vector<string> split2(const string& inp, const string& ds) {
        vector<string> res;
        int s = 0, t = 0, len = sz(inp);
        for (;;) {
            s = t;
            while (s < len && int(ds.find(inp[s])) != -1) s++;
            if (s >= len) break;
            t = s;
            while (t < len && int(ds.find(inp[t])) == -1) t++;
            res.push_back(inp.substr(s, t - s));
            if (t >= len) break;
        }
        return res;
    }

    string join(const string& d, const vector<string>& tokens) {
        int total_len = max((sz(tokens) - 1) * sz(d), 0);
        for (const string& s : tokens)
            total_len += sz(s);
        string res(total_len, '$');
        int idx = 0;
        for (int i = 0; i < sz(tokens); i++) {
            if (i != 0) {
                copy(d.begin(), d.end(), res.begin() + idx);
                idx += sz(d);
            }
            copy(tokens[i].begin(), tokens[i].end(), res.begin() + idx);
            idx += sz(tokens[i]);
        }
        return res;
    }

    string replace(const string& inp, const string& key, const string& val) {
        if (sz(key) == 0) return inp;
        vector<string> tokens;
        int idx = 0, pos = 0, len = sz(inp);
        // vector<int> f = get_fail(key);
        // while (idx < len && (pos = kmp(inp, key, f, idx)) != -1) {
        while (idx < len && (pos = inp.find(key, idx)) != -1) {
            tokens.push_back(inp.substr(idx, pos - idx));
            tokens.push_back(val);
            idx = pos + sz(key);
        }
        if (idx < len) {
            tokens.push_back(inp.substr(idx, sz(inp) - idx));
        }
        int total_len = 0;
        for (const string& s: tokens)
            total_len += sz(s);
        string res(total_len, '$');
        int i = 0;
        for (const string& s: tokens) {
            copy(s.begin(), s.end(), res.begin() + i);
            i += sz(s);
        }
        return res;
    }

    int main() {
        cout << kmp("abc", "", get_fail("bc")) << endl;

        for (string s : split1("1, 2, 3, 4, 5", ", "))
            cout << s << " ";
        cout << endl;

        for (string s : split2("(1, 2, 3) (4, 5, 6) 7", " ,()"))
            cout << s << " ";
        cout << endl;

        cout << join(" ", vector<string>{"1", "2", "3"}) << endl;
        cout << replace("1 2 3 4 ", "", "||") << endl;

        return 0;
    }


************************
模板驗證
************************

待測
