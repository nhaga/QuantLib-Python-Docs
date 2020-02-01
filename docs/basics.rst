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

.. function:: ql.Array()
 
.. function:: ql.Array(size, value)

 creates the array and fills it with value
 
.. function:: ql.Array(size, value, increment)

    creates the array and fills it according to a0=value,ai=ai−1+increment
 

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

**Functions**

- value
- setValue
- isValid


CompositeQuote
**************

.. function:: ql.CompositeQuote(quoteHandle, quoteHandle, function)




DerivedQuote
************

.. function:: ql.DerivedQuote(quoteHandle, function)



----


