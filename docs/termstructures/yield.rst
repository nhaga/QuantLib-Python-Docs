Yield Term Structures
#####################

FlatForward
***********
Flat interest-rate curve.

.. function:: ql.FlatForward(date, quote, dayCounter, compounding, frequency)

.. function:: ql.FlatForward(integer, Calendar, quote, dayCounter, compounding, frequency)

.. function:: ql.FlatForward(integer, rate, dayCounter)

Examples:

.. code-block:: Python

    ql.FlatForward(ql.Date(15,6,2020), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360(), ql.Compounded, ql.Annual)
    ql.FlatForward(ql.Date(15,6,2020), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360(), ql.Compounded)
    ql.FlatForward(ql.Date(15,6,2020), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360())
    ql.FlatForward(2, ql.TARGET(), ql.QuoteHandle(ql.SimpleQuote(0.05)), ql.Actual360())
    ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())

DiscountCurve
*************
Term structure based on log-linear interpolation of discount factors.

.. function:: ql.DiscountCurve(dates, dfs, dayCounter, cal=ql.NullCalendar())


Example:

.. code-block:: Python

    dates = [ql.Date(7,5,2019), ql.Date(7,5,2020), ql.Date(7,5,2021)]
    dfs = [1, 0.99, 0.98]
    dayCounter = ql.Actual360()
    curve = ql.DiscountCurve(dates, dfs, dayCounter)



ZeroCurve
*********

* ZeroCurve
* LogLinearZeroCurve
* CubicZeroCurve
* NaturalCubicZeroCurve
* LogCubicZeroCurve
* MonotonicCubicZeroCurve

.. function:: ql.ZeroCurve(dates, yields, dayCounter, cal, i, comp, freq)


.. list-table:: 
    :widths: 10 60

    * - Dates
      - The date sequence, the maturity date corresponding to the zero interest rate. Note: The first date must be the base date of the curve, such as a date with a yield of 0.0.
    * - yields
      - a sequence of floating point numbers, zero coupon yield
    * - dayCounter
      - DayCounter object, number of days calculation rule
    * - cal
      - Calendar object, calendar
    * - i
      - Linear object, linear interpolation method
    * - comp and freq
      - are preset integers indicating the way and frequency of payment


.. code-block:: Python

    dates = [ql.Date(31,12,2019),  ql.Date(31,12,2020),  ql.Date(31,12,2021)]
    zeros = [0.01, 0.02, 0.03]

    ql.ZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    ql.LogLinearZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    ql.CubicZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    ql.NaturalCubicZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    ql.LogCubicZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    ql.MonotonicCubicZeroCurve(dates, zeros, ql.ActualActual(), ql.TARGET())
    

ForwardCurve
************
Term structure based on flat interpolation of forward rates.


.. function:: ql.ForwardCurve(dates, rates, dayCounter)

.. function:: ql.ForwardCurve(dates, rates, dayCounter, calendar, BackwardFlat)

.. function:: ql.ForwardCurve(dates, date, rates, rate, dayCounter, calendar)

.. function:: ql.ForwardCurve(dates, date, rates, rate, dayCounter)

.. code-block:: python 

    dates = [ql.Date(15,6,2020), ql.Date(15,6,2022), ql.Date(15,6,2023)]
    rates = [0.02, 0.03, 0.04]
    ql.ForwardCurve(dates, rates, ql.Actual360(), ql.TARGET())
    ql.ForwardCurve(dates, rates, ql.Actual360())


Piecewise
*********

* PiecewiseLogLinearDiscount
* PiecewiseLogCubicDiscount
* PiecewiseLinearZero
* PiecewiseCubicZero
* PiecewiseLinearForward
* PiecewiseSplineCubicDiscount



ImpliedTermStructure
********************

Implied term structure at a given date in the future

.. function:: ql.ImpliedTermStructure(YieldTermStructure, date)

.. code-block:: python

  crv = ql.FlatForward(ql.Date(10,1,2020),0.04875825,ql.Actual365Fixed())
  yts = ql.YieldTermStructureHandle(crv)
  ql.ImpliedTermStructure(yts, ql.Date(20,9,2020))


ForwardSpreadedTermStructure
****************************

Term structure with added spread on the instantaneous forward rate.

.. function:: ql.ForwardSpreadedTermStructure(YieldTermStructure, spread)

.. code-block:: python

  crv = ql.FlatForward(ql.Date(10,1,2020),0.04875825,ql.Actual365Fixed())
  yts = ql.YieldTermStructureHandle(crv)
  spread = ql.QuoteHandle(ql.SimpleQuote(0.005))
  ql.ForwardSpreadedTermStructure(yts, spread)


