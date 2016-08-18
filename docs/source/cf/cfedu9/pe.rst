###################################################
[cfedu9] E. Thief in a Shop
###################################################

.. sidebar:: Tags

    - ``tag_fft``

.. contents:: TOC
    :depth: 2


******************************************************
`題目 <http://codeforces.com/contest/632/problem/E>`_
******************************************************

給定一個背包可以容納 K 個物品，現在 N 種物品，第 i 種物品價值 A[i]，且有無限多個。
請問在考慮所有放法，背包放入 K 個物品，背包內的物品價值和可以達到哪些數字？

************************
Specification
************************

::

    1 <= N, K <= 10^3
    1 <= A[i] <= 10^3

************************
分析
************************

.. note:: fft

算是 fft 的標準題（？
這是我第一個 fft 題…

先考慮只拿一個，能達到的數字當然就是所有物品的價值 A[0], A[1], ..., A[N - 1]
把它轉成一個多項式 P，若數字 i 在 A 中，那 x^i 的係數就是 1，要不然就是 0。

舉個例，假設 A = [1, 3, 4]，那 P 就是::

    x^4 + x^3 + x

就我的感想，這就只是用一個多項式而不是一個 bool 陣列，來存各個數字可不可以達到。
這有什麼優點呢？如果我們要拿二個，我們只需把 P 平方，
如果 x^i 項的係數不為 0，那 i 這個數字就是可以達到的。
同一個例子，P^2 =
::

    (x^8) + (2 * x^7) + (x^6) + (2 * x^5) + (2 * x^4) + (x^2)

也就是說，拿二個時，能達到的數字為::

    8 7 6 5 4 2

而這題拿 K 個，就是 P^K，可以用個快速冪加速來確保不會 TLE

--------------------------------

要處理多項式乘法，當然就是 fft 啦！什麼？你不懂為什麼？去看 CLRS 吧，寫得非常好~大推
（正確來講是 dft，但使用 fft 來達到 O(nlgn) 的時間）

話說我發現 fft 的執行時間竟然受 bit-reverse-copy 的實作好壞影響非常大。
寫得好或不好（都是 O(nlgN) 的說），在這題可以差到 1s 以上（執行時間的 1/3 以上）。
底下的 rev_bit 是出自大神 `morris <http://morris821028.github.io/>`_ 的 code。
基本想法是反轉 32 個 bit，再平移到正確位置。接近 O(1) 的時間。

快速冪部份 ans 的初使值是多項式的單位元素，也就是 1。乘上任何多項式都得到多項式本身。
另外，不要用 c++ 內建的 complex，非常非常慢，直接讓你 TLE。
這個程式碼達行時間為 1.8s ~ 2s 左右，在所有用 fft 解這題的程式中應該算不錯了。

************************
AC Code
************************

.. code-block:: cpp
    :linenos:

    #include <bits/stdc++.h>
    using namespace std;

    typedef unsigned int ui;
    typedef long double ldb;
    const ldb pi = atan2(0, -1);

    struct Complex {
        ldb real, imag;
        Complex(): real(0.0), imag(0.0) {;}
        Complex(ldb a, ldb b) : real(a), imag(b) {;}
        Complex conj() const {
            return Complex(real, -imag);
        }
        Complex operator + (const Complex& c) const {
            return Complex(real + c.real, imag + c.imag);
        }
        Complex operator - (const Complex& c) const {
            return Complex(real - c.real, imag - c.imag);
        }
        Complex operator * (const Complex& c) const {
            return Complex(real*c.real - imag*c.imag, real*c.imag + imag*c.real);
        }
        Complex operator / (ldb x) const {
            return Complex(real / x, imag / x);
        }
        Complex operator / (const Complex& c) const {
            return *this * c.conj() / (c.real * c.real + c.imag * c.imag);
        }
    };

    inline ui rev_bit(ui x, int len){
    	x = ((x & 0x55555555u) << 1)  | ((x & 0xAAAAAAAAu) >> 1);
    	x = ((x & 0x33333333u) << 2)  | ((x & 0xCCCCCCCCu) >> 2);
    	x = ((x & 0x0F0F0F0Fu) << 4)  | ((x & 0xF0F0F0F0u) >> 4);
    	x = ((x & 0x00FF00FFu) << 8)  | ((x & 0xFF00FF00u) >> 8);
    	x = ((x & 0x0000FFFFu) << 16) | ((x & 0xFFFF0000u) >> 16);
    	return x >> (32 - len);
    }

     // flag = -1 if ifft else +1
    void fft(vector<Complex>& a, int flag = +1) {
        int n = a.size(); // n should be power of 2

        int len = __builtin_ctz(n);
        for (int i = 0; i < n; i++) {
            int rev = rev_bit(i, len);

            if (i < rev)
                swap(a[i], a[rev]);
        }

        for (int m = 2; m <= n; m <<= 1) { // width of each item
            auto wm = Complex(cos(2 * pi / m), flag * sin(2 * pi / m));
            for (int k = 0; k < n; k += m) { // start idx of each item
                auto w = Complex(1, 0);
                for (int j = 0; j < m / 2; j++) { // iterate half
                    Complex t = w * a[k + j + m / 2];
                    Complex u = a[k + j];
                    a[k + j] = u + t;
                    a[k + j + m / 2] = u - t;
                    w = w * wm;
                }
            }
        }

        if (flag == -1) { // if it's ifft
            for (int i = 0; i < n; i++)
                a[i].real /= n;
        }
    }

    vector<int> mul(const vector<int>& a, const vector<int>& b) {
        int n = int(a.size()) + int(b.size()) - 1;
        int nn = 1;
        while (nn < n)
            nn <<= 1;

        vector<Complex> fa(nn, Complex(0, 0));
        vector<Complex> fb(nn, Complex(0, 0));
        for (int i = 0; i < int(a.size()); i++)
            fa[i] = Complex(a[i], 0);
        for (int i = 0; i < int(b.size()); i++)
            fb[i] = Complex(b[i], 0);

        fft(fa, +1);
        fft(fb, +1);
        for (int i = 0; i < nn; i++) {
            fa[i] = fa[i] * fb[i];
        }
        fft(fa, -1);

        vector<int> c;
        for(int i = 0; i < nn; i++) {
            int val = int(fa[i].real + 0.5);
            if (val) {
                while (int(c.size()) <= i)
                    c.push_back(0);
                c[i] = 1;
            }
        }

        return c;
    }

    int main() {
        int N, K;
        scanf("%d %d", &N, &K);

        vector<int> a;
        for (int i = 0; i < N; i++) {
            int inp; scanf("%d", &inp);
            while (int(a.size()) <= inp)
                a.push_back(0);
            a[inp] = 1;
        }

        // a^k
        vector<int> ans(1, 1); // identity of polynomials
        vector<int> base(a);
        while (K) {
            if (K & 1) ans = mul(ans, base);
            K >>= 1;
            base = mul(base, base);
        }

        for (int i = 0; i < int(ans.size()); i++) {
            if (ans[i] > 0) {
                printf("%d ", i);
            }
        }
        puts("");

        return 0;
    }
