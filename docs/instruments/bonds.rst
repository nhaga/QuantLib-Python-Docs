Bonds
*****


Bond
----

Redemptions and maturity are calculated from the coupon data, if available. Therefore, redemptions must not be included in the passed cash flows.

.. py:class:: ql.Bond(settlementDays, calendar, issueDate, coupons)

    .. code-block:: python

        start = ql.Date(15,12,2019)
        maturity = ql.Date(15,12,2020)
        schedule = ql.MakeSchedule(start, maturity, ql.Period('6M'))

        interest = ql.FixedRateLeg(schedule, ql.Actual360(), [100.], [0.05])
        bond = ql.Bond(0, ql.TARGET(), start, interest)


    .. py:method:: .bondYield(dayCounter, compounding, frequency, accuracy=1.0e-8, maxEvaluations=100)

    .. py:method:: .bondYield(cleanPrice, dayCounter, compounding, frequency, settlementDate=Date,  accuracy=1.0e-8, maxEvaluations=100)

        .. code-block:: python
        
            bond.bondYield(100, ql.Actual360(), ql.Compounded, ql.Annual)

    .. py:method:: .dirtyPrice()

        .. code-block:: python

            bond.dirtyPrice()

    .. py:method:: .dirtyPrice(yield, dayCount, compounding, frequency)

        .. code-block:: python

            bond.dirtyPrice(0.05, ql.Actual360(), ql.Compounded, ql.Annual)






ZeroCouponBond
--------------

.. function:: ql.ZeroCouponBond(settlementDays, calendar, faceAmount, maturityDate)

.. code-block:: python

    bond = ql.ZeroCouponBond(2, ql.TARGET(), 100, ql.Date(20,6,2020))



FixedRateBond
-------------

.. function:: ql.FixedRateBond(settlementDays, calendar, faceAmount, startDate, maturityDate, tenor, coupon, paymentConvention)

.. function:: ql.FixedRateBond(settlementDays, faceAmount, schedule, coupon, paymentConvention)

.. code-block:: python

    bond = ql.FixedRateBond(2, ql.TARGET(), 100.0, ql.Date(15,12,2019), ql.Date(15,12,2024), ql.Period('1Y'), [0.05], ql.ActualActual(ql.ActualActual.Bond))

AmortizingFixedRateBond
-----------------------

.. function:: ql.AmortizingFixedRateBond(settlementDays, notionals, schedule, coupons, accrualDayCounter, paymentConvention=Following, issueDate=Date())

.. code-block:: python

    notionals = [100,100,100,50]
    schedule = ql.MakeSchedule(ql.Date(25,1,2018), ql.Date(25,1,2022), ql.Period('1y'))
    bond = ql.AmortizingFixedRateBond(0, notionals, schedule, [0.03], ql.Thirty360(ql.Thirty360.USA))
    

FloatingRateBond
----------------

.. function:: ql.FloatingRateBond(settlementDays, faceAmount, schedule, index, dayCounter, paymentConvention)

.. code-block:: python

    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('6m'))
    index = ql.Euribor6M()
    bond = ql.FloatingRateBond(2,100, schedule, index, ql.Actual360(), spreads=[0.01])


AmortizingFloatingRateBond
--------------------------

.. function:: ql.FloatingRateBond(settlementDays, notionals, schedule, index, dayCounter)

.. code-block:: python

    notional = [100, 50]
    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('1Y'))
    index = ql.Euribor6M()
    bond = ql.AmortizingFloatingRateBond(2, notional, schedule, index, ql.ActualActual(ql.ActualActual.Bond))


CMS Rate Bond
-------------

.. function:: ql.CmsRateBond(settlementDays, faceAmount, schedule, index, dayCounter, paymentConvention, fixingDays, gearings, spreads, caps, floors)


