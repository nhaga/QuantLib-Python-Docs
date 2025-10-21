******
Basics
******

Below are the commands to set up the QuantLib with evaluation date. Everything starts with "evaluation date" which means the date you want to value a instrument (for more info see `The global evaluation Date <https://implementingquantlib.substack.com/p/the-global-evaluation-date>`_). Consider you want to value a "Swap" as of 09/16/2020, you will first set the evaluationDate in QuantLib. Underhood C++ quant library is packaged using SWIG and python is more a API calling the C++ library.

Settings

########


.. code-block:: python

    #import the Quant Lib
    import QuantLib as ql
    
    # Let the today date whenwe want to value a instrument be
    today = ql.Date(15,6,2020)
    
    # we can set evaluationDate in QL as
    ql.Settings.instance().evaluationDate = today
    print(ql.Settings.instance().evaluationDate);
    # prints..June 15th, 2020
    
    # or you can do
    today = ql.Date(15,12,2021);
    ql.Settings.instance().setEvaluationDate(today)
    print(ql.Settings.instance().evaluationDate)
    # prints..December 15th, 2021


Moves date of referenced curves:
Following returns the term structure based on FlatForward


.. code-block:: python
    
    settlementDays = 2
    
    # Holiday calendar of united states
    calendar = ql.UnitedStates()
    
    forwardRate = 0.05
    
    """Day Counter provides methods for determining the length of a time period according to given market convention, 
    both as a number of days and as a year fraction."""
    dayCounter = ql.Actual360()
    
    # Construct flat forward rate term structure
    flatForwardTermStructure = ql.FlatForward(settlementDays, calendar, forwardRate, dayCounter)
    
    flatForwardTermStructure.referenceDate()
    
    print("Max Date: ", flatForwardTermStructure.maxDate())


Changes evaluation date of calculation: 
Following shows the use of evaluation or Valuation date. Lets construct a schedule which will be used to create a leg and then we will calculate interest rate on the leg.

.. code-block:: python

    today = ql.Date(15,6,2020)
    ql.Settings.instance().evaluationDate = today
    
    effectiveDate = ql.Date(15, 6, 2020)
    terminationDate = ql.Date(15, 6, 2022)
    
Create a schedule

.. code-block:: python    

    schedule = ql.MakeSchedule(effectiveDate, terminationDate, ql.Period('6M'))

create a fixed rate leg using helper class building a sequence of fixed rate coupons

.. code-block:: python

    notional = [100.0]
    rate = [0.05]
    leg = ql.FixedRateLeg(schedule, dayCounter, notional, rate)      
 
Interest rate class encapsulate the interest rate compounding algebra.
It manages day-counting conventions, compounding conventions,
conversion between different conventions, discount/compound factor
calculations, and implied/equivalent rate calculations.

.. code-block:: python
    
    dayCounter = ql.Thirty360()
    rate = 0.03
    
    """
    ql/Compounding.hpp
        //! Interest rate compounding rule
        enum Compounding { Simple = 0,          //!< \f$ 1+rt \f$
                           Compounded = 1,      //!< \f$ (1+r)^t \f$
                           Continuous = 2,      //!< \f$ e^{rt} \f$
                           SimpleThenCompounded, //!< Simple up to the first period then Compounded
                           CompoundedThenSimple //!< Compounded up to the first period then Simple
        };
    """
    
    compoundingType = ql.Compounded
    
    """
    ql/time/frequency.hpp
    enum Frequency { NoFrequency = -1,     //!< null frequency
                         Once = 0,             //!< only once, e.g., a zero-coupon
                         Annual = 1,           //!< once a year
                         Semiannual = 2,       //!< twice a year
                         EveryFourthMonth = 3, //!< every fourth month
                         Quarterly = 4,        //!< every third month
                         Bimonthly = 6,        //!< every second month
                         Monthly = 12,         //!< once a month
                         EveryFourthWeek = 13, //!< every fourth week
                         Biweekly = 26,        //!< every second week
                         Weekly = 52,          //!< once a week
                         Daily = 365,          //!< once a day
                         OtherFrequency = 999  //!< some other unknown frequency
        };
    """
    
    frequency = ql.Annual
    interestRate = ql.InterestRate(rate, dayCounter, compoundingType, frequency)

4.958531764309427


ql/cashflows/Cashflows.hpp 
The NPV is the sum of the cash flows, each discounted
according to the given constant interest rate.  The result
is affected by the choice of the interest-rate compounding
and the relative frequency and day counter.

.. code-block:: python

    ql.Settings.instance().evaluationDate = ql.Date(15,12,2020)
    print( ql.CashFlows.npv(leg, rate, False) )

2.4906934531375144

--------


Array
#####

creates an empty array

.. function:: ql.Array()

creates the array and fills it with value 

.. function:: ql.Array(size, value)

creates the array and fills it according to a0=value,ai=ai−1+increment

.. function:: ql.Array(size, value, increment)


-----

Matrix
######

creates a null matrix

