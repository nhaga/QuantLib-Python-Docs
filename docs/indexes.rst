#######
Indexes
#######

QuantLib provides a set of classes that represent various types of Indexes. 

The available classes under the **Interest Rate Indexes** are:

* ``IborIndex``
* ``OvernightIndex``
* ``SpreadIndex``
* ``SwapSpreadIndex``

The available classes under the **Inflation Indexes** are:

* ``InflationIndex``
* ``ZeroInflationIndex``
* ``YoYInflationIndex``

The class that defines that main interface for all the following classes is the purely abstract ``Index`` class 

Index
*****

.. class:: Index

    The Index class defines the following methods that every subclass inherits:

    .. method:: name()

        Returns the name of the index.

        :return: The name of the index.
        :rtype: str

    .. method:: fixingCalendar()

        Returns the calendar defining valid fixing dates for the index.

        :return: The fixing calendar.
        :rtype: ql.Calendar

    .. method:: isValidFixingDate(fixingDate: ql.Date)

        Checks if the given date is a valid fixing date for the index.

        :param fixingDate: The date to check.
        :type fixingDate: ql.Date
        :return: True if the date is a valid fixing date, False otherwise.
        :rtype: bool

    .. method:: hasHistoricalFixing(fixingDate: ql.Date)

        Returns whether a historical fixing was stored for the given date.

        :param fixingDate: The date to check.
        :type fixingDate: ql.Date
        :return: True if a historical fixing exists for the date, False otherwise.
        :rtype: bool

    .. method:: fixing(fixingDate: ql.Date, forecastTodaysFixing: bool = False)

        Returns the fixing for the given date.

        :param fixingDate: The date for which the fixing is requested.
        :type fixingDate: ql.Date
        :param forecastTodaysFixing: If True, today's fixing is forecasted instead of retrieved from history.
        :type forecastTodaysFixing: bool
        :return: The fixing value.
        :rtype: float

    .. method:: pastFixing(fixingDate: ql.Date)

        Returns a past fixing for the given date.

        :param fixingDate: The date for which the past fixing is requested.
        :type fixingDate: ql.Date
        :return: The past fixing value.
        :rtype: float

    .. method:: timeSeries()

        Returns the time series of historical fixings for the index.

        :return: The time series of fixings.
        :rtype: ql.TimeSeries

    .. method:: allowsNativeFixings()

        Returns whether the index allows for native fixings.

        :return: True if native fixings are allowed, False otherwise.
        :rtype: bool

    .. method:: addFixing(fixingDate: ql.Date, fixing: float, forceOverwrite: bool = False)

        Stores a historical fixing at the given date.

        :param fixingDate: The date of the fixing.
        :type fixingDate: ql.Date
        :param fixing: The fixing value.
        :type fixing: float
        :param forceOverwrite: If True, overwrites any existing fixing for the date.
        :type forceOverwrite: bool

    .. method:: addFixings(timeSeries: ql.TimeSeries, forceOverwrite: bool = False)

        Stores historical fixings from a time series.

        :param timeSeries: The time series of fixings to add.
        :type timeSeries: ql.TimeSeries
        :param forceOverwrite: If True, overwrites any existing fixings for the dates.
        :type forceOverwrite: bool

    .. method:: clearFixings()

        Clears all stored historical fixings for the index.

IndexManager
************

To avoid discrepancies between the indexes themselves QuantLib employes a unique global repository for the various registered indexes under the ``IndexManager`` class.
``IndexManager`` basically stores for each index added a timeseries of the past fixings.

The public methods that ``IndexManager`` exposes are:


    .. method:: histories()

        Returns a list of all index names for which fixings have been stored.

        :return: List of index names.
        :rtype: List[str]

    .. method:: clearHistories()

        Clears all stored fixings for all indexes.

The ``IndexManager`` instance can be accessed thought: 

.. code-block:: python

    ql.IndexManager.instance()

-----


Interest Rate
*************

In the following block there are going to be listed all the classes that are subclasses of the ``InterestRateIndex`` class.
``InterestRateIndex`` class itself if a child class of the ``Index`` class and serves as the abstract base for all interest rate indexes in QuantLib, including IBOR and overnight indexes. 

