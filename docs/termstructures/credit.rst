Credit Term Structures
######################

FlatHazardRate
**************

Flat hazard-rate curve.

.. function:: ql.FlatHazardRate(settlementDays, calendar, Quote, dayCounter)

.. function:: ql.FlatHazardRate(settelementDate, Quote, dayCounter)

.. code-block:: python

  pd_curve = ql.FlatHazardRate(2, ql.TARGET(), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360())
  pd_curve = ql.FlatHazardRate(ql.Date().todaysDate(), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360())

PiecewiseFlatHazardRate
***********************

Piecewise default-probability term structure.

.. function:: ql.PiecewiseFlatHazardRate(settlementDate, helpers, dayCounter)

.. code-block:: python

  recoveryRate = 0.4
  settlementDate = ql.Date().todaysDate()
  yts = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())

  CDS_tenors = [ql.Period(6, ql.Months), ql.Period(1, ql.Years), ql.Period(2, ql.Years), ql.Period(3, ql.Years), \
      ql.Period(4, ql.Years), ql.Period(5, ql.Years), ql.Period(7, ql.Years), ql.Period(10, ql.Years), ql.Period(50, ql.Years)]
  CDS_ctpy = [26.65, 37.22, 53.17, 65.79, 77.39, 91.14, 116.84, 136.67, 136.67]

  CDSHelpers_ctpy = [ql.SpreadCdsHelper((CDS_spread / 10000.0), CDS_tenor, 0, ql.TARGET(), ql.Quarterly, ql.Following, \
      ql.DateGeneration.TwentiethIMM, ql.Actual360(), recoveryRate, ql.YieldTermStructureHandle(yts))
  for CDS_spread, CDS_tenor in zip(CDS_ctpy, CDS_tenors)] 

  pd_curve = ql.PiecewiseFlatHazardRate(settlementDate, CDSHelpers_ctpy, ql.Thirty360()) 


SurvivalProbabilityCurve
************************

.. function:: ql.SurvivalProbabilityCurve(dates, survivalProbabilities, dayCounter, calendar)

.. code-block:: python

  today = ql.Date().todaysDate()
  dates = [today + ql.Period(n , ql.Years) for n in range(11)]
  sp = [1.0, 0.9941, 0.9826, 0.9674, 0.9488, 0.9246, 0.8945, 0.8645, 0.83484, 0.80614, 0.7784]
  crv = ql.SurvivalProbabilityCurve(dates, sp, ql.Actual360(), ql.TARGET())
  crv.enableExtrapolation()

