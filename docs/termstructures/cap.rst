Cap Volatility
##############


ConstantOptionletVolatility
***************************

floating reference date, floating market data

.. function:: ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility (Quote), dc, type=ShiftedLognormal, displacement=0.0)

fixed reference date, floating market data

.. function:: ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility (Quote), dc, type=ShiftedLognormal, displacement=0.0)

floating reference date, fixed market data

.. function:: ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility (value), dc, type=ShiftedLognormal, displacement=0.0)

fixed reference date, fixed market data

.. function:: ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility (value), dc, type=ShiftedLognormal, displacement=0.0)


.. code-block:: python

  settlementDays = 2
  settlementDate = ql.Date().todaysDate()
  cal = ql.TARGET()
  bdc = ql.ModifiedFollowing
  volatility = 0.55
  vol_quote = ql.QuoteHandle(ql.SimpleQuote(volatility))
  dc = ql.Actual365Fixed()

  #floating reference date, floating market data
  c1 = ql.ConstantOptionletVolatility(settlementDays, cal, bdc, vol_quote, dc, ql.Normal)

  #fixed reference date, floating market data
  c2 = ql.ConstantOptionletVolatility(settlementDate, cal, bdc, vol_quote, dc)

  #floating reference date, fixed market data
  c3 = ql.ConstantOptionletVolatility(settlementDays, cal, bdc, volatility, dc)

  #fixed reference date, fixed market data
  c4 = ql.ConstantOptionletVolatility(settlementDate, cal, bdc, volatility, dc)



CapFloorTermVolCurve
********************

Cap/floor at-the-money term-volatility vector.


**floating reference date, floating market data**

.. function:: ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols (Quotes), dc=Actual365Fixed)

**fixed reference date, floating market data**

.. function:: ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols (Quotes), dc=Actual365Fixed)

**fixed reference date, fixed market data**

.. function:: ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols (vector), dc=Actual365Fixed)

**floating reference date, fixed market data**

.. function:: ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols (vector), dc=Actual365Fixed)


.. code-block:: python

  settlementDate = ql.Date().todaysDate()
  settlementDays = 2
  calendar = ql.TARGET()
  bdc = ql.ModifiedFollowing
  optionTenors  = [ql.Period('1y'), ql.Period('2y'), ql.Period('3y')]
  vols = [0.55, 0.60, 0.65]

  # fixed reference date, fixed market data
  c3 = ql.CapFloorTermVolCurve(settlementDate, calendar, bdc, optionTenors, vols)

  # floating reference date, fixed market data
  c4 = ql.CapFloorTermVolCurve(settlementDays, calendar, bdc, optionTenors, vols)


CapFloorTermVolSurface
**********************


**floating reference date, floating market data**

.. function:: ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, vol_data (Handle), daycount=ql.Actual365Fixed)

**fixed reference date, floating market data**

.. function:: ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, vol_data (Handle), daycount=ql.Actual365Fixed)

**fixed reference date, fixed market data**

.. function:: ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, vol_data (Matrix), daycount=ql.Actual365Fixed)

**floating reference date, fixed market data**

.. function:: ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, vol_data (Matrix), daycount=ql.Actual365Fixed)


.. code-block:: python

  settlementDate = ql.Date().todaysDate()
  settlementDays = 2
  calendar = ql.TARGET()
  bdc = ql.ModifiedFollowing
  expiries  = [ql.Period('9y'), ql.Period('10y'), ql.Period('12y')]
  strikes = [0.015, 0.02, 0.025]

  black_vols = [
      [1.    , 0.792 , 0.6873],
      [0.9301, 0.7401, 0.6403],
      [0.7926, 0.6424, 0.5602]]


  # fixed reference date, fixed market data
  s3 = ql.CapFloorTermVolSurface(settlementDate, calendar, bdc, expiries, strikes, black_vols)

  # floating reference date, fixed market data
  s4 = ql.CapFloorTermVolSurface(settlementDays, calendar, bdc, expiries, strikes, black_vols)


OptionletStripper1
******************

.. function:: ql.OptionletStripper1(CapFloorTermVolSurface, index, switchStrikes=Null, accuracy=1.0e-6, maxIter=100, discount=YieldTermStructure, type=ShiftedLognormal, displacement=0.0, dontThrow=false)

.. code-block:: python

  index = ql.Euribor6M()
  optionlet_surf = ql.OptionletStripper1(s3, index, type=ql.Normal)


StrippedOptionletAdapter
************************

.. function:: ql.StrippedOptionletAdapter(StrippedOptionletBase)

OptionletVolatilityStructureHandle
**********************************

.. function:: ql.OptionletVolatilityStructureHandle(OptionletVolatilityStructure)

.. code-block:: python

  ovs_handle = ql.OptionletVolatilityStructureHandle(
      ql.StrippedOptionletAdapter(optionlet_surf)
  )


RelinkableOptionletVolatilityStructureHandle
********************************************

.. function:: ql.RelinkableOptionletVolatilityStructureHandle()

.. code-block:: python

  ovs_handle = ql.RelinkableOptionletVolatilityStructureHandle()
  ovs_handle.linkTo(ql.StrippedOptionletAdapter(optionlet_surf))



