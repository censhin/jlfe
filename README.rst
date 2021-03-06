####
jlfe
####

*An experimental wrapper around bits of LFE*

.. image:: images/logos/DukeOfferingLFE-square-tiny.png


Introduction
============

This project is 1100100% experimentation.

Its sole purpose is to explore the possibility of slightly increasing
programmer convenience when using LFE on `Erjang`_ (Erlang on the JVM).

Initially, chunks of LFE code were copied, but the latest version requires
that one manually make a single change to a function (instructions below),
and then does the rest in the jlfe code.


**Development Information**

* `Goals`_

* `Features`_


Dependencies
============

This project assumes that you have `lfetool`_ installed somwhere in your
``$PATH``. Also, we're not going to cover the installation of Java -- you
*will* need Java installed on your system in order to run jlfe ;-)

This project depends upon the following, which are saved to ``./deps`` when
you run ``make get-deps``:

* `LFE`_ (Lisp Flavored Erlang; needed only to compile)
* `lfeunit`_ (needed only to run the unit tests)
* `lfe-utils`_

Dependencies not installed automatically:

* `lfetool`_ (click the link for installation instructions)
* `kerl`_ (see below)
* `Erjang`_ (see below)
* `rlwrap`_ (``readline`` support for the Erjang shell; installable on many
  linux distros; on Mac OS X, install with `Homebrew`_)

If you don't have ``rebar``, ``kerl`` and Erlang installed:

.. code:: bash

    $ lfetool install rebar
    $ lfetool install kerl
    $ lfetool install erlang R16B
    $ . /opt/erlang/R16B/activate

Erjang installation is similarly easy:

.. code:: bash

    $ lfetool install erjang


Obtaining and Building jlfe
===========================

Building jlfe and its dependencies is as easy as this:

.. code:: bash

    $ git clone https://github.com/oubiwann/jlfe.git
    $ cd jlfe
    $ make compile

That last ``make`` target will do the following:

* Download all the project dependencies,

* Apply a patch to LFE to accept the jlfe form ``(.XXX ...)``, and

* Compile all the dependencies, the patched LFE, and jlfe.


jlfe Usage
==========


With everything built, you're now ready to play. To run the examples below,
start the jlfe REPL:

.. code:: bash

    $ lfetool repl jlfe


Syntax Additions
----------------


Constructors
,,,,,,,,,,,,


.. code:: cl

    > (.java.util.HashMap)
    ()
    >
    > (.java.lang.Double 42)
    42.0

Or you can use the short-cut for all ``java.lang.*`` classes:

.. code:: cl

    > (.Double 42)
    42.0


Static Methods
,,,,,,,,,,,,,,

.. code:: cl

    > (.java.lang.String:getName)
    java.lang.String

or

.. code:: cl

    > (.String:getName)
    java.lang.String
    >
    > (.Math:sin 0.5)
    0.479425538604203


Static Field Variables
,,,,,,,,,,,,,,,,,,,,,,

e.g., constants:

.. code:: cl

    > (.Math:PI)
    3.141592653589793
    >
    > (.java.math.BigDecimal:ROUND_CEILING)
    2


Nested Classes
,,,,,,,,,,,,,,

.. code:: cl

    > (.java.util.AbstractMap$SimpleEntry "a" "b")
    #B()


Utility Functions
-----------------

Some Java types from Erjang don't render anything useful when evaluated:

.. code:: cl

    > (set bool (.Boolean true))
    #B()
    > (set flt (.Float 42))
    #B()
    > (set bigdec (.java.math.BigDecimal 42))
    #B()


The ``value-of`` function lets us treat Java objects as distinct values
while still keeping the object around, should we want to call any methods on
it, etc.:

.. code:: cl

    > (jlfe-types:value-of bool)
    true
    > (jlfe-types:value-of flt)
    42.0
    > (jlfe-types:value-of bigdec)
    42.0

Types that don't need special treatment are passed through, as-is:

.. code:: cl

    > (jlfe-types:value-of (.Integer 42))
    42


.. Links
.. -----
.. _rebar: https://github.com/rebar/rebar
.. _LFE: https://github.com/rvirding/lfe
.. _lfeunit: https://github.com/lfe/lfeunit
.. _lfe-utils: https://github.com/lfe/lfe-utils
.. _Erjang: https://github.com/trifork/erjang
.. _lfetool: https://github.com/lfe/lfetool/
.. _kerl: https://github.com/spawngrid/kerl
.. _rlwrap: http://utopia.knoware.nl/~hlub/uck/rlwrap/#rlwrap
.. _Homebrew: http://brew.sh/
.. _Goals: doc/goals.rst
.. _Features: doc/features.rst
