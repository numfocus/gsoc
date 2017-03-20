Port testsuite to pytest
========================


Abstract
========
Testing is crucial to the development of any software and MDAnalysis currently uses nose to test their code. 
Unfortunately, [nose](http://nose.readthedocs.io/en/latest/) is no longer under active development so the community has decided to shift over to [pytest](http://doc.pytest.org/en/latest/) .
The objective of this project is to implement this shift in a way that existing development work is not affected
and to standardise and improve the existing test cases.


Current State
=============
Currently the testsuite consists of 8 modules that have a total of 4731 test cases. Running this testsuite with 
pytest (without any modifications) results in the following output:

`1789 failed, 2630 passed, 52 skipped, 1610 pytest-warnings, 26 error`

Also, it is to be noted that pytest misses out some (231 to be exact) test cases too. This is due to differences 
between test discovery rules of nose and pytest.


Technical Details
=================

Incompatible nose idioms
------------------------

1. Test classes with `__init__` methods are not collected by pytest.

     **Example**

     ```
     class TestGROReader(BaseReaderTest):
       def __init__(self, reference=None):
         if reference is None:
         reference = GROReference()
         super(TestGROReader, self).__init__(reference)

       def test_flag_convert_lengths(self):
         assert_equal(mda.core.flags['convert_lengths'], True,
         "MDAnalysis.core.flags['convert_lengths'] should be True "
         "by default")

       def test_time(self):
         u = mda.Universe(self.ref.topology, self.ref.trajectory)
         assert_equal(u.trajectory.time, 0.0,
         "wrong time of the frame")

       def test_full_slice(self):
         u = mda.Universe(self.ref.topology, self.ref.trajectory)
         trj_iter = u.trajectory[:]
         frames = [ts.frame for ts in trj_iter]
         assert_equal(frames, np.arange(u.trajectory.n_frames))
	```


    This class is not collected for tests because `__init__` is present


2. `setUp` and `tearDown` methods are not executed in test classes that do not inherit from `TestCase`.

    **Example**

    ```
    class TestGROIncompleteVels(object):
      def setUp(self):
      	self.u = mda.Universe(GRO_incomplete_vels)

      def tearDown(self):
      	del self.u

      def test_load(self):
      	assert_equal(len(self.u.atoms), 4)

      def test_velocities(self):
        assert_array_almost_equal(self.u.atoms[0].velocity,
        np.array([ 79.56,  124.08,   49.49]),
        decimal=3)
        assert_array_almost_equal(self.u.atoms[2].velocity,
        np.array([0.0, 0.0, 0.0]),
        decimal=3)
    ```

    **Output**
    `AttributeError - 'TestGROIncompleteVels' object has no attribute 'u'`

3. `yield` based test generators are deprecated in the current version of pytest. For now the are executed just
fine, but a deprecation warning is displayed.

	**Example**

    ```
    def check_even(n, nn):
      print(n)
      assert n % 2 == 0 or nn % 2 == 0


    def test_evens():
      for i in range(0, 5):
      print(i)
      yield check_even, i, i * 3

	```

   **Warning**

   ```
   WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated, and scheduled to be removed in pytest 4.0
   WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated, and scheduled to be removed in pytest 4.0
   WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated, and scheduled to be removed in pytest 4.0
   WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated, and scheduled to be removed in pytest 4.0
   WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated, and scheduled to be removed in pytest 4.0
   ```


4. Test case discovery in pytest is quite different from that in nose.

	**Example**

    ```
    class _IgnoreClass(TestCase):

      def test_number(self):
      	assert 2 == 2

      def test_bool(self):
      	assert True is True

      def dont_collect(self):
      	assert 1 == 1


    class IgnoreClass(object):
      def test_number(self):
      	assert 2 == 2

      def test_bool(self):
      	assert True is False
	```

  In the above case,  `_IgnoreClass` is collected even tough it doesn't follow the naming convention required for test discovery and the class `IgnoreClass` is not collected.

5. Inheritance also causes problems in pytest.

    **Example**
    ```
    class ParentClass(TestCase):

      def test_value(self):
      	assert self.a == 2


    class TestChildClass(ParentClass):
    	a = 2
    ```
    The problem with the above example is that both the test classes are collected for tests (only `TestChildClass` should be collected)
    and hence test case in `ParentClass` fails

    **Solution**
    ```
    class ParentClass(TestCase):
      __test__ = False

      def test_value(self):
          assert self.a == 2


    class TestChildClass(ParentClass):
      __test__ = True
      a = 2
    ```
    Here only `TestChildClass` is collected for test cases.




The project can be divided into three major parts which will cover issues [#884](https://github.com/MDAnalysis/mdanalysis/issues/884) and [#516](https://github.com/MDAnalysis/mdanalysis/issues/516)


Part 1
------
This part will make the bare minimum changes needed to shift from nose to pytest and get TravisCI builds along with 
code coverage running as usual. This is being done in a single PR to avoid any weird coexistence of two testing 
libraries.

* Modify the code to make it discoverable by pytest (take care of `__init__` and inheritance problems as explained above)
* Port the 5 existing nose plugins to pytest -
    * Capture error (`capture_err.py`) This plugin captures stderr during test execution. If the test fails
    or raises an error, the captured output is appended to the error or failure output. Pytest already has this
feature inbuilt so this plugin can be dropped.

        **Example**
        ```
        def check_even(n, nn):
            print(n)
            assert n % 2 == 0 or nn % 2 == 0


        def test_evens():
            for i in range(0, 5):
                yield check_even, i, i * 3
        ```

        **Output**
        ```
        ======================================= FAILURES =======================================
        ____________________________________ test_evens[1] _____________________________________

        n = 1, nn = 3

            def check_even(n, nn):
                print(n)
        >       assert n % 2 == 0 or nn % 2 == 0
        E       assert ((1 % 2) == 0 or (3 % 2) == 0)

        experiments.py:37: AssertionError
        --------------------------------- Captured stdout call ---------------------------------
        1
        ____________________________________ test_evens[3] _____________________________________

        n = 3, nn = 9

            def check_even(n, nn):
                print(n)
        >       assert n % 2 == 0 or nn % 2 == 0
        E       assert ((3 % 2) == 0 or (9 % 2) == 0)

        experiments.py:37: AssertionError
        --------------------------------- Captured stdout call ---------------------------------
        3
        ```


    * Cleanup (`cleanup.py`) This plugin deletes XDR offset files after all testing is done. This will have to
    be re-written as a pytest plugin.
    * Knownfailure (`knownfailure.py`) This is a decorator to check tests that are expected to fail. Pytest has
    something similar to this called `xfail`. The documentation for this is available [here](http://doc.pytest.org/en/latest/skipping.html#mark-a-test-function-as-expected-to-fail) .
    This needs discussion to finalize if we need to port the existing code or if `xfail` is good enough for our use.
    * Memory leak (`memleak.py`) This plugin detects memory leaks in tests. There is an open-source (MIT license) plugin, [pytest-leaks](https://github.com/abalkin/pytest-leaks).
    This also needs discussion to finalize if we need to port the existing code or if pytest-leaks is good enough for our use.
    * Open files (`open_files.py`) This plugin lists all open files when a test fails or errors. They also are listed at
    the end of the test suite. This plugin is currently disabled because we were facing issue with capturing stdout. Discussion
    is needed to decide if we need this plugin anymore. If we do, this will have to be rewritten for pytest as no open source
    alternatives are available.
* Configure TravisCI and code coverage
* Take care of time limit for a single test case

Part 2 
------
This part will focus on the coordinates module and its tests. The coordinates module is a very important part of 
MDAnalysis and any bugs in it will result in error in further calculations. Currently the coordinates module has not 
been tested in a consistent fashion. Also all the reader/writers don’t necessarily follow the standard API.

* Modify Reader/Writer API for files in the coordinates module for to follow the standard API.
* Modify coordinates tests to use base Reader/Writer test classes. This will help to set a baseline that all 
readers/writers will have to conform to and at the same time avoid code duplication among test classes.
* Modify code to make use of pytest only features such as assert statements and fixtures.

This part will be split into multiple pull requests, ideally one pull request for each coordinate test file.
The following files will be fixed in this part -
* `test_amber_inpcrd.py`
* `test_chainreader.py`
* `test_crd.py`
* `test_dcd.py`
* `test_dlpoly.py`
* `test_dms.py`
* `test_gms.py`
* `test_gro.py`
* `test_lammps.py`
* `test_memory.py`
* `test_mmtf.py`
* `test_mol2.py`
* `test_netcdf.py`
* `test_null.py`
* `test_pdb.py`
* `test_pdbqt.py`
* `test_pqr.py`
* `test_reader_api.py`
* `test_timestep_api.py`
* `test_trj.py`
* `test_trz.py`
* `test_writer_registration.py`
* `test_xdr.py`
* `test_xyz.py`
* `base.py`
* `reference.py`


Part 3
------
This part will focus on the remaining tests and will modify them to use pytest features such as assert statements and
fixtures. This part should go relatively smoothly because I will be able to follow the patterns and conventions 
formed while working on the second part of the project.


Personal Information
===================

**Name**: Utkarsh Bansal

**Email**: bansalutkarsh3@gmail.com

**Github**: utkbansal

**Blog**: utkarshbansal.me

**Timezone**: UTC + 5:30

I am a 4th year undergraduate student pursuing my B.Tech. in Computer Science and Engineering.

Why are you interested in working with us?
-----------------------------------------

Have you used MDAnalysis for your research already?
--------------------------------------------------
No. I haven’t done any research.

Do you have any experience programming?
----------------------------------------
Yes, I have over 3 years of experience with Python and

Do you have any exams during GSoC or plan a vacation during the summer?
----------------------------------------------------------------------
Yes


# Port testsuite to pytest

## Abstract

{{ Abstract }}

## Technical Details

{{
Long description of the project.
**Must** include all technical details of the projects like libraries involved.
}}

## Schedule of Deliverables

### May 4th - May 29th, **Community Bonding Period**

{{ Delieverables }}

### May 30th - June 3rd

{{ Delieverables }}

### June 5th - June 9th

{{ Delieverables }}

### June 12th - June 16th

{{ Delieverables }}

### June 19th - June 23th, **End of Phase 1**

{{ Delieverables }}

### June 26 - June 30th, **Begin of Phase 2**

{{ Delieverables }}

### July 3rd - July 7th

{{ Delieverables }}

### July 10th - July 14th

{{ Delieverables }}

### July 17th - July 21th, **End of Phase 2**

{{ Delieverables }}

### July 24th - July 28th, **Begin of Phase 3**

{{ Delieverables }}

### July 31st - August 4th

{{ Delieverables }}

### August 7th - August 11th

{{ Delieverables }}

### August 14th - August 18th

{{ Delieverables }}

### August 21st - August 25th, **Final Week**

{{ Delieverables }}

### August 28th - August 29th, **Submit final work**

## Future works

{{ Future works }}

## Development Experience

{{ Development Experience }}

## Other Experiences

{{ Experience }}

## Why this project?

{{ Why you want to do this project? }}

Relevant Discussions and References
===================================
