================
Module _gmp.gmpz
================

.. module:: _gmp.gmpz

GMP Integers
============

Object mpz
----------

.. py:class:: mpz([int|float|str[, base=0]|mpz])

   :param (optional): This argument is either an :py:class:`int`, a
		      :py:class:`float`, a :py:class:`str`, or a
		      :py:class:`mpz`.
		      
		      * *float*: must fit into a C :C:`double`.
		      * *str*: *base* must be :Py:`0` or in range
                        :Py:`[2-62]`.
   :returns: A new :py:class:`mpz` object.
   :raise TypeError, ValueError, OverflowError:

   Some examples of :py:class:`mpz` object construction:

   .. code-block:: Python

      >>> z = mpz() # C equiv.: mpz_t z; mpz_init(z);
      >>> z = mpz(2026)
      >>> z = mpz(3.14) # C equiv.: mpz_t z; mpz_init_set_d(3.14);
      >>> z = mpz('10001011', 2) # C equiv.: mpz_t z; mpz_init_set_str('10001011', 2);
      >>> u = mpz(z)
      >>> print(u)
      2026

Properties of the mpz object
----------------------------

Operators
^^^^^^^^^

.. table::
  :width: 75%
  :widths: auto
  :align: left
	  
  ================= ============================== =========
  **Operation**     **Result**                     **Notes**
  ================= ============================== =========
  x + y             sum of x and y                 \(1\)
  ----------------- ------------------------------ ---------
  x - y             difference of x and y          \(1\)
  ----------------- ------------------------------ ---------
  x * y             product of x and y             \(1\)
  ----------------- ------------------------------ ---------
  x // y            floored quotient of x and y    \(1\)
  ----------------- ------------------------------ ---------
  x % y             remainder of :Py:`x / y`       \(1\)
  ----------------- ------------------------------ ---------
  \-x               x negated
  ----------------- ------------------------------ ---------
  \+x               x unchanged
  ----------------- ------------------------------ ---------
  abs(x)            absolute value of x
  ----------------- ------------------------------ ---------
  int(x)            x converted to Python integer
  ----------------- ------------------------------ ---------
  divmod(x, y)      the pair :Py:`(x // y, x % y)` \(1\)
  ----------------- ------------------------------ ---------
  pow(x, y, z=None) x ** y (mod z) if z != None    \(2\)
  ----------------- ------------------------------ ---------
  x ** y            x to the power y               \(3\)
  ================= ============================== =========

Notes:

1. The operands :Py:`x` and :Py:`y` are either instances of
   :py:class:`mpz` or :py:class:`int`.
2. All operands are either instances of :py:class:`mpz` or
   :py:class:`int`, except for :Py:`z`,
   which can be :py:data:`Ǹone`, then :Py:`pow(x, y, None)` is equivalent
   to :Py:`x ** y`. In the latter case, :Py:`y` must be an
   instance of :py:class:`int` that must fit into a C :C:`unsigned
   long`.
3. Operand :Py:`x` is an instance of :py:class:`mpz` and :Py:`y` is an
   instance of :py:class:`int` that must fit into a C :C:`unsigned
   long`.

.. note:: All corresponding in-place operators are therefore
   available, namely: :Py:`+=`, :Py:`-=`, :Py:`*=`, etc.


Number Theoretic Functions
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: In the following, :Py:`z` denotes a generic :py:class:`mpz`
          object.

.. py:method:: z.probab_prime_p([reps=24])

   Python wrapper for GNU MP :C:`mpz_probab_prime_p()` function.

   :param reps: Number of Miller=Rabin probabilistic primality
                tests. Must be in range :Py:`[15-50]`, otherwise
                raises :py:exc:`ValueError`.
   :returns: :Py:`2` if :Py:`z` is definitely prime, returns :Py:`0`
	     if :Py:`z` is probably prime (:math:`p\leq4^{=reps}`), or
	     returns :Py:`0` if :Py:`z` is composite.
   :raises TypeError, ValueError:

.. py:method:: z.nextprime()

   Python wrapper for GNU MP :C:`mpz_nextprime()` function.

   :returns: next prime greater than :Py:`z`.