.. class:: InterestRateIndex

    Base class for interest rate indexes.

    This class extends :class:`Index` and provides the common interface for all interest rate indexes in QuantLib, such as Ibor and Overnight indexes. It is not meant to be instantiated directly, but provides additional methods for tenor, currency, day count, and date calculations.

    **Additional Methods**

    .. method:: familyName()

        Returns the family name of the index.

        :return: The family name.
        :rtype: str

    .. method:: tenor()

        Returns the tenor (e.g., 3M, 6M) of the index.

        :return: The tenor.
        :rtype: ql.Period

    .. method:: fixingDays()

        Returns the number of fixing days for the index.

        :return: The number of fixing days.
        :rtype: int

    .. method:: currency()

        Returns the currency of the index.

        :return: The currency.
        :rtype: ql.Currency

    .. method:: dayCounter()

        Returns the day count convention used by the index.

        :return: The day counter.
        :rtype: ql.DayCounter

    .. method:: fixingDate(valueDate: ql.Date)

        Returns the fixing date corresponding to a given value date.

        :param valueDate: The value date.
        :type valueDate: ql.Date
        :return: The fixing date.
        :rtype: ql.Date

    .. method:: valueDate(fixingDate: ql.Date)

        Returns the value date corresponding to a given fixing date.

        :param fixingDate: The fixing date.
        :type fixingDate: ql.Date
        :return: The value date.
        :rtype: ql.Date

    .. method:: maturityDate(valueDate: ql.Date)

        Returns the maturity date corresponding to a given value date.

        :param valueDate: The value date.
        :type valueDate: ql.Date
        :return: The maturity date.
        :rtype: ql.Date

    .. method:: forecastFixing(fixingDate: ql.Date)

        Returns the forecasted fixing for the given fixing date.

        :param fixingDate: The fixing date.
        :type fixingDate: ql.Date
        :return: The forecasted fixing value.
        :rtype: float

IborIndex
---------

.. class:: IborIndex(familyName: str, tenor: ql.Period, settlementDays: int, currency: ql.Currency, fixingCalendar: ql.Calendar, convention: ql.Convention, endOfMonth: bool, dayCounter: ql.DayCounter, h: ql.YieldTermStructureHandle = ql.YieldTermStructureHandle())

    Base class for Interbank Offered Rate (IBOR) indexes.

    :param familyName: The name of the index family (e.g., "Euribor", "Libor").
    :type familyName: str
    :param tenor: The tenor of the index (e.g., 3M, 6M).
    :type tenor: ql.Period
    :param settlementDays: Number of settlement days.
    :type settlementDays: int
    :param currency: The currency of the index.
    :type currency: ql.Currency
    :param fixingCalendar: The calendar used for fixing dates.
    :type fixingCalendar: ql.Calendar
    :param convention: The business day convention for the index.
    :type convention: ql.Convention
    :param endOfMonth: Whether end-of-month adjustment is used.
    :type endOfMonth: bool
    :param dayCounter: The day count convention for interest calculation.
    :type dayCounter: ql.DayCounter
    :param h: (Optional) The yield term structure handle used for forecasting fixings.
    :type h: Optional[ql.YieldTermStructureHandle]

    :returns: An instance of IborIndex.
    :rtype: ql.IborIndex

    **Example:**

.. code-block:: python

    ql.IborIndex('MyIndex', ql.Period('6m'), 2, ql.EURCurrency(), ql.TARGET(), ql.ModifiedFollowing, True, ql.Actual360())
    ql.Libor('MyIndex', ql.Period('6M'), 2, ql.USDCurrency(), ql.TARGET(), ql.Actual360())
    ql.Euribor(ql.Period('6M'))        
    ql.USDLibor(ql.Period('6M'))
    ql.Euribor6M()

The most notable derived classes are: 

- ``ql.Euribor()``
- ``ql.Euribor1M()``
- ``ql.Euribor3M()``
- ``ql.Euribor6M()``
- ``ql.GBPLibor()``
- ``ql.USDLibor()``
- ``ql.CHFLibor()``

The ``IborIndex`` other subclasses can be found under `ql/indexes/ibor` (in QuantLib C++ Library).


Constructors for derived classes:

.. class:: Euribor(tenor: ql.Period)

.. class:: Euribor(tenor: ql.Period, yts: ql.YieldTermStructureHandle)

While for Fixed Tenor classes (like ``ql.Euribor3M``) the constructor is the following

.. class:: Euribor6M(yts: ql.YieldTermStructureHandle)

From QuantLib 1.39 the class ``CustomIborIndex`` will be available, which lets you define a LIBOR-like index that allows specifying custom calendars for value and maturity date calculations.