ZeroSpreadedTermStructure
*************************

Term structure with an added spread on the zero yield rate

.. function:: ql.ZeroSpreadedTermStructure(YieldTermStructure, spread)

.. code-block:: python

  crv = ql.FlatForward(ql.Date(10,1,2020),0.04875825,ql.Actual365Fixed())
  yts = ql.YieldTermStructureHandle(crv)
  spread = ql.QuoteHandle(ql.SimpleQuote(0.005))
  ql.ZeroSpreadedTermStructure(yts, spread)

SpreadedLinearZeroInterpolatedTermStructure
*******************************************

.. function:: ql.SpreadedLinearZeroInterpolatedTermStructure(YieldTermStructure, quotes, dates, compounding, frequency, dayCounter, linear)

.. code-block:: python

  crv = ql.FlatForward(settlement,0.04875825,ql.Actual365Fixed())
  yts = ql.YieldTermStructureHandle(crv)

  calendar = ql.TARGET()
  spread21 = ql.SimpleQuote(0.0050)
  spread22 = ql.SimpleQuote(0.0050)
  startDate = ql.Date().todaysDate()
  endDate = calendar.advance(startDate, ql.Period(50, ql.Years))

  tsSpread = ql.SpreadedLinearZeroInterpolatedTermStructure(
      yts,
      [ql.QuoteHandle(spread21), ql.QuoteHandle(spread22)],
      [startDate, endDate]
  )


FittedBondCurve
***************

.. function:: ql.FittedBondDiscountCurve(bondSettlementDate, helpers, dc, method)

Methods:

- CubicBSplinesFitting
- ExponentialSplinesFitting
- NelsonSiegelFitting
- SimplePolynomialFitting
- SvenssonFitting

.. code-block:: python

  pgbs = pd.DataFrame(
      {'maturity': ['15-06-2020', '15-04-2021', '17-10-2022', '25-10-2023',
                    '15-02-2024', '15-10-2025', '21-07-2026', '14-04-2027',
                    '17-10-2028', '15-06-2029', '15-02-2030', '18-04-2034',
                    '15-04-2037', '15-02-2045'],
      'coupon': [4.8, 3.85, 2.2, 4.95,  5.65, 2.875, 2.875, 4.125,
                  2.125, 1.95, 3.875, 2.25, 4.1, 4.1],
      'px': [102.532, 105.839, 107.247, 119.824, 124.005, 116.215, 117.708,
              128.027, 115.301, 114.261, 133.621, 119.879, 149.427, 159.177]})

  calendar = ql.TARGET()
  today = calendar.adjust(ql.Date(19, 12, 2019))
  ql.Settings.instance().evaluationDate = today

  bondSettlementDays = 2
  bondSettlementDate = calendar.advance(
      today,
      ql.Period(bondSettlementDays, ql.Days))
  frequency = ql.Annual
  dc = ql.ActualActual(ql.ActualActual.ISMA)
  accrualConvention = ql.ModifiedFollowing
  convention = ql.ModifiedFollowing
  redemption = 100.0

  instruments = []
  for idx, row in pgbs.iterrows():
      maturity = ql.Date(row.maturity, '%d-%m-%Y')
      schedule = ql.Schedule(
          bondSettlementDate,
          maturity,
          ql.Period(frequency),
          calendar,
          accrualConvention,
          accrualConvention,
          ql.DateGeneration.Backward,
          False)
      helper = ql.FixedRateBondHelper(
              ql.QuoteHandle(ql.SimpleQuote(row.px)),
              bondSettlementDays,
              100.0,
              schedule,
              [row.coupon / 100],
              dc,
              convention,
              redemption)

      instruments.append(helper)

  params = [bondSettlementDate, instruments, dc]

  cubicNots = [-30.0, -20.0, 0.0, 5.0, 10.0, 15.0,20.0, 25.0, 30.0, 40.0, 50.0]
  fittingMethods = {
      'NelsonSiegelFitting': ql.NelsonSiegelFitting(),
      'SvenssonFitting': ql.SvenssonFitting(),
      'SimplePolynomialFitting': ql.SimplePolynomialFitting(2),
      'ExponentialSplinesFitting': ql.ExponentialSplinesFitting(),
      'CubicBSplinesFitting': ql.CubicBSplinesFitting(cubicNots),
  }

  fittedBondCurveMethods = {
      label: ql.FittedBondDiscountCurve(*params, method)
      for label, method in fittingMethods.items()
  }

  curve = fittedBondCurveMethods.get('NelsonSiegelFitting')

FXImpliedCurve
**************
