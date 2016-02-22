######################
reST CheatSheet
######################

.. contents::
    :depth: 2

You can refer to `Another Reference <http://rest-sphinx-memo.readthedocs.org/en/latest/ReST.html
/>`_

**********************
Headers
**********************

::

    ##############
    Headers
    ##############

    **************
    H2
    **************

    H3
    ==============

    H4
    --------------

    H5
    ^^^^^^^^^^^^^^

    H6
    """"""""""""""

********************
Quote
********************

::

    ::

        quotation

********************
Code Block
********************

::

    .. code-block:: cpp
        :linenos:
        :emphasize-lines: 1-2,7,9-

        #include <bits/stdc++.h>
        using namespace std;

        int main() {
            return 0;
        }

results in

.. code-block:: cpp
    :linenos:
    :emphasize-lines: 1-2,7,9-

    #include <bits/stdc++.h>
    using namespace std;

    typedef long long ll;
    typedef pair<ll, ll> pll;

    int main() {
        return 0;
    }


****************
Inline Markup
****************

::

    *italic*
    **bold**
    ``code``

results in *italic*, **bold**, ``code`` and it can\ **not** be nested.

To render space, asterisk beside inline markup, prefix ``\`` should be used::

    can\ **not**
    \*\*bold\*\*

can\ **not**
\*\*bold\*\*


*******************
Links & Bookmarks
*******************

::

    .. _demo_header:

    Title
    ==========================

    `Python <http://www.python.org/>`_

    `Link to Header <#code-block>`_

    This is a paragraph that contains `a link`_.

    .. _a link: http://example.com/

    This is a link to :ref:`Header`.


.. _demo_header:

*************************
Title
*************************

`Python <http://www.python.org/>`_

Link to Header: `Code Block <#code-block>`_

This is a paragraph that contains `a link`_.

.. _a link: http://example.com/

This is a link to :ref:`demo_header`. It can also be a label in external files.


*************************
TOC & Content
*************************

::

    .. toctree::
        :maxdepth: 2
        :numbered: 3

        cheatsheet/cheatsheet
        tutorials/tutorials
        template
        cf/cf
        poj/poj
        uva/uva
        ptc/ptc

    .. content::
        :depth: 2


*************************
Horizontal Line
*************************

::

    (empty line)
    ------------------------------
    (empty line)

text before

------------------------

text after

*************************
Tables
*************************


*************************
Mathjax
*************************

::

    formula in text :math:`a^2 + b^2 = c^2` .

    .. math:: (a + b)^2 = a^2 + 2ab + b^2

    .. math::
        :nowrap:

        \begin{eqnarray}
            y    & = & ax^2 + bx + c \\
            f(x) & = & x^2 + 2xy + y^2
        \end{eqnarray}


formula in text :math:`a^2 + b^2 = c^2` .

.. math:: (a + b)^2 = a^2 + 2ab + b^2

.. math::
    :nowrap:

    \begin{eqnarray}
        y    & = & ax^2 + bx + c \\
        f(x) & = & x^2 + 2xy + y^2
    \end{eqnarray}
