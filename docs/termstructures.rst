###############
Term Structures
###############

QuantLib provides a module for the representation of different term structures used in Quantitative Finance. A term structure describe the evolution of any variable defined across maturities.
Mathematically a term structure describe the stochastic evolution of a variable :math:`X(t, T)`, indexed by current time :math:`t` and maturity :math:`T \ge t`, such that the processes satisfies no-arbitrage conditions and is consistent with observed market prices.

.. math::

	X: (t, T) \rightarrow X(t, T) \in \mathbb{R}, T \ge t

In QuantLib, term structures (represented as objects) are defined for several financial variables, the main categories are:

- :ref:`termstructures-yield`
- :ref:`termstructures-volatility`
- :ref:`termstructures-credit`
- :ref:`termstructures-inflation`


.. include:: termstructures/yield.rst

-------------

.. include:: termstructures/volatility.rst

-------------

.. include:: termstructures/credit.rst

-------------

.. include:: termstructures/inflation.rst