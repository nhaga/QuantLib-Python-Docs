Forwards
********

Forward Rate Agreement
----------------------

.. function:: ql.ForwardRateAgreement(valueDate, maturityDate, position, strikeForward, notional, iborIndex, discountCurve)


.. code-block:: python

  fra = ql.ForwardRateAgreement(
      ql.Date(15,6,2020),
      ql.Date(15,12,2020),
      ql.Position.Long,
      0.01,
      1e6,
      ql.Euribor6M()    
  )

  
Fixed Rate Bond Forward
-----------------------