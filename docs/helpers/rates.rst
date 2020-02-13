
Interest Rate
#############

DepositRateHelper
*****************



.. function:: ql.DepositRateHelper (quote, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)

.. code-block:: python

  quote = ql.QuoteHandle(ql.SimpleQuote(0.05))
  tenor = ql.Period('6M')
  fixingDays = 2
  calendar = ql.TARGET()
  convention = ql.ModifiedFollowing
  endOfMonth = False
  dayCounter = ql.Actual360()
  ql.DepositRateHelper(quote, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)
  
.. function:: ql.DepositRateHelper(rate, tenor, fixingDays, calendar, convention, endOfMonth, dayCounter)

.. code-block:: python

  ql.DepositRateHelper(0.05, ql.Period('6M'), 2, ql.TARGET(), ql.ModifiedFollowing, False, ql.Actual360())


.. function:: ql.DepositRateHelper(quote, index)

.. code-block:: python

  ql.DepositRateHelper(ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Euribor6M())


.. function:: ql.DepositRateHelper(rate, index)


.. code-block:: python

  ql.DepositRateHelper(0.05, ql.Euribor6M());


FraRateHelper
*************

.. function:: ql.FraRateHelper()


FuturesRateHelper
*****************

.. function:: ql.FuturesRateHelper()

SwapRateHelper
**************

.. function:: ql.SwapRateHelper()

OISRateHelper
*************

.. function:: ql.OISRateHelper()

DatedOISRateHelper
******************

.. function:: ql.DatedOISRateHelper()

FxSwapRateHelper
****************

.. function:: ql.FxSwapRateHelper()


FixedRateBondHelper
*******************

.. function:: ql.FixedRateBondHelper()


BondHelper
**********

.. function:: ql.BondHelper(cleanPrice, bond)


BondHelperVector
****************

.. function:: ql.BondHelperVector()
