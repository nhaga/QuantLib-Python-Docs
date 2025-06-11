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

Piecewise yield term structure. This term structure is bootstrapped on a number of interest rate instruments which are passed as a vector of RateHelper instances. Their maturities mark the boundaries of the interpolated segments.

Each segment is determined sequentially starting from the earliest period to the latest and is chosen so that the instrument whose maturity marks the end of such segment is correctly repriced on the curve.

* PiecewiseLogLinearDiscount
* PiecewiseLogCubicDiscount
* PiecewiseLinearZero
* PiecewiseCubicZero
* PiecewiseLinearForward
* PiecewiseSplineCubicDiscount

.. function:: ql.Piecewise(referenceDate, helpers, dayCounter)

.. code-block:: python

  helpers = []
  helpers.append( ql.DepositRateHelper(0.05, ql.Euribor6M()) )
  helpers.append(
      ql.SwapRateHelper(0.06, ql.EuriborSwapIsdaFixA(ql.Period('1y')))
  )
  curve = ql.PiecewiseLogLinearDiscount(ql.Date(15,6,2020), helpers, ql.Actual360())

.. function:: ql.PiecewiseYieldCurve(referenceDate, instruments, dayCounter, jumps, jumpDate, i=Interpolator(), bootstrap=bootstrap_type() )

.. code-block:: python

  referenceDate = ql.Date(15,6,2020)
  ql.PiecewiseLogLinearDiscount(referenceDate, helpers, ql.ActualActual())

  jumps = [ql.QuoteHandle(ql.SimpleQuote(0.01))]
  ql.PiecewiseLogLinearDiscount(referenceDate, helpers, ql.ActualActual(), jumps)

  jumpDates = [ql.Date(15,9,2020)]
  ql.PiecewiseLogLinearDiscount(referenceDate, helpers, ql.ActualActual(), jumps, jumpDates)

.. code-block:: python

  import pandas as pd
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

  piecewiseMethods = {
      'logLinearDiscount': ql.PiecewiseLogLinearDiscount(*params),
      'logCubicDiscount': ql.PiecewiseLogCubicDiscount(*params),
      'linearZero': ql.PiecewiseLinearZero(*params),
      'cubicZero': ql.PiecewiseCubicZero(*params),
      'linearForward': ql.PiecewiseLinearForward(*params),
      'splineCubicDiscount': ql.PiecewiseSplineCubicDiscount(*params),
  }


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

PiecewiseZeroSpreadedTermStructure
**********************************

Represents a yield term structure constructed by applying a piecewise-linear interpolation of zero-rate spreads to an existing base curve. The resulting zero rate at any date is the base curve's zero rate plus the interpolated spread at that date.

This structure is useful when modeling a market-implied yield curve that deviates from a base curve by a known set of spreads at given dates.

Other interpolations:

* **SpreadedLinearZeroInterpolatedTermStructure** (alias for PiecewiseZeroSpreadedTermStructure)
* **SpreadedCubicZeroInterpolatedTermStructure**
* **SpreadedKrugerZeroInterpolatedTermStructure**
* **SpreadedSplineCubicZeroInterpolatedTermStructure**
* **SpreadedParabolicCubicZeroInterpolatedTermStructure**
* **SpreadedMonotonicParabolicCubicZeroInterpolatedTermStructure**


.. function:: ql.PiecewiseZeroSpreadedTermStructure(baseCurve: ql.YieldTermStructureHandle, spreads: List[ql.Handle], dates: List[ql.Date], compounding: ql.Compounding = ql.Continuous, freq: ql.Frequency = ql.NoFrequency, dc: ql.DayCounter)
	
	:param baseCurve: The base yield term structure to which zero-rate spreads are applied.
	:type baseCurve: ql.YieldTermStructureHandle

	:param spreads: A list of handles to quotes representing the zero-rate spreads.
	:type spreads: List[ql.Handle]

	:param dates: The dates corresponding to each spread value. Must be in strictly increasing order.
	:type dates: List[ql.Date]

	:param compounding: The compounding method used for zero rates. Defaults to ql.Continuous.
	:type compounding: ql.Compounding, optional

	:param freq: The frequency of compounding. Only relevant if compounding is not continuous. Defaults to ql.NoFrequency.
	:type freq: ql.Frequency, optional

	:param dc: The day count convention used for year fractions.
	:type dc: ql.DayCounter, optional
   