.. py:method:: z.prevprime()

   Python wrapper for GNU MP :C:`mpz_prevprime()` function.

   :returns: :Py:`(p, n)` where :Py:`p` is the greatest prime less
	     than :Py:`z` or :py:data:`None` (if it doesn't exist) and
	     :Py:`n` is either :Py:`0` if :Py:`p` is :py:data:`None`,
	     :Py:`1` if :Py:`p` is probably prime, or :Py:`2` if
	     :Py:`p` is definitely prime.

   .. code=block:: Python

      >>> z = mpz(2)
      >>> print(z.prevprime())
      (None, 0)
      >>> z = mpz(2026)
      >>> print(z.prevprime())
      (2017, 2)

.. py:method:: z.sqrt()

   Python wrapper for GNU MP :C:`mpz_sqrt()` function.

   :returns: :math:`\lfloor\sqrt z\rfloor`
   :raises GMPErrSqrtOfNegative:

.. py:method:: z.sqrtrem()

   Python wrapper for GNU MP :C:`mpz_sqrtrem()` function.

   :returns: :Py:`(root, rem)`, where :Py:`root` is
	     :math:`\lfloor\sqrt z\rfloor` and :Py:`rem` is
	     :math:`(z-root^2)`.
   :raises GMPErrSqrtOfNegative:

.. py:method:: z.root(n)

   Python wrapper for GNU MP :C:`mpz_root()` function.

   :param n: an instance of :py:class:`int` that must fit into a C
	     :C:`unsigned long`.
   :returns: :Py:`(r, b)`, where :Py:`r` is
	     :math:`\lfloor\sqrt[n]z\rfloor` and :Py:`b` a :py:class:`bool`
	     whose value is :py:data:`True` if the computation was exact,
	     :py:data:`False` otherwise.
   :raises TypeError, GMPErrSqrtOfNegative:

.. py:method:: z.rootrem(n)

   Python wrapper for GNU MP :C:`mpz_rootrem()` function.

   :param n: an instance of :py:class:`int` that must fit into a C
	     :C:`unsigned long`.
   :returns: :Py:`(root, rem)`, where :Py:`root` is
	     :math:`\lfloor\sqrt[n]z\rfloor` and :Py:`rem` is
	     :math:`(z-root^n)`.
   :raises TypeError, GMPErrSqrtOfNegative:

.. py:method:: z.perfect_power_p()

   Python wrapper for GNU MP :C:`mpz_perfect_power_p()` function.

   :returns: :py:data:`True` if :Py:`z` is a perfect power,
             :py:data:`False` otherwise.

.. py:method:: z.perfect_square_p()

   Python wrapper for GNU MP :C:`mpz_perfect_square_p()` function.

   :returns: :py:data:`True` if :Py:`z` is a perfect square,
             :py:data:`False` otherwise.

Bitwise Operations
^^^^^^^^^^^^^^^^^^

.. table::
  :width: 75%
  :widths: auto
  :align: left
	  
  =============== ================================= =========
  **Operation**   **Result**                        **Notes**
  =============== ================================= =========
  x | y           bitwise *or* of x and y           \(1\)
  --------------- --------------------------------- ---------
  x ^ y           bitwise *exclusive or* of x and y \(1\)
  --------------- --------------------------------- ---------
  x & y           bitwise *and* of x and y          \(1\)
  --------------- --------------------------------- ---------
  x << n          x shifted left by n bits          \(2\)
  --------------- --------------------------------- ---------
  x >> n          x shifted right by n bits         \(2\)
  --------------- --------------------------------- ---------
  ~x              the bits of x inverted        
  =============== ================================= =========

Notes:

1. The operands :Py:`x` and :Py:`y` are either instances of
   :py:class:`mpz` or :py:class:`int`.
2. Operand :Py:`x` is an instance of :py:class:`mpz` and :Py:`n` is an
   instance of :py:class:`int` that must fit into a C :C:`unsigned
   long`.

.. note:: All corresponding in-place operators are therefore
   available, namely: :Py:`|=`, :Py:`^=`, :Py:`&=`, :Py:`<<=` and
   :Py:`>>=`.

Other Bitwise Operations
........................

Let :Py:`z` be an instance of :py:class:`mpz`, then the following
methods are applicable to :Py:`z`:

.. py:method:: z.setbit(bit_index)

   Python wrapper for GNU MP :C:`mpz_setbit()` function. Set bit
   *bit_index* in :Py:`z`.

   :param bit_index: a :py:class:`int` that must fit into a C
		     :C:`unsigned long`.
   :returns: :py:data:`None`.

.. py:method:: z.clrbit(bit_index)

   Python wrapper for GNU MP :C:`mpz_clrbit()` function. Clear bit
   *bit_index* in :Py:`z`.

   :param bit_index: a :py:class:`int` that must fit into a C
		     :C:`unsigned long`.
   :returns: :py:data:`None`.

.. py:method:: z.combit(bit_index)

   Python wrapper for GNU MP :C:`mpz_combit()` function. Complement
   bit *bit_index* in :Py:`z`.

   :param bit_index: a :py:class:`int` that must fit into a C
		     :C:`unsigned long`.
   :returns: :py:data:`None`.

.. py:method:: z.tstbit(bit_index)

   Python wrapper for GNU MP :C:`mpz_combit()` function. Test bit
   *bit_index* in :Py:`z` and return :py:data:`False` or
   :py:data:`True` accordingly.

   :param bit_index: a :py:class:`int` that must fit into a C
		     :C:`unsigned long`.
   :returns: :py:data:`True` or :py:data:`False`.

.. py:method:: z.popcount()

   Python wrapper for GNU MP :C:`mpz_popcount()` function.

   :returns: If :Py:`z > 0`, returns the number of :Py:`1` bits in the
	     binary representation of :Py:`z`. If :Py:`z < 0`, the
	     number of :Py:`1` is infinite, and the return value is the
	     largest possible value of a :C:`unsigned long`.

.. py:method:: z.scan0(starting_bit=0)

   Python wrapper for GNU MP :C:`mpz_scan0()` function. Scan :Py:`z`,
   starting from bit :Py:`starting bit`, towards more significant
   bits, until the first :Py:`0` bit is found. Return the index of the
   found bit.

   :parameters starting_bit: a :py:class:`int` that must fit into a C
			     :C:`unsigned long`.
   :returns: The index of the found bit.
   :raises TypeError:

.. py:method:: z.scan1(starting_bit=0)

   Python wrapper for GNU MP :C:`mpz_scan0()` function. Scan :Py:`z`,
   starting from bit :Py:`starting bit`, towards more significant
   bits, until the first :Py:`1` bit is found. Return the index of the
   found bit.

   :parameters starting_bit: a :py:class:`int` that must fit into a C
			     :C:`unsigned long`.
   :returns: The index of the found bit.
   :raises TypeError:

Some examples:

.. code-block:: Python

   >>> z = mpz()
   >>> z.setbit(31)
   >>> print(z)
   2147483648
   >>> z.clrbit(31)
   >>> print(z)
   0
   >>> z.combit(15)
   >>> print(z, z.tstbit(15))
   32768 True
   >>> z = mpz(0x8fffffff)
   >>> print(z.popcount()
   29
   >>> print((-z).popcount())
   18446744073709551615
   >>> z = mpz(0x8fffff)
   >>> print(z.scan0())
   20
   >>> z = mpz(1 << 32)
   >>> print(z.scan1())
   32

Miscellaneous methods
^^^^^^^^^^^^^^^^^^^^^

Let :Py:`z` be an instance of :py:class:`mpz`:

.. py:method:: z.sizeinbase(base=10)

   Python wrapper for GNU MP :C:`mpz_sizeinbase()` function.

   :param base: a :py:class:`int` in range :Py:`[2-62]`.
   :returns: The size of :Py:`z` measured in number of digits in the
             given :Py:`base`.
   :raises TypeError, ValueError:

.. py:method:: z.get_str(base=10)

   Python wrapper for GNU MP :C:`mpz_get_str()` function.

   :param base: a :py:class:`int` in interval :Py:`[2, 62]` or in
                interval :Py:`[-36, -2]`.
   :returns: The conversion of :Py:`z` to a string (:py:class:`str`)
             of digits in base :Py:`base`.
   :raises TypeError, ValueError:

   .. note:: The prefix :Py:`0b` (respectively: :Py:`0o`, :Py:`0x`) is
             added when the base is :Py:`2` (respectively: :Py:`8`,
             :Py:`16`).

   .. code-block:: Python

      >>> z = mpz(2000)
      >>> print(z.get_str(7))
      5555
      >>> print(z.get_str(16))
      0x7d0
      >>> print(z.get_str(2))
      0b11111010000

Comparisons and Booleans
^^^^^^^^^^^^^^^^^^^^^^^^

Let :Py:`x` and :Py:`y` be two objects that are either instances of
:py:class:`mpz` or :py:class:`int`. Then all comparison operations are
valid: :Py:`x < y`, :Py:`x <= y`, :Py:`x > y`, :Py:`x >= y`, :Py:`x ==
y`, :Py:`x != y`.

Instructions such as :Py:`if x:` and :Py:`if not x:` are valid and
have their usual meaning.

Some examples:

.. code-block:: Python

   >>> x = mpz(10)
   >>> y = mpz(5)
   >>> x < y
   False
   >>> x == 2 * y
   True
   >>> -y >= -5
   True
   >>> 11 > x
   True
   >>> -2 * -y == x
   True

Attributes
^^^^^^^^^^

.. note:: In the following, :Py:`z` denotes a generic :py:class:`mpz`
          object.

.. py:attribute:: z.sgn

   :Py:`z.sgn` is the sign of :Py:`z`: +1 if :Py:`z > 0`, 0 if
   :Py:`z == 0`, and −1 if :Py:`z < 0`.

.. py:attribute:: z.is_even

   :Py:`z.is_even` is :py:data:`True` if :Py:`z` is even,
   :py:data:`False` otherwise.

.. py:attribute:: z.is_odd

   :Py:`z.is_odd` is :py:data:`True` if :Py:`z` is odd,
   :py:data:`False` otherwise.

.. py:attribute:: z.is_pow_of_2

   :Py:`z.is_pow_of_2` is :py:data:`True` if :Py:`z` is a power of :Py:`2`,
   :py:data:`False` otherwise.

Module Methods
--------------

.. py:method:: gcd(z1, z2)

   Python wrapper for GNU MP :C:`mpz_gcd()` function.

   :parameters z1, z2: :Py:`z1` and :Py:`z2` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: The greatest common divisor of :Py:`z1` and :Py:`z2`.
   :raises TypeError:

.. py:method:: gcdext(z1, z2)

   Python wrapper for GNU MP :C:`mpz_gcdext()` function.

   :parameters z1, z2: :Py:`z1` and :Py:`z2` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: :Py:`(g, s, t)` where :Py:`g` is the greatest common
             divisor of :Py:`z1` and :Py:`z2` and the pair :Py:`(s,
             t)` that satisfies the equation: :math:`g=sz_1+tz_2`.
   :raises TypeError:

   .. code-block:: Python

      >>> print(gcdext(1728, 356))
      (4, 41, -199) # 4 = 41*1728 - 199*356

.. py:method:: lcm(z1, z2)

   Python wrapper for GNU MP :C:`mpz_lcm()` function.

   :parameters z1, z2: :Py:`z1` and :Py:`z2` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: The least common multiple of :Py:`z1` and :Py:`z2`.
   :raises TypeError:

.. py:method:: divisible_p(z, d)

   Python wrapper for GNU MP :C:`mpz_divisible_p()` and
   :C:`mpz_divisible_ui_p()` functions.

   :parameters z, d: :Py:`z` and :Py:`d` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: :py:data:`True` if :Py:`z` is exactly divisible by
	     :Py:`d`, :py:data:`False` otherwise.
   :raises TypeError:

.. py:method:: divisible_2exp_p(z, d)

   Python wrapper for GNU MP :C:`mpz_divisible_2exp_p()` function.

   :parameters z, d: :Py:`z` is either an instance of :py:class:`mpz`
		     or :py:class:`int`. :Py:`d` is a :py:class:`int`
		     that must fit into a C :C:`unsigned long`.
   :returns: :py:data:`True` if :Py:`z` is exactly divisible by
	     :math:`2^d`, :py:data:`False` otherwise.
   :raises TypeError:

.. py:method:: congruent_p(z, c, d)

   Python wrapper for GNU MP :C:`mpz_congruent_p()` and
   :C:`mpz_congruent_ui_p()` functions.

   :parameters z, c, d: :Py:`z`, :Py:`c` and :Py:`d` are either instances of
			:py:class:`mpz` or :py:class:`int`.
   :returns: :py:data:`True` if :Py:`z` is congruent to :Py:`c` modulo
	     :Py:`d`, :py:data:`False` otherwise.
   :raises TypeError:

.. py:method:: congruent_2exp_p(z, c, d)

   Python wrapper for GNU MP :C:`mpz_congruent_2exp_p()` function.

   :parameters z, c, d: :Py:`z` and :Py:`c` are either instances of
			:py:class:`mpz` or :py:class:`int`. :Py:`d` is
			a :py:class:`int` that must fit into a C
			:C:`unsigned long`.
   :returns: :py:data:`True` if :Py:`z` is congruent to :Py:`c` modulo
	     :math:`2^d`, :py:data:`False` otherwise.
   :raises TypeError:

.. py:method:: jacobi(a, b)

   Python wrapper for GNU MP :C:`mpz_jacobi()` function.

   :parameters a, b: :Py:`a` and :Py:`b` are instances of
		     :py:class:`mpz` or :py:class:`int`.
		     :Py:`b` must be odd, otherwise
		     :py:exc:`ValueError` is raised.
   :returns: The jacobi symbol :math:`\left(\dfrac{a}{b}\right)` of
	     :Py:`a` and :Py:`b`.
   :raises TypeError, ValueError:

.. py:method:: legendre(a, b)

   Python wrapper for GNU MP :C:`mpz_legendre()` function.

   :parameters a, b: :Py:`a` and :Py:`b` are instances of
		     :py:class:`mpz` or :py:class:`int`.
		     :Py:`b` must be an odd positive prime, otherwise
		     :py:exc:`ValueError` is raised.
   :returns: The legendre symbol :math:`\left(\dfrac{a}{b}\right)` of
	     :Py:`a` and :Py:`b`.
   :raises TypeError, ValueError:

.. py:method:: kronecker(a, b)

   Python wrapper for GNU MP :C:`mpz_kronecker()`,
   :C:`mpz_kronecker_si()`, :C:`mpz_kronecker_ui()`,
   :C:`mpz_si_kronecker()` and :C:`mpz_ui_kronecker()` functions.

   :parameters a, b: :Py:`a` and :Py:`b` are instances of
		     :py:class:`mpz` or :py:class:`int`.
   :returns: The kronecker symbol :math:`\left(\dfrac{a}{b}\right)` of
	     :Py:`a` and :Py:`b`.
   :raises TypeError:

.. py:method:: invert(a, b)

   Python wrapper for GNU MP :C:`mpz_invert()` function.

   :parameters a, b: :Py:`a` and :Py:`b` are instances of
		     :py:class:`mpz` or :py:class:`int` .
		     :Py:`b` cannot be zero, otherwise
		     :py:exc:`ValueError` is raised.
   :returns: The inverse of :Py:`a` modulo :Py:`b` or :py:data:`None`
             if it does not exist.`
   :raises TypeError, ValueError:

   Some examples:
   
   .. code-block:: Python

      >>> z = mpz(2026)
      >>> print(invert(1013, z))
      None
      >>> print(invert(-11, z))
      1105

.. py:method:: fac_ui(n)

   Python wrapper for GNU MP :C:`mpz_fac_ui()` function.

   :parameters n: a :py:class:`int` that must fit into a C
		  :C:`unsigned long`.
   :returns: :math:`n!`,  the factorial of :Py:`n`.
   :raises TypeError:

.. py:method:: fac2_ui(n)

   Python wrapper for GNU MP :C:`mpz_2fac_ui()` function.

   :parameters n: a :py:class:`int` that must fit into a C
		  :C:`unsigned long`.
   :returns: :math:`n!!`,  the double factorial of :Py:`n`.
   :raises TypeError:

.. py:method:: mfac_uiui(n, m)

   Python wrapper for GNU MP :C:`mpz_mfac_uiui()` function.

   :parameters n, m: two instances of :py:class:`int` that must fit into a C
		     :C:`unsigned long`.
   :returns: :math:`n!^{(m)}`,  the m-multi-factorial of :Py:`n`.
   :raises TypeError:

.. py:method:: primorial_ui(n)

   Python wrapper for GNU MP :C:`mpz_primorial_ui()` function.

   :parameters n: a :py:class:`int` that must fit into a C
		  :C:`unsigned long`.
   :returns: :math:`n\sharp`, the product of all positive prime numbers
             :math:`\leq` :Py:`n`.
   :raises TypeError:

   .. code-block::

      >>> print(primorial_ui(10))
      210 # 2*3*5*7

.. py:method:: bin_ui(n, k)

   Python wrapper for GNU MP :C:`mpz_bin_ui()` and :C:`mpz_bin_uiui()`
   functions.

   :parameters n, k: :Py:`n` is a :py:class:`mpz` or a :py:class:`int`
		     that must fit into a C :C:`unsigned
		     long`. :Py:`k` is :py:class:`int` that must fit
		     into a C :C:`unsigned long`.
   :returns: The binomial coefficient :math:`\binom{n}{k}`.
   :raises TypeError, OverflowError:

.. py:method:: remove(z1, z2)

   Python wrapper for GNU MP :C:`mpz_remove()` function.

   :parameters z1, z2: :Py:`z1` and :Py:`z2` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: :Py:`(z, occ)` where :Py:`z` is the number :Py:`z1` in
             which all occurrences of the factor :Py:`z2` have been
             removed and :Py:`occ` is the number of these occurrences.
   :raises TypeError:

   .. code-block:: Python

      >>> print(remove(1728, 2))
      (27, 6)

.. py:method:: hamdist(z1, z2)

   Python wrapper for GNU MP :C:`mpz_hamdist()` function.

   :parameters z1, z2: :Py:`z1` and :Py:`z2` are either instances of
		       :py:class:`mpz` or :py:class:`int`.
   :returns: The hamming distance between :Py:`z1` and :Py:`z2`.
   :raises TypeError:

Random Number Functions
^^^^^^^^^^^^^^^^^^^^^^^

.. note:: GMP random number functions use a variable of type
   :C:`gmp_randstate_t` which holds an algorithm selection and a
   current state. Such a global (and internal) :C:`state` variable is
   initialized during :py:mod:`_gmp.gmpz` module initialization via
   the GMP function :C:`gmp_randinit_default()`.
 
 
.. py:method:: randseed_ui()

   Python wrapper for GNU MP :C:`mpz_randseed_ui()` function. Set an
   initial *seed* value.

   :returns: :Py:`Ǹone`

   If the special file :Bash:`/dev/random` exists on the operating
   system, *seed* will be extracted from it. Otherwise, it will be the
   system's time that will be used (less secure).

.. py:method:: urandomb_ui(n)

   Python wrapper for GNU MP :C:`mpz_urandomb_ui()` function.

   :parameters n: A :py:class:`int` that must be :Py:`<=`
		  :C:`(sizeof(unsigned long) << 3)`, otherwise
		  :py:exc:`ValueError` is raised.
   :returns: A random number (:py:class:`int`) of :Py:`n` bits.
   :raises TypeError, ValueError:

.. py:method:: urandomm_ui(n)

   Python wrapper for GNU MP :C:`mpz_urandomm_ui()` function.

   :parameters n: A :py:class:`int` that must fit into a :C:`unsigned
		  long`, otherwise :py:exc:`TypeError` is raised.
   :returns: A random number (:py:class:`int`) in range :math:`[0, n-1]`.
   :raises TypeError:

Some examples:

.. code-block:: Python
		
   >>> randseed_ui()
   >>> print(urandomb_ui(64))
   3049390686714079220 # random number of 64 bits
   >>> print(urandomm_ui(64))
   48                  # random number in interval [0, 63]

Exceptions
----------

.. py:exception:: GMPZError
