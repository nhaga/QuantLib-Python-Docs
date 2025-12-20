
Installation
------------

This page explains the most common ways to install the Python bindings for QuantLib and how to verify the installation.

Prerequisites
^^^^^^^^^^^^^

- A working Python 3 installation (Python 3.8+ recommended).

Using a virtual environment (recommended)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create and activate an isolated environment before installing packages in Linux:

.. code-block:: bash

    python -m venv .venv
    source .venv/bin/activate

While from Windows (using powershell):

.. code-block:: cmd

    python -m venv .venv
    .venv/Scripts/activate.ps1


Install with pip
^^^^^^^^^^^^^^^^

The simplest installation (binary wheels available on PyPI for many platforms) is:

.. code-block:: bash

    pip install --upgrade pip
    pip install QuantLib

If you already have QuantLib installed and want to upgrade:

.. code-block:: bash

    pip install --upgrade QuantLib

Conda (alternative)
^^^^^^^^^^^^^^^^^^^^

If you use conda, the conda-forge channel typically provides QuantLib packages. Example:

.. code-block:: bash

    conda install conda-forge::quantlib

Note: package names on conda-forge can vary (e.g. "quantlib" or "quantlib-python"). If the command above does not find a package, search the conda-forge index or use the Anaconda Navigator UI.

Importing
---------

.. code-block:: python

    import QuantLib as ql