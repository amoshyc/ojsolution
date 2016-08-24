###################################################
Cheatsheet
###################################################

.. contents:: TOC
    :depth: 2

************************
Math formula
************************

::

    .. math::

        a^{-1} &\equiv b\pmod n                             &\qquad (1) \\
        a^{-1} &= n \cdot q + b \quad (q \in \mathbb{R})    &\qquad (2)

    reference to equation :math:`\ref{eq1}`

--------------

inline :math:`a^2` mathjax.

.. math::

    a^{-1} &\equiv b\pmod n                             &\qquad (1) \\
    a^{-1} &= n \cdot q + b \quad (q \in \mathbb{R})    &\qquad (2)


************************
Hyperlink
************************

::

    `(web link) text <https://www.google.com.tw>`_
    `(local link) tags <./tags.html>`_

--------------

`(web link) text <https://www.google.com.tw>`_
`(local link) tags <./tags.html>`_

************************
Image
************************

::

    .. image:: http://i.imgur.com/Y5Vn53w.jpg
        :width: 400px

--------------

.. image:: http://i.imgur.com/Y5Vn53w.jpg
    :width: 400px
