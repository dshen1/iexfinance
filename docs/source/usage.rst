.. _usage:

.. role:: strike

.. ipython:: python
    :suppress:

    import requests_cache


Usage
=====

.. _usage.common:

Common Tasks
------------

Historical Data
~~~~~~~~~~~~~~~

Daily
^^^^^




Minutely
^^^^^^^^

Overview
--------

The iexfinance codebase and documentation are structured in a way that emulates much of the `IEX API Documentation <https://iextrading.com/developer/docs>`__ for readability and ease of use.

Thus there are four main modules of iexfinance, each allowing the retrieval of data from one of IEX's main endpoint groups:

  - :ref:`Stocks<usage.stocks>`
  - :ref:`Reference Data<usage.reference-data>`
  - :ref:`IEX Market Data<usage.iex-market-data>`
  - :ref:`IEX Stats<usage.iex-stats>`

These modules provide classes and top-level functions to execute queries to the IEX API.


.. _usage.parameters:

Parameters
----------



All classes and top-level functions utilize the ``_IEXBASE`` class to make their
requests:

.. autoclass:: iexfinance.base._IEXBase

.. _usage.stocks:

Stocks
------

.. seealso:: For more information, see `Stocks <stocks.html>`__.


IEX provides a list of symbols that are available for access, and as
such, we provide a function ``get_available_symbols`` to obtain this
list. Invalid symbols will be met with a ``IEXSymbolError``, and
duplicate symbols will be kept intact without alteration.

Endpoints
~~~~~~~~~

The Stock endpoints of the `IEX Developer
API <https://iextrading.com/developer/>`__ are below, each of which
contains data regarding a different aspect of the security/securities.
The top-level ``Stock`` function creates an object that can obtain each
of these endpoints. Requests for single symbols will return the *exact* results
from that endpoint as shown in the IEX API documentation (see below). Requests
for multiple symbols will return a symbol-indexed dictionary of
the endpoint requested.

**Endpoint Method** examples ``get_quote()``, ``get_volume_by_venue()``

.. ipython:: python

	from iexfinance import Stock
	aapl = Stock("AAPL")
    aapl.get_previous()


For a detailed list of the *endpoint methods*, see
`here <stocks.html#endpoints>`__.

Fields
~~~~~~

To obtain individual fields from an endpoint, select `Field Methods
<stocks.html#field-methods>`__ are also provided.

Examples ``get_open()``, ``get_name()``

**Single Symbol**

.. ipython:: python

    aapl.get_open()
    aapl.get_price()

**Multiple Symbols**

.. ipython:: python

    b = Stock(["AAPL", "TSLA"])
    b.get_open()


For a detailed list of these functions, see `here <stocks.html>`__.

Endpoint-Specific Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Top-level parameters may be passed to the ``Stock`` function, including
``output_format`` and request parameters (such as ``retry_count``, and
``pause``) - the latter of which will be used should any queries made by
the object fail. These parameters are passed keyword arguments, and are
entirely optional.

Certain endpoints (such as quote and chart), however, allow customizable
parameters. To specify one of these parameters, merely pass it to an endpoint
method as a keyword argument.

.. ipython:: python

    aapl = Stock("AAPL", output_format='pandas')
    aapl.get_quote(displayPercent=True).loc["ytdChange"]

.. note:: The ``output_format`` from the initial
  call to the ``Stock`` function will be used (if the output format has not been
  change through ``change_output_format`` since) and **cannot be changed**
  through calls to endpoint methods. See `Stocks <stocks.html>`__ for
  more information.

.. _usage.reference-data:

Reference Data
--------------

.. seealso:: For more information, see `Reference Data <ref.html>`__


.. _usage.iex-market-data:


IEX Market Data
---------------

.. seealso:: For more information, see `IEX Market Data <market.html>`__

The IEX Market Data `endpoints <market.html>`__


.. _usage.iex-stats:

IEX Stats
---------

.. seealso:: For more information, see `IEX Stats <stats.html>`__

The IEX Stats `endpoints <stats.html>`__

.. _usage.caching:

Caching
-------

iexfinance supports the caching of HTTP requests to IEX using the
`requests-cache <https://pypi.python.org/pypi/requests-cache>`__ package.

.. seealso:: `Caching Queries <caching.html>`__