.. code-block:: python

    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('1Y'))
    index = ql.EuriborSwapIsdaFixA(ql.Period('10y'))
    bond = ql.CmsRateBond(2, 100, schedule, index, ql.Actual360(), ql.ModifiedFollowing, fixingDays=2, gearings=[1], spreads=[0], caps=[], floors=[])


Callable Bond
-------------

.. function:: ql.CallableFixedRateBond(settlementDays, faceAmount, schedule, coupons, accrualDayCounter, paymentConvention, redemption, issueDate, putCallSchedule)

.. code-block:: python

    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Period('1Y'))
    putCallSchedule = ql.CallabilitySchedule()

    my_price  = ql.BondPrice(100, ql.BondPrice.Clean)

    putCallSchedule.append(
        ql.Callability(my_price, ql.Callability.Call, ql.Date(15,6,2021))
    )

    bond = ql.CallableFixedRateBond(2, 100, schedule, [0.01], ql.Actual360(), ql.ModifiedFollowing, 100, ql.Date(15,6,2020), putCallSchedule)


Convertible Bond
----------------

BondFunctions
-------------


.. code-block:: python

    bond = ql.FixedRateBond(
        2, ql.TARGET(), 100.0,
        ql.Date(15,12,2019), ql.Date(15,12,2024), ql.Period('1Y'),
        [0.05], ql.ActualActual(ql.ActualActual.Bond))

**Date Inspectors**

.. code-block:: python

    ql.BondFunctions.startDate(bond)
    ql.BondFunctions.maturityDate(bond)
    ql.BondFunctions.isTradable(bond)


**Cashflow Inspectors**

.. code-block:: python

    ql.BondFunctions.previousCashFlowDate(bond)
    ql.BondFunctions.previousCashFlowDate(bond, ql.Date(15,12,2020))
    ql.BondFunctions.previousCashFlowAmount(bond)
    ql.BondFunctions.previousCashFlowAmount(bond, ql.Date(15,12,2020))
    ql.BondFunctions.nextCashFlowDate(bond)
    ql.BondFunctions.nextCashFlowDate(bond, ql.Date(15,12,2020))
    ql.BondFunctions.nextCashFlowAmount(bond)
    ql.BondFunctions.nextCashFlowAmount(bond, ql.Date(15,12,2020))


**Coupon Inspectors**

.. code-block:: python

    ql.BondFunctions.previousCouponRate(bond)
    ql.BondFunctions.nextCouponRate(bond)
    ql.BondFunctions.accrualStartDate(bond)
    ql.BondFunctions.accrualEndDate(bond)
    ql.BondFunctions.accrualPeriod(bond)
    ql.BondFunctions.accrualDays(bond)
    ql.BondFunctions.accruedPeriod(bond)
    ql.BondFunctions.accruedDays(bond)
    ql.BondFunctions.accruedAmount(bond)

**YieldTermStructure**    

.. code-block:: python

    crv = ql.FlatForward(2, ql.TARGET(), 0.04, ql.Actual360())
    ql.BondFunctions.cleanPrice(bond, crv)
    ql.BondFunctions.bps(bond, crv)
    ql.BondFunctions.atmRate(bond, crv)

**Yield (a.k.a. Internal Rate of Return, i.e. IRR) functions**

.. code-block:: python

    rate = ql.InterestRate(0.05, ql.Actual360(), ql.Compounded, ql.Annual)
    ql.BondFunctions.cleanPrice(bond, rate)
    ql.BondFunctions.bps(bond, rate)
    ql.BondFunctions.duration(bond, rate)
    ql.BondFunctions.convexity(bond, rate)
    ql.BondFunctions.basisPointValue(bond, rate)
    ql.BondFunctions.yieldValueBasisPoint(bond, rate)

**Z-spread functions**

.. code-block:: python

    crv = ql.FlatForward(2, ql.TARGET(), 0.04, ql.Actual360())
    ql.BondFunctions.zSpread(bond, 101, crv, ql.Actual360(), ql.Compounded, ql.Annual)
