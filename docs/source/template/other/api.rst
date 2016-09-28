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

.. code-block:: cpp
    :linenos:

    vector/deque
        []: [idx] -> val // O(1)
        erase: [it] -> it
        erase: [it s, it t] -> it
        resize: [sz, val = 0] -> void
        insert: [it, val] -> void // insert before it
        insert: [it, cnt, val] -> void // insert before it
        insert: [it pos, it from_s, it from_t] -> void //insert before it
    queue
        front: [] -> val
    stack
        top: [] -> val
    pq
        top: [] -> val

    set/mulitset
        insert: [val] -> pair<it, bool> // bool: if val already exist
        erase: [val] -> void
        erase: [it] -> void
        clear: [] -> void
        find: [val] -> it
        count: [val] -> sz
        lower_bound: [val] -> it
        upper_bound: [val] -> it
        equal_range: [val] -> pair<it, int>

    map/mulitmap
        begin/end: [] -> it (*it = pair<key, val>)
        []: [val] -> map_t&
        insert: [pair<key, val>] -> pair<it, bool>
        erase: [key] -> sz
        clear: [] -> void
        find: [key] -> it
        count: [key] -> sz
        lower_bound: [key] -> it
        upper_bound: [key] -> it
        equal_range: [key] -> it

    algorithm
        any_of: [it s, it t, unary_func] -> bool // C++11
        all_of: [it s, it t, unary_func] -> bool // C++11
        none_of: [it s, it t, unary_func] -> bool // C++11
        find: [it s, it t, val] -> it
        find_if: [it s, it t, unary_func] -> it
        count: [it s, it t, val] -> int
        count_if: [it s, it t, unary_func] -> int
        copy: [it fs, it ft, it ts] -> void // t should be allocated
        equal: [it s1, it t1, it s2, it t2] -> bool
        remove: [it s, it t, val] -> it (it = new end)
        unique: [it s, it t] -> it (it = new end)
        random_shuffle: [it s, it t] -> void
        lower_bound: [it s, it t, val, binary_func(a, b): a < b] -> it
        upper_bound: [it s, it t, val, binary_func(a, b): a < b] -> it
        binary_search: [it s, it t, val] -> bool ([s, t) sorted)
        merge: [it s1, it t1, it s2, it t2, it o] -> void (o allocated)
        includes: [it s1, it t1, it s2, it t2] -> bool(2 included? in 1)

    string
        replace(idx, len, string) -> void
        replace(it s1, it t1, it s2, it t2) -> void
    string <-> int
        stringstream; // remember to clear
        sscanf(s.c_str(), "%d", &i);
        sprintf(result, "%d", i); string s = result;

    numeric
        accumulate(it s, it t, val init);

    math/cstdlib
        atan2(0, -1) -> pi
        sqrt(db/ldb) -> db/ldb
        fabs(db/ldb) -> db/ldb
        abs(int) -> int
        ceil(db/ldb) -> db/ldb
        floor(db/ldb) -> db/ldb
        llabs(ll) -> ll (C++11)
        round(db/ldb) -> db/ldb (C99, C++11)
        log2(db) -> db (C99)
        log2(ldb) -> ldb (C++11)

    ctype
        toupper(char) -> char (remain same if input is not alpha)
        tolower(char) -> char (remain same if input is not alpha)
        isupper(char) -> bool
        islower(char) -> bool
        isalpha(char) -> bool
        isdigit(char) -> bool

    io printf/scanf
        int:                "%d"       /   "%d"
        doulbe:             "%lf","f"  /   "%lf"
        string:             "%s"       /   "%s"
        long long:          "%lld"     /   "%lld"
        long double:        "%Lf"      /   "%Lf"
        unsigned int:       "%u"       /   "%u"
        unsigned long long: "%ull"     /   "%ull"
        oct:                "0%o"
        hex:                "0x%x"
        scientific:         "%e"
        width:              "%05d"
        precision:          "%.5f"
        adjust left:        "%-5d"

    io cin/cout
        oct:                cout << oct << showbase;
        hex:                cout << hex << showbase;
        scientific:         cout << scientific;
        width:              cout << setw(5);
        precision:          cout << fixed << setprecision(5);
        adjust left:        cout << setw(5) << left;