.. code-block:: python

	calendar = ql.TARGET()
	today = ql.Date(9, 6, 2009)
	ql.Settings.instance().evaluationDate = today
	day_count = ql.Actual360()
	compounding = ql.Continuous

	# Build base term structure
	settlement_days = 2
	settlement_date = calendar.advance(today, ql.Period(settlement_days, ql.Days))
	ts_days = [13, 41, 75, 165, 256, 345, 524, 703]
	rates = [0.035, 0.033, 0.034, 0.034, 0.036, 0.037, 0.039, 0.040]
	dates = [settlement_date] + [calendar.advance(today, n, ql.Days) for n in ts_days]
	curve_rates = [0.035] + rates
	term_structure = ql.ZeroCurve(dates, curve_rates, day_count)

	# Spreads and spread dates
	spread_1 = ql.makeQuoteHandle(0.02)
	spread_2 = ql.makeQuoteHandle(0.03)
	spreads = [spread_1, spread_2]

	spread_dates = [
		calendar.advance(today, 8, ql.Months),
		calendar.advance(today, 15, ql.Months)
	]

	# PiecewiseZeroSpreadedTermStructure
	spreaded_term_structure = ql.PiecewiseZeroSpreadedTermStructure(
		ql.YieldTermStructureHandle(term_structure),
		spreads, spread_dates
	)

	interpolation_date = calendar.advance(today, 6, ql.Months)
	t = day_count.yearFraction(today, interpolation_date)
	interpolated_zero_rate = spreaded_term_structure.zeroRate(t, compounding).rate()

PiecewiseLinearForwardSpreadedTermStructure
*******************************************

Represents a yield term structure constructed by applying a piecewise-linear interpolation of **forward-rate** spreads to an existing base curve.
The resulting forward rate at any date is the base curve's forward rate plus the interpolated spread at that date.

This structure is useful when modeling market-implied forward curves that deviate from a base term structure by a known set of spreads at given dates.

Other interpolations:

* **PiecewiseForwardSpreadedTermStructure** (Backward-flat interpolated)

.. function:: ql.PiecewiseLinearForwardSpreadedTermStructure(baseCurve: ql.YieldTermStructureHandle, spreads: List[ql.Handle], dates: List[ql.Date], dc: ql.DayCounter)

	:param baseCurve: The base yield term structure to which forward-rate spreads are applied.
	:type baseCurve: ql.YieldTermStructureHandle

	:param spreads: A list of handles to quotes representing the forward-rate spreads.
	:type spreads: List[ql.Handle]

	:param dates: The dates corresponding to each spread value. Must be in strictly increasing order.
	:type dates: List[ql.Date]

	:param dc: The day count convention used for computing year fractions.
	:type dc: ql.DayCounter, optional

.. note::

Unlike the zero-spreaded structure, this one applies spreads to **instantaneous forward rates**, not zero yields. Therefore, the impact on discount factors and derived instruments may differ.

.. code-block:: python

	today = ql.Date(10, ql.January, 2024)
	ql.Settings.instance().evaluationDate = today

	# Define forward curve dates and rates (annualized, continuous compounding)
	dates = [
		today,
		today + ql.Period(3, ql.Months),
		today + ql.Period(6, ql.Months),
		today + ql.Period(1, ql.Years),
		today + ql.Period(2, ql.Years),
		today + ql.Period(3, ql.Years),
		today + ql.Period(5, ql.Years),
		today + ql.Period(10, ql.Years)
	]
	forwards = [0.02, 0.021, 0.022, 0.023, 0.025, 0.025, 0.023, 0.022]

	# Build the forward curve
	calendar = ql.TARGET()
	day_count = ql.Actual365Fixed()
	forward_curve = ql.ForwardCurve(dates, forwards, day_count, calendar)
	fwd_crv_handle = ql.YieldTermStructureHandle(forward_curve)
	
	spreads = [ql.makeQuoteHandle(0.00), ql.makeQuoteHandle(0.005), ql.makeQuoteHandle(0.0025), ql.makeQuoteHandle(0.0)]
	spread_dates = [ today,
					calendar.advance(today, ql.Period(3, ql.Years)), 
					calendar.advance(today, ql.Period(5, ql.Years)), 
					calendar.advance(today, ql.Period(10, ql.Years))]
					
	spreaded_fwd_crv = ql.PiecewiseLinearForwardSpreadedTermStructure(fwd_crv_handle, spreads, spread_dates, day_count)


FittedBondCurve
***************

.. function:: ql.FittedBondDiscountCurve(bondSettlementDate, helpers, dc, method,  accuracy=1.0e-10, maxEvaluations=10000, guess=Array(), simplexLambda=1.0 )

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
