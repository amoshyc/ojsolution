###################################################
D. Wu-Zi-Qi（五子棋）
###################################################

.. sidebar:: Tags

    - ``tag_implementation``

.. contents:: TOC
    :depth: 2

*******************************************************************************
`題目 <http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=23190>`_
*******************************************************************************

************************
分析
************************

.. note:: 水題

苦工題…

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <cstring>
    #include <sstream>

    using namespace std;

    int map[20][20];

    int mystoi(string s) {
        int value = 0;
        int len = s.length();
        for (int i = 0; i < len; i++)
            value = value * 10 + s[i] - '0';
        return value;
    }

    int max5(int row, int col, int dr, int dc) {
        int max_ = map[row][col];
        for (int i = 1; i < 5; i++)
            max_ = max(max_, map[row + i * dr][col + i * dc]);
        return max_;
    }

    bool check5(int row, int col, int dr, int dc) {
        for (int i = 0; i < 5; i++)
            if (map[row + i * dr][col + i * dc] == -1)
                return false;
        for (int i = 1; i < 5; i++)
            if (map[row + i * dr][col + i * dc] % 2 != map[row][col] % 2)
                return false;

        int br1 = row + 5 * dr;
        int bc1 = col + 5 * dc;
        if (0 <= br1 && br1 <= 19 && 0 <= bc1 && bc1 <= 19 &&
            map[br1][bc1] != -1 && map[br1][bc1] % 2 == map[row][col] % 2)
            return false;

        int br2 = row + (-1) * dr;
        int bc2 = col + (-1) * dc;
        if (0 <= br2 && br2 <= 19 && 0 <= bc2 && bc2 <= 19 &&
            map[br2][bc2] != -1 && map[br2][bc2] % 2 == map[row][col] % 2)
            return false;

        return true;
    }

    void find_winner() {
        int earlist = 100000;

        // vertical
        for (int row = 0; row <= 15; row++)
            for (int col = 0; col <= 19; col++)
                if (check5(row, col, 1, 0))
                    earlist = min(earlist, max5(row, col, 1, 0));

        // horizontal
        for (int row = 0; row <= 19; row++)
            for (int col = 0; col <= 15; col++)
                if (check5(row, col, 0, 1))
                    earlist = min(earlist, max5(row, col, 0, 1));

        // main diagonal
        for (int row = 0; row <= 15; row++)
            for (int col = 0; col <= 15; col++)
                if (check5(row, col, 1, 1))
                    earlist = min(earlist, max5(row, col, 1, 1));

        // sub diagonal
        for (int row = 0; row <= 15; row++)
            for (int col = 4; col <= 19; col++)
                if (check5(row, col, 1, -1))
                    earlist = min(earlist, max5(row, col, 1, -1));

        // output
        if (earlist == 100000)
            cout << "T\n";
        else if (earlist % 2 == 0)
            cout << "A\n";
        else
            cout << "B\n";
    }

    int main() {
        ios::sync_with_stdio(false);

        int T;
        cin >> T;

        string temp;
        getline(cin, temp); // eat the endl

        while (T--) {
            memset(map, -1, sizeof(map));

            string sa;
            string sb;

            getline(cin, sa);
            getline(cin, sb);

            stringstream ss;
            string p;
            int level;

            ss << sa;
            level = 0;
            while (ss >> p) {
                int idx = ((p[2] == ',') ? 2 : 3);
                int row = mystoi(p.substr(1, idx - 1));
                int col = mystoi(p.substr(idx + 1, p.length() - idx - 1 - 1));

                map[row][col] = level;
                level += 2;
            }
            ss.clear();

            ss << sb;
            level = 1;
            while (ss >> p) {
                int idx = ((p[2] == ',') ? 2 : 3);
                int row = mystoi(p.substr(1, idx - 1));
                int col = mystoi(p.substr(idx + 1, p.length() - idx - 1 - 1));

                map[row][col] = level;
                level += 2;
            }

            find_winner();
        }

        return 0;
    }