.. class:: CustomIborIndex(familyName: str, tenor: ql.Period, settlementDays: int, currency: ql.Currency, fixingCalendar: ql.Calendar, valueCalendar: ql.Calendar, maturityCalendar: ql.Calendar, convention: ql.BusinessDayConvention, endOfMonth: bool, dayCounter: ql.DayCounter, h: Optional[ql.YieldTermStructureHandle] = None)

    Typical LIBOR indexes use:
      - fixingCalendar = valueCalendar = UK, maturityCalendar = JoinHolidays(UK, CurrencyCalendar) for non-EUR currencies.
      - fixingCalendar = JoinHolidays(UK, TARGET), valueCalendar = maturityCalendar = TARGET for EUR.

    :param familyName: The name of the index family (e.g., "USD-LIBOR", "EURIBOR").
    :type familyName: str
    :param tenor: The tenor of the index (e.g., 3M, 6M).
    :type tenor: ql.Period
    :param settlementDays: The number of settlement days for the index.
    :type settlementDays: int
    :param currency: The currency of the index.
    :type currency: ql.Currency
    :param fixingCalendar: The calendar used for fixing dates.
    :type fixingCalendar: ql.Calendar
    :param valueCalendar: The calendar used for value date calculations.
    :type valueCalendar: ql.Calendar
    :param maturityCalendar: The calendar used for maturity date calculations.
    :type maturityCalendar: ql.Calendar
    :param convention: The business day convention for date adjustments.
    :type convention: ql.BusinessDayConvention
    :param endOfMonth: Whether end-of-month adjustment is used.
    :type endOfMonth: bool
    :param dayCounter: The day count convention used for interest calculation.
    :type dayCounter: ql.DayCounter
    :param h: (Optional) The yield term structure used to forecast fixings. If not provided, it can be linked later.
    :type h: Optional[ql.YieldTermStructureHandle]

    The ``CustomIborIndex`` expose the following methods:

    .. method:: valueDate(fixingDate: ql.Date)

        Advances the given ``fixingDate`` on the valueCalendar and adjusts on the maturityCalendar.

        :return: The new adjusted date.
        :rtype: ql.Date

    .. method:: maturityDate(valueDate: ql.Date)

        Advances the given ``valueDate`` on the maturityCalendar.

        :return: The new adjusted date.
        :rtype: ql.Date

    .. method:: fixingDate(valueDate: float)

        Draw back the given ``fixingDate`` minus the ``settlementDays`` on the valueCalendar.

        :return: The new adjusted date.
        :rtype: ql.Date

OvernightIndex
--------------

.. class:: OvernightIndex(familyName: str, settlementDays: int, currency: ql,Currency, fixingCalendar: ql.Calendar, dayCounter: ql.DayCounter, h:Optional[ql.YieldTermStructureHandle])

    Base class for overnight interbank offered rate indexes (e.g., ESTR, SOFR, SONIA).

    :param familyName: The name of the index family (e.g., "EONIA", "FedFunds", "SONIA").
    :type familyName: str
    :param settlementDays: The number of settlement days for the index.
    :type settlementDays: int
    :param currency: The currency of the index.
    :type currency: ql.Currency
    :param fixingCalendar: The calendar used for fixing dates.
    :type fixingCalendar: ql.Calendar
    :param dayCounter: The day count convention used for interest calculation.
    :type dayCounter: ql.DayCounter
    :param yieldTermStructure: (Optional) The yield term structure used to forecast fixings. If not provided, it can be linked later.
    :type yieldTermStructure: Optional[ql.YieldTermStructureHandle]

    **Example**

.. code-block:: python

    name = 'CNYRepo7D'
    fixingDays = 1
    currency = ql.CNYCurrency()
    calendar = ql.China()
    dayCounter = ql.Actual365Fixed()
    overnight_index = ql.OvernightIndex(name, fixingDays, currency, calendar, dayCounter)

The most notable derived classes are: 

- ``ql.Estr()``
- ``ql.Sofr()``
- ``ql.Sonia()``
- ``ql.Saron()``
- ``ql.Aonia()``
- ``ql.Corra()``
- ``ql.Kofr()``

The ``OvernightIndex`` other subclasses can be found under `ql/indexes/ibor` (in QuantLib C++ Library).

-----


SwapIndex
---------

.. class:: SwapIndex(familyName: str, tenor: ql.Period, settlementDays: int, currency: ql.Currency, fixingCalendar: ql.Calendar, fixedLegTenor: ql.Period, fixedLegConvention: ql.BusinessDayConvention, fixedLegDayCounter: ql.DayCounter, index: ql.IborIndex)

    Main constructor for SwapIndex.

    :param familyName: The name of the swap index family (e.g., "EuriborSwapIsdaFixA").
    :type familyName: str
    :param tenor: The tenor of the swap (e.g., 5Y, 10Y).
    :type tenor: ql.Period
    :param settlementDays: Number of settlement days for the swap.
    :type settlementDays: int
    :param currency: The currency of the swap.
    :type currency: ql.Currency
    :param fixingCalendar: The calendar used for fixing dates.
    :type fixingCalendar: ql.Calendar
    :param fixedLegTenor: The tenor of the fixed leg payments (e.g., 1Y).
    :type fixedLegTenor: ql.Period
    :param fixedLegConvention: The business day convention for the fixed leg.
    :type convention: ql.BusinessDayConvention
    :param fixedLegDayCounter: The day count convention for the fixed leg.
    :type dayCounter: ql.DayCounter
    :param index: The floating leg Ibor index.
    :type index: ql.IborIndex

