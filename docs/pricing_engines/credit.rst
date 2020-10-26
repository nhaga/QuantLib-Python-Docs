Credit Pricing Engines
######################

IsdaCdsEngine
*************

.. function:: ql.IsdaCdsEngine(defaultProbability, recoveryRate, yieldTermStructure, includeSettlementDateFlows=None, numericalFix=ql.IsdaCdsEngine.Taylor, AccrualBias accrualBias=ql.IsdaCdsEngine.HalfDayBias, forwardsInCouponPeriod=ql.IsdaCdsEngine.Piecewise)

.. code-block:: python

    today = ql.Date().todaysDate()
    defaultProbability = ql.DefaultProbabilityTermStructureHandle(
        ql.FlatHazardRate(today, ql.QuoteHandle(ql.SimpleQuote(0.01)), ql.Actual360())
    )
    yieldTermStructure = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual360()))

    recoveryRate = 0.4
    engine = ql.IsdaCdsEngine(defaultProbability, recoveryRate, yieldTermStructure)

MidPointCdsEngine
*****************

.. function:: ql.MidPointCdsEngine(defaultProbability, recoveryRate, yieldTermStructure)

.. code-block:: python

    today = ql.Date().todaysDate()
    defaultProbability = ql.DefaultProbabilityTermStructureHandle(
        ql.FlatHazardRate(today, ql.QuoteHandle(ql.SimpleQuote(0.01)), ql.Actual360())
    )
    yieldTermStructure = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual360()))

    recoveryRate = 0.4
    engine = ql.MidPointCdsEngine(defaultProbability, recoveryRate, yieldTermStructure)

IntegralCdsEngine
*****************

.. function:: ql.IntegralCdsEngine(integrationStep, probability, recoveryRate, discountCurve, includeSettlementDateFlows=False)

.. code-block:: python

    today = ql.Date().todaysDate()
    defaultProbability = ql.DefaultProbabilityTermStructureHandle(
        ql.FlatHazardRate(today, ql.QuoteHandle(ql.SimpleQuote(0.01)), ql.Actual360())
    )
    yieldTermStructure = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual360()))

    integralStep = ql.Period('1d')
    engine = ql.IntegralCdsEngine(integralStep, defaultProbability, 0.4, yieldTermStructure, includeSettlementDateFlows=False)

BlackCdsOptionEngine
********************

.. function:: ql.BlackCdsOptionEngine(defaultProbability, recoveryRate, yieldTermStructure, vol)

.. code-block:: python

    today = ql.Date().todaysDate()
    defaultProbability = ql.DefaultProbabilityTermStructureHandle(
        ql.FlatHazardRate(today, ql.QuoteHandle(ql.SimpleQuote(0.01)), ql.Actual360())
    )
    yieldTermStructure = ql.YieldTermStructureHandle(ql.FlatForward(today, 0.05, ql.Actual360()))


    vol = ql.QuoteHandle(ql.SimpleQuote(0.2))
    engine = ql.BlackCdsOptionEngine(defaultProbability, 0.4, yieldTermStructure, vol)
