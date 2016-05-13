###################################################
API
###################################################

.. sidebar:: Tags

    - ``tag_template``

.. contents:: TOC
    :depth: 2

************************
程式碼
************************

Set

.. code-block:: cpp
    :linenos:

    set<T> s; // iterable
    void clear();
    size_t count(T val); // number of val in set
    void erase(T val);
    it find(T val); // = s.end() if not found
    void insert(T val);
    it lower_bound(T val); // = s.end() if not found, *it = <key, val>
    it upper_bound(T val); // = s.end() if not found, *it = <key, val>

------------------

Map

.. code-block:: cpp
    :linenos:

    map<T1, T2> m; // iterable
    void clear();
    void erase(T1 key);
    it find(T1 key); // <key, val>
    void insert(pair<T1, T2> P);
    T2& [](T1 key); // if key not in map, new key will be inserted with default val
    it lower_bound(T1 key); // = m.end() if not found, *it = <key, val>
    it upper_bound(T1 key); // = m.end() if not found, *it = <key, val>

-------------------

Algorithm

.. code-block:: cpp
    :linenos:

    // return if i is smaller than j
    comp = [&](const T& i, const T& j) -> bool;
    vector<T> v;
    bool any_of(v.begin(), v.end(), [&](const T& i) -> bool);
    bool all_of(v.begin(), v.end(), [&](const T& i) -> bool);
    void copy(inp.begin(), in.end(), out.begin());
    int count(v.begin(), v.end(), int val); // number of val in v
    it unique(v.begin(), v.end()); // it - v.begin() = size
    // after calling, v[nth] will be n-th smallest elem in v
    void nth_element(v.begin(), nth_it, bin_comp);
    void merge(in1.begin(), in1.end(), in2.begin(), in2.end(), out.begin(), comp);
    // include union, intersection, difference, symmetric_difference(xor)
    void set_union(in1.begin(), in1.end(), in2.begin(), in2.end(), out.begin(), comp);
    bool next_permutation(v.begin(), v.end());
    // v1, v2 need sorted already, whether v1 includes v2
    bool inclues(v1.begin(), v1.end(), v2.begin(), v2.end());
    it find(v.begin(), v.end(), T val); // = v.end() if not found
    it search(v1.begin(), v1.end(), v2.begin(), v2.end());
    it lower_bound(v.begin(), v.end(), T val);
    it upper_bound(v.begin(), v.end(), T val);
    bool binary_search(v.begin(), v.end(), T val); // exist in v ?
    void sort(v.begin(), v.end(), comp);
    void stable_sort(v.begin(), v.end(), comp);
