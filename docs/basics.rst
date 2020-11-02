******
Basics
******

Settings
########

.. code-block:: python

    today = ql.Date(15,6,2020)
    ql.Settings.instance().evaluationDate = today
    ql.Settings.instance().setEvaluationDate(today)


Moves date of referenced curves:

.. code-block:: python

    crv = ql.FlatForward(2, ql.TARGET(), 0.05, ql.Actual360())
    crv.referenceDate()


Changes evaluation date of calculation:

.. code-block:: python

    today = ql.Date(15,6,2020)
    ql.Settings.instance().evaluationDate = today
    schedule = ql.MakeSchedule(ql.Date(15,6,2020), ql.Date(15,6,2021), ql.Period('6M'))
    leg = ql.FixedRateLeg(schedule, ql.Actual360(), [100.], [0.05])
    rate = ql.InterestRate(.03, ql.Thirty360(), ql.Compounded, ql.Annual)
    print( ql.CashFlows.npv(leg, rate, False) )

4.958531764309427

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

.. function:: ql.CompositeQuote(quoteHandle, quoteHandle, function)

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

.. function:: ql.DeltaVolQuote(delta, volQuoteHandle, maturity, deltaType)
.. function:: ql.DeltaVolQuote(volQuoteHandle, deltaType, maturity, atmType)

.. code-block:: python

    deltaType = ql.DeltaVolQuote.Fwd    # Also supports: Spot, PaSpot, PaFwd
    atmType = ql.DeltaVolQuote.AtmFwd   # Also supports: AtmSpot, AtmDeltaNeutral, AtmVegaMax, AtmGammaMax, AtmPutCall50

    maturity = 1.0
    volAtm, vol25DeltaCall, vol25DeltaPut = 0.08, 0.075, 0.095

    atmDeltaQuote = ql.DeltaVolQuote(ql.QuoteHandle(ql.SimpleQuote(volAtm)), deltaType, maturity, atmType)
    vol25DeltaPutQuote = ql.DeltaVolQuote(-0.25, ql.QuoteHandle(ql.SimpleQuote(vol25DeltaPut)), maturity, deltaType)
    vol25DeltaCallQuote = ql.DeltaVolQuote(0.25, ql.QuoteHandle(ql.SimpleQuote(vol25DeltaCall)), maturity, deltaType)

----