.. function:: ql.Matrix()
 
creates a matrix with the given dimensions

.. function:: ql.Matrix(rows, columns)
 
creates the matrix and fills it with value

.. function:: ql.Matrix (rows, columns, value)


.. code-block:: python

    ql.Matrix()
    ql.Matrix(2,2)
    ql.Matrix(2,2,0.5)


.. code-block:: python

    A = ql.Matrix(3,3)
    A[0][0]=0.2
    A[0][1]=8.4
    A[0][2]=1.5
    A[1][0]=0.6
    A[1][1]=1.4
    A[1][2]=7.3
    A[2][0]=0.8
    A[2][1]=4.4
    A[2][2]=3.2

-----

Observable
##########

.. code-block:: python

    import QuantLib as ql

    flag = None
    def raiseFlag():
        global flag
        flag = 1
        
    me = ql.SimpleQuote(0.0)
    obs = ql.Observer(raiseFlag)
    obs.registerWith(me)
    me.setValue(3.14)
    if not flag:
        print("Case 1: Observer was not notified of market element change")
    flag = None
    obs.unregisterWith(me)
    me.setValue(3.14)
    if not flag:
        print("Case 2: Observer was not notified of market element change")


----

Quotes
######

SimpleQuote
***********

.. function:: ql.SimpleQuote(value)

.. code-block:: python

    s = ql.SimpleQuote(0.01)

**Functions**

- value
- setValue
- isValid

.. code-block:: python

    s.value()
    s.setValue(0.05)
    s.isValid()


DerivedQuote
************

.. function:: ql.DerivedQuote(quoteHandle, function)

.. code-block:: python

    d1 = ql.SimpleQuote(0.06)
    d2 = ql.DerivedQuote(ql.QuoteHandle(d1),lambda x: 10*x)


CompositeQuote
**************

.. function:: ql.CompositeQuote(element1: ql.QuoteHandle, element2: ql.QuoteHandle, f)

.. code-block:: python

    c1 = ql.SimpleQuote(0.02) 
    c2 = ql.SimpleQuote(0.03)

    def f(x,y):
        return x+y

    c3 = ql.CompositeQuote(ql.QuoteHandle(c1),ql.QuoteHandle(c2), f)
    c3.value()

    c4 = ql.CompositeQuote(ql.QuoteHandle(c1),ql.QuoteHandle(c2), lambda x,y:x+y)
    c4.value()    


DeltaVolQuote
*************

A class for FX-style quotes where delta-maturity pairs are quoted in implied vol

.. class:: ql.DeltaVolQuote(delta, volQuoteHandle, maturity, deltaType)
.. class:: ql.DeltaVolQuote(volQuoteHandle, deltaType, maturity, atmType)
   :no-index-entry:

.. code-block:: python

    deltaType = ql.DeltaVolQuote.Fwd    # Also supports: Spot, PaSpot, PaFwd
    atmType = ql.DeltaVolQuote.AtmFwd   # Also supports: AtmSpot, AtmDeltaNeutral, AtmVegaMax, AtmGammaMax, AtmPutCall50

    maturity = 1.0
    volAtm, vol25DeltaCall, vol25DeltaPut = 0.08, 0.075, 0.095

    atmDeltaQuote = ql.DeltaVolQuote(ql.QuoteHandle(ql.SimpleQuote(volAtm)), deltaType, maturity, atmType)
    vol25DeltaPutQuote = ql.DeltaVolQuote(-0.25, ql.QuoteHandle(ql.SimpleQuote(vol25DeltaPut)), maturity, deltaType)
    vol25DeltaCallQuote = ql.DeltaVolQuote(0.25, ql.QuoteHandle(ql.SimpleQuote(vol25DeltaCall)), maturity, deltaType)

Handles
######

The ``Handle`` class is a key component of the C++ `QuantLib` library. Conceptually, a `Handle` behaves like a **double pointer** — it essentially wraps a ``shared_ptr`` (for more details, see the `Handling dependencies <https://www.quantlibguide.com/Handling%20dependencies.html#fnref1>`_ section).

Since `Handle` is a **templated class**, it is not directly accessible from QuantLib’s Python bindings. Instead, specific handle classes are provided for each QuantLib object type that needs to be wrapped. In practice, these are usually **term structures** or **quote** objects.

In Python, the naming convention for these handle classes follows the pattern ``<ClassName>Handle`` — for example, ``YieldTermStructureHandle``.

We have two types of handles: Handle and RelinkableHandle. The first can be think as a **const** Handle while the second as a non-**const** Handle that can be relinked, for example to another termstructure. 

Handles are especially useful when the underlying object may change dynamically. For instance, if the base term structure changes and we need to reprice a bond, we can use a ``RelinkableHandle`` to easily update the reference. By calling the ``linkTo`` method, the handle can be relinked to a new term structure, after which we can simply request the bond's ``NPV()`` again.

This approach eliminates the need for manual setters and helps avoid confusion and potential errors when managing multiple objects that depend on different term structures.