.. class:: SwapIndex(familyName: str, tenor: ql.Period, settlementDays: int, currency: ql.Currency, fixingCalendar: ql.Calendar, fixedLegTenor: ql.Period, fixedLegConvention: ql.BusinessDayConvention, fixedLegDayCounter: ql.DayCounter, index: ql.IborIndex, discountingTermStructure: ql.YieldTermStructureHandle)

    Alternate constructor with explicit discounting term structure.

    :param familyName: The name of the swap index family (e.g., "EuriborSwapIsdaFixA").
    :type familyName: str
    :param tenor: The tenor of the swap (e.g., 5Y, 10Y).
    :type tenor: ql.Period
    :param settlementDays: Number of settlement days for the swap.
    :type settlementDays: int
    :param currency: The currency of the swap.
    :type currency: ql.Currency
    :param fixingCalendar: The calendar used for fixing dates.
    :type fixingCalendar: ql.Calendar
    :param fixedLegTenor: The tenor of the fixed leg payments (e.g., 1Y).
    :type fixedLegTenor: ql.Period
    :param fixedLegConvention: The business day convention for the fixed leg.
    :type convention: ql.BusinessDayConvention
    :param fixedLegDayCounter: The day count convention for the fixed leg.
    :type dayCounter: ql.DayCounter
    :param index: The floating leg Ibor index.
    :type index: ql.IborIndex
    :param discountingTermStructure: The yield term structure used for discounting.
    :type discountingTermStructure: ql.YieldTermStructureHandle

    .. method:: underlyingSwap(fixingDate: ql.Date)

        returns a ``ql.Swap`` (either a ``VanillaSwap`` or an ``OvernightIndexedSwap``) that represents the underlying swap for the given fixing date.

        :param fixingDate: The given fixingDate
        :type familyName: ql.Date
        :return: The new adjusted date.
        :rtype: ql.Date

Derived Classes: 

- ``ql.ChfLiborSwapIsdaFix``
- ``ql.EuriborSwapIsdaFixA``
- ``ql.EuriborSwapIsdaFixB``
- ``ql.EuriborSwapIfrFix``
- ``ql.EurLiborSwapIfrFix``
- ``ql.EurLiborSwapIsdaFixA``
- ``ql.EurLiborSwapIsdaFixB``
- ``ql.GbpLiborSwapIsdaFix``
- ``ql.JpyLiborSwapIsdaFixAm``
- ``ql.JpyLiborSwapIsdaFixPm``
- ``ql.OvernightIndexedSwapIndex``
- ``ql.UsdLiborSwapIsdaFixAm``
- ``ql.UsdLiborSwapIsdaFixPm``


Constructors for derived classes:

.. class:: ql.EuriborSwapIsdaFixA(period: ql.Period)

.. class:: ql.EuriborSwapIsdaFixA(period: ql.Period, yts: ql.YieldTermStructureHandle)

.. class:: ql.EuriborSwapIsdaFixA(period: ql.Period, forward_yts: ql.YieldTermStructureHandle, discounting_yts: ql.YieldTermStructureHandle)

-----


SwapSpreadIndex
---------------

.. class:: SwapSpreadIndex(familyName: str, swapIndex1: ql.SwapIndex, swapIndex2: ql.SwapIndex, gearing1: float = 1.0, gearing2: float = -1.0)

    Constructor for swap-rate spread indexes objects

    :param familyName: The name of the swap spread index family (e.g., "EuriborSwapSpread").
    :type familyName: str
    :param swapIndex1: The first swap index in the spread.
    :type swapIndex1: ql.SwapIndex
    :param swapIndex2: The second swap index in the spread.
    :type swapIndex2: ql.SwapIndex
    :param gearing1: The multiplier applied to the first swap index (default is 1.0).
    :type gearing1: float
    :param gearing2: The multiplier applied to the second swap index (default is -1.0).
    :type gearing2: float

    **Example**:

.. code-block:: python

    cms10y = ql.EuriborSwapIsdaFixA(ql.Period(10, ql.Years), for_yts, disc_yts)
    cms2y = ql.EuriborSwapIsdaFixA(ql.Period(2, ql.Years), for_yts, disc_yts)
    cms10y2y = ql.SwapSpreadIndex("cms10y2y", cms10y, cms2y)

    cms10y.addFixing(refDate, 0.05)

Inflation
*********

.. class:: InflationIndex(familyName: str, region: ql.Region, revised: bool, frequency: ql.Frequency, availabilityLag: ql.Period, currency: ql.Currency)

    Base class for inflation-rate index

    :param familyName: The name of the inflation index family (e.g., "CPI", "HICP").
    :type familyName: str
    :param region: The geographical region for which the index is published.
    :type region: ql.Region
    :param revised: Whether the index can be revised after publication.
    :type revised: bool
    :param frequency: The frequency with which the index is published (e.g., Monthly, Quarterly).
    :type frequency: ql.Frequency
    :param availabilityLag: The lag between the reference period and the publication date.
    :type availabilityLag: ql.Period
    :param currency: The currency in which the index is quoted.
    :type currency: ql.Currency

Zero Inflation
--------------

.. class:: ZeroInflationIndex(familyName: str, region: ql.Region, revised: bool, frequency: ql.Frequency, availabilityLag: ql.Period, currency: ql.Currency, h: Optional[ql.ZeroInflationTermStructureHandle])

    Base class for zero inflation indices.

    :param familyName: The name of the zero inflation index family (e.g., "CPI", "HICP").
    :type familyName: str
    :param region: The geographical region for which the index is published.
    :type region: ql.Region
    :param revised: Whether the index can be revised after publication.
    :type revised: bool
    :param frequency: The frequency with which the index is published (e.g., Monthly, Quarterly).
    :type frequency: ql.Frequency
    :param availabilityLag: The lag between the reference period and the publication date.
    :type availabilityLag: ql.Period
    :param currency: The currency in which the index is quoted.
    :type currency: ql.Currency
    :param h: (Optional) The zero inflation term structure handle used for forecasting.
    :type h: Optional[ql.ZeroInflationTermStructureHandle]

Notable derived classes:

- ``ql.UKRPI``
- ``ql.USCPI``
- ``ql.EUHICP``
- ``ql.EUHICPXT``

The ``ZeroInflationIndex`` other subclasses can be found under `ql/indexes/inflation` (in QuantLib C++ Library).


YoY inflation
-------------

.. class:: YoYInflationIndex(familyName: str, region: ql.Region, revised: bool, frequency: ql.Frequency, availabilityLag: ql.Period, currency: ql.Currency, h: Optional[ql.ZeroInflationTermStructureHandle])

    Constructor for quoted year-on-year indices.
    An index built with this constructor needs its past fixings (i.e., the past year-on-year values) to be stored via the ``addFixing`` or ``addFixings`` method.

    :param familyName: The name of the year-on-year inflation index family (e.g., "YYCPI", "YYHICP").
    :type familyName: str
    :param region: The geographical region for which the index is published.
    :type region: ql.Region
    :param revised: Whether the index can be revised after publication.
    :type revised: bool
    :param frequency: The frequency with which the index is published (e.g., Monthly, Quarterly).
    :type frequency: ql.Frequency
    :param availabilityLag: The lag between the reference period and the publication date.
    :type availabilityLag: ql.Period
    :param currency: The currency in which the index is quoted.
    :type currency: ql.Currency
    :param h: (Optional) The zero inflation term structure handle used for forecasting.
    :type h: Optional[ql.ZeroInflationTermStructureHandle]

.. class:: YoYInflationIndex(underlyingIndex: ql.ZeroInflationIndex, ts: Optional[ql.YoYInflationTermStructureHandle])

    Constructor for year-on-year indices defined as a ratio.
    An index build with this constructor won't store past fixings of its own; they will be calculated as a ratio from the past fixings stored in the underlying index.

    :param underlyingIndex: The underlying zero inflation index used to compute year-on-year values.
    :type underlyingIndex: ql.ZeroInflationIndex
    :param ts: (Optional) The year-on-year inflation term structure handle used for forecasting.
    :type ts: Optional[ql.YoYInflationTermStructureHandle]

The ``YoYInflationIndex`` other subclasses can be found under `ql/indexes/inflation` (in QuantLib C++ Library).

- ``ql.YYEUHICP``
- ``ql.YYEUHICPXT``
- ``ql.YYFRHICP``
- ``ql.YYUKRPI``
- ``ql.YYUSCPI``
- ``ql.YYZACPI``
