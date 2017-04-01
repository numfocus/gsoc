Port testsuite to pytest
========================


Abstract
========
Testing is crucial to the development of any software and MDAnalysis currently uses nose to test their code. 
Unfortunately, [nose](http://nose.readthedocs.io/en/latest/) is **no longer under active development** so the community has decided to shift over to [pytest](http://doc.pytest.org/en/latest/).
Another problem is the **performance** of the current testsuite in terms of execution time. Currently the TravisCI
build takes around 45 minutes (50 min. being the cap). This causes builds to fail because they are terminated
on exceeding the maximum execution time. The objective of this project is to implement this shift in a way
that existing development work is not affected and to improve performance of the existing test cases and to
make it more maintainable in general.


Current State
=============
Currently the testsuite consists of 8 modules that have a total of 4731 test cases (detected by nose). The execution
time is around 45 minutes (on TravisCI).

Running this testsuite with pytest (without any modifications) results in the following output:

`1789 failed, 2630 passed, 52 skipped, 1610 pytest-warnings, 26 error`

Also, it is to be noted that pytest misses out some (231 to be exact) test cases too. This is due to differences 
between test discovery rules of nose and pytest (explained further).


Technical Details
=================

Incompatible nose idioms
------------------------

1. Test classes with `__init__` methods are not collected by pytest.

    **Example**

    ```python
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

    **Solution**
    Switch from using `__init__` to `setUp`/`tearDown` or move things to pytest fixtures with the same logic.
    This will have to be dealt with on a case-by-case basis and will involve a lot of discussions.


2. `setUp` and `tearDown` methods are not executed in test classes that do not inherit from `TestCase`.

    **Example**

    ```python
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

    **Solution**
    Inheriting from `TestCase` fixes this problem

3. `yield` based test generators are deprecated in the current version of pytest. For now, they are executed just
fine, but a deprecation warning is displayed.

	**Example**

    ```python
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
    WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated,
     and scheduled to be removed in pytest 4.0
    WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated,
     and scheduled to be removed in pytest 4.0
    WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated,
     and scheduled to be removed in pytest 4.0
    WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated,
     and scheduled to be removed in pytest 4.0
    WC1 /Users/utkbansal/Code/mdanalysis/testsuite/MDAnalysisTests/coordinates/experiments.py yield tests are deprecated,
     and scheduled to be removed in pytest 4.0
    ```

    **Solution**
    Using `pytest.mark.parametrize` is the recommended way to port yield based tests to pytest.

    ```python
    @pytest.mark.parametrize("n, nn", [
      (0, 0),
      pytest.mark.xfail((1, 3)),
      (2, 6),
    ])


    def check_even(n, nn):
      print(n)
      assert n % 2 == 0 or nn % 2 == 0
    ```


4. Test case discovery in pytest is quite different from that in nose.

	**Example**

    ```python
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
  In short, anything that inherits from `TestCase` is collected for tests.

5. Inheritance also causes problems in pytest.

    **Example**
    ```python
    class ParentClass(TestCase):

      def test_value(self):
      	assert self.a == 2


    class TestChildClass(ParentClass):
    	a = 2
    ```

    The problem with the above example is that both the test classes are collected for tests (only `TestChildClass` should be collected)
    and hence test case in `ParentClass` fails

    **Solution**
    ```python
    class ParentClass(TestCase):
      __test__ = False

      def test_value(self):
          assert self.a == 2


    class TestChildClass(ParentClass):
      __test__ = True
      a = 2
    ```
    Here only `TestChildClass` is collected for test cases.

    In short, using the `__test__` attribute we can be explicit about which classes have to be collected for tests.


Useful pytest features
----------------------

1. Fixtures - pytest has completely different type of fixtures that are very modular and simple. The documentation
is available [here](http://doc.pytest.org/en/latest/fixture.html). Using fixtures is one of the major goals of this project as it will help in making the tests
more modular and will  speed them up (by caching parsed files among others, see [#1191](https://github.com/MDAnalysis/mdanalysis/issues/1191)).

    **Usage**

    Currently we have a class `TempDir` (`MDAnalysisTests/tempdir.py`) that is used to get temporary directory for writing files. This is used as follows -
    ```python
    class BaseWriterTest(object):
      def __init__(self, reference):
        self.ref = reference

        # Instantiating a TempDir object
        self.tmpdir = tempdir.TempDir()
        self.reader = self.ref.reader(self.ref.trajectory)

      def tmp_file(self, name):

        # Note the usage to self.tmpdir here
        return self.tmpdir.name + name + '.' + self.ref.ext

    ```

    Custom fixtures are easy to create. The tempdir instantiation part can be easily converted into a custom fixture as follows
    ```python

    # Creating a custom fixture
    @pytest.fixture
    def tmpdir():
        return tempdir.TempDir()

    class BaseWriterTest(object):
      def __init__(self, reference):
        self.ref = reference
        # Note that we do not have to initialize self.tmpdir how
        self.reader = self.ref.reader(self.ref.trajectory)

      # Note that the method now takes an additional argument - tmpdir (our fixture)
      def tmp_file(self, name, tmpdir):
        # Note the usage to tmpdir here
        return tmpdir.name + name + '.' + self.ref.ext
    ```

    Moreover fixtures have configurable scopes and can be used to cache resources that are reused, i.e. there is no need to initialize a particular resource everytime
    it is needed in a different class/function. This will lead to performance gains too and will help reduce boilerplate code further.

    ```python

    # Note the usage of scope
    @pytest.fixture(scope="function")
    def custom_fixture():
      return resource
    ```

    The scope can be "function", "class", "module", "session" or "invocation". This way we can cache resources that are being reused in other parts of the code.
    As with the above example, many parts of the code require the `tmpdir` and as of now each class instantiates its own object. The scope of this fixture may be
    changed to `"session"` so that the object is created only once.

    Pytest also has some commonly used inbuilt fixtures.
    An interesting one of them is `cache` that returns a cache object that can persist state between testing sessions.

    ```python
    cache.get(key, default)
    cache.set(key, value)
    ```

    This fixture may be used to speed up tests and TravisCI builds in particular (which take very long time as of now - approx. 50 min.)

2. Universal `assert` statements - pytest allows us to use the standard python assert for verifying expectations and values in tests. It also automagically provides
quite helpful tracebacks when tests fail.

    **Usage**

    Nose has a number of `assert_*` methods such as `assert_equal`, `assert_raises`, etc. Pytest on the other hand uses only plain `assert` statements.
    This is easier as developers don't have to remember all the methods and also pytest has awesome advanced introspection which brings great and
    informative tracebacks when test cases fail.

    In nose - `assert_equal(1,1)`
    Equivalent statement in pytest - `assert(1==1)`

    This can mostly be done using a search-replace or sed.


3. `parametrize` instead of `yield` bases test generators - The builtin `pytest.mark.parametrize` decorator enables parametrization of arguments for a test
function. This is more manageable than the nose `yield` based counterpart.

    **Usage**

    Nose uses yield based test generators-
    ```python
        def check_even(n, nn):
          print(n)
          assert n % 2 == 0 or nn % 2 == 0


        def test_evens():
          for i in range(0, 5):
          print(i)
          yield check_even, i, i * 3
    ```

    Pytest on the other hand has far better and simpler `pytest.mark.parametrize`
    ```python
       @pytest.mark.parametrize("n, nn", [
        (0, 0),
        pytest.mark.xfail((1, 3)),
        (2, 6),
       ])
       def check_even(n, nn):
          print(n)
          assert n % 2 == 0 or nn % 2 == 0
    ```


4. `raises` helper - it is used to assert that some code raises an exception and is better than the decorator that is used in nose.

    **Usage**

    ```python
    def f():
      raise SystemExit(1)

    def test_mytest():
      with pytest.raises(SystemExit):
        f()
    ```


5. `warns` helper - similar to `raises` helper, used to assert warnings.

    **Usage**

    ```python
    def test_warning():
      with pytest.warns(UserWarning):
        warnings.warn("my warning", UserWarning)
    ```


6. `pytest.mark.xfail` to check test cases that are expected to fail. Usage already shown above.

**Overall the project can be divided into three major parts which will cover issues [#884](https://github.com/MDAnalysis/mdanalysis/issues/884) Port to pytest and
[#516](https://github.com/MDAnalysis/mdanalysis/issues/516) Update coordinate tests to use base test classes.**


Part 1
------
This part will make the bare minimum changes needed to shift from nose to pytest and get TravisCI builds along with 
code coverage running as usual. This is being done in a single PR to avoid any weird coexistence of two testing 
libraries.

* Modify the code to make it discoverable by pytest (take care of `__init__` and inheritance problems as explained above)
* Port the 5 existing nose plugins to pytest. Pytest has awesome documentation on how to implement plugins which can be
 found [here](http://doc.pytest.org/en/latest/writing_plugins.html) .
    * Capture error (`capture_err.py`) This plugin captures stderr during test execution. If the test fails
    or raises an error, the captured output is appended to the error or failure output. Pytest already has this
feature inbuilt so this plugin can be dropped.

        **Example**
        ```python
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
    There is another simpler implementation suggested by the community [here](https://nvbn.github.io/2017/02/02/pytest-leaking/).
    This also needs discussion, I will have to check the performance of all the alternatives to finalize which one to use.
    * Open files (`open_files.py`) This plugin lists all open files when a test fails or errors. They also are listed at
    the end of the test suite. This plugin is currently disabled because we were facing issue with capturing stdout.
    This will have to be rewritten for pytest as no open source alternatives are available.
    * To run tests in parallel, there is a open source (MIT license) plugin [pytest-xdist](https://github.com/pytest-dev/pytest-xdist) that is recommended by pytest.
* Configure TravisCI and code coverage. This will not take much time, only a couple of commands will have to be changed.
We will be using the command line parameters directly i.e. no wrappers (like the one we currently use with nose)


**Challenges** The main challenge in this part is to not loose any tests in the conversion process. Code coverage data can be used to identify parts where this
 problem occurs.
It is to be noted that custom plugins are a major cause for the slowdown of tests. Some discussion is being done [here](https://github.com/MDAnalysis/mdanalysis/issues/1191) .
Plugins need to be profiled extensively to get rid of any potential bottlenecks.

Part 2
------
This part will focus on modifying tests to use pytest features such as -
* assert statements (will help make the code more maintainable)
* fixtures (this is the single most important fix that will improve performance a lot)
* parameterize in place of yield (improves maintainability)
* raises & warns helper (improves maintainability)

PRs will be made on a per file basis.
The following modules will be fixed in this part -

* `analysis` (21 files)
* `auxiliary` (2 files)
* `core` (17 files)
* `coordinates` (26 files)
* `formats` (1 file)
* `lib` (1 file)
* `topology` (20 files)
* Files in the root of `testsuite` directory (19 files)

The main focus of this part will be the performance aspect of the tests. Module wide fixtures should help
bring down the execution time vastly.



Part 3 (Optional)
-----------------
This part will focus on the coordinates module and its tests. The coordinates module is a very important part of
MDAnalysis and any bugs in it will result in error in further calculations. Currently the coordinates module has not
been tested in a consistent fashion. Also all the reader/writers don’t necessarily follow the standard API See [#516](https://github.com/MDAnalysis/mdanalysis/issues/516).

* Modify Reader/Writer API for files in the coordinates module for to follow the standard API.
* Modify coordinates tests to use base Reader/Writer test classes. This will help to set a baseline that all
readers/writers will have to conform to and at the same time avoid code duplication among test classes.

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


**Challenges** The main challenge in this part is to modify (if needed) the reader/writer API of the coordinates module to
follow the API standard. Here my mentor can point out any problems with the changes via reviews. Again, this is because
I want to do PRs on a per-file basis - it will be easier to review and get merged.


I want to work on parts 2 and 3 on a per-file basis because I find small changes are easy to review and get merged. Also
this way, I will be able to stick to my proposed timeline. This will also help my mentors to easily keep track of my progress
and help me sooner, if I were to start lagging behind or go off track.


Other Research
--------------
Just to get an approximate how much time _part 1_ of this project would take, I went ahead and did a small part of it on
my fork. I worked on the `coordinates` module (which is also the largest module) and made some changes to make it work
with pytest. Here are my findings

**Change #1** Changed all classes that have either setUp or tearDown methods to inherit from TestCase.

**Before** `580 failed, 954 passed, 31 skipped, 505 pytest-warnings, 26 error in 146.41 seconds`

**After**  `171 failed, 1361 passed, 87 skipped, 42 pytest-warnings, 50 error in 177.44 seconds`

In short, there are around 1600 test cases, by changing the base class I got around 400 more to run successfully with pytest.

**Change #2** Use `__test__` attribute in classes which are either inheriting from other base test classes & classes that
are being inherited from i.e. both parent and child classes.

**Before** `171 failed, 1361 passed, 87 skipped, 42 pytest-warnings, 50 error in 177.44 seconds`

**After**  `1408 passed, 68 skipped`

In short, _none of the test cases fail_.

It is to be noted that pytest collects around 270 test cases less that nosetests, that is largely because of the fact that
pytest does not collect classes with an `__init__` method (which I've explained above)

Also to get an approximate of how much time and effort _part 2_ would take, I have worked on a part of issue [#516](https://github.com/MDAnalysis/mdanalysis/issues/516)
where I fixed the `GRO` writers API to follow the reader API standard and modified test classes to inherit from base test classes.
See [#1196](https://github.com/MDAnalysis/mdanalysis/pull/1196)


Timeline
========

### May 4th - May 29th, **Community Bonding Period**
* Setup and configure TravisCI, QuantifiedCode and related tools on my fork of MDAnalysis.
* Setup communications with mentor.
* Profile the memory leak plugin alternatives and decide which one will be used.
* Discuss the `__init__` issue with the community and finalize the pattern to deal with it.

### May 30th - June 3rd
* Begin work on **Part 1**. Start by editing tests to make them discoverable by pytest. Move `__init__` code to
`setUp` and use `__test__` attribute.
* Send a PR only for review and discussion purposes. It will _not_ be ready to merge.

### June 5th - June 9th
* Complete the test discovery issue and get `knownfailure`, `capture_err` and `xdist` plugins working with pytest.
* Work is done on the same PR. Still _not_ ready to merge.
* Get a _bare minimum_ TravisCI setup to work.

### June 12th - June 16th
* Get `cleanup`, `memleak` and `open_files` plugin to work.

### June 19th - June 23th, **End of Phase 1**
* Complete configuring TravisCI with plugins, code coverage and quantified code.
* Final review, documentation and code cleanup as suggested.
* Get the PR merged. This completes the basic shift to pytest.

### June 26 - June 30th, **Begin of Phase 2**
* Begin work on **Part 2**.
* Start with the `analysis` module and work on 5 files.
* PRs are submitted on a per-file basis and are worked on in parallel.

### July 3rd - July 7th
* Wrap up the `analysis` module. Submit PRs on a per-file basis.

### July 10th - July 14th
* Port `auxiliary` module - multiple PRs

### July 17th - July 21th, **End of Phase 2**
* Port `coordinates` modules - multiple PRs

### July 24th - July 28th, **Begin of Phase 3**
* Port `core` module - multiple PRs

### July 31st - August 4th
* Port `topology` module - multiple PRs

### August 7th - August 11th
* Port files in the root directory, `lib` and `formats` modules - multiple PRs

### August 14th - August 18th
* By now module wide fixtures have been implemented. Have a final look at the test performance.
Identify any remaining major bottlenecks. Discuss with the community and work on them to bring
down execution time further.

### August 21st - August 25th, **Final Week**
* This week is set aside as a buffer to negate any time lost/lag in work. Follow up on reviews and get PRs
merged into develop.

### August 28th - August 29th, **Submit final work**
* Submit all the work.

## Future works
All the points mentioned here are _optional_ and may be done if I'm ahead of schedule

* Part 3 (as explained above)
* Add support for doctests.

Contributions to MDAnalysis
===========================
* Universe now woks with StringIO objects [#1254](https://github.com/MDAnalysis/mdanalysis/pull/1254) (Work in progress)
* Adds filename attribute to MemoryReader & associated tests [#1252](https://github.com/MDAnalysis/mdanalysis/pull/1252)
* Port GRO tests to new BaseReader/Writer Test classes [#1196](https://github.com/MDAnalysis/mdanalysis/pull/1196)
* Add support for start and stop in transfer_to_memory [#1189](https://github.com/MDAnalysis/mdanalysis/pull/1189)
* Fix error message in case of unidentified file format [#1184](https://github.com/MDAnalysis/mdanalysis/pull/1184)
* Raises IOError if the toplogy file doesn't exist or can't be accessed [#1177](https://github.com/MDAnalysis/mdanalysis/pull/1177)


Contributions to other Open Source Projects
===========================================
* https://github.com/mozilla/bedrock/pull/2697
* https://github.com/mozilla/bedrock/pull/2699
* https://github.com/mozilla/kuma/pull/3131
* https://github.com/rrmerugu/django-seed/pull/5
* https://github.com/rrmerugu/django-seed/pull/4
* Also built an open source library for django forms [here](https://github.com/utkbansal/crispy-forms-materialize).


Personal Information
===================

**Name**: Utkarsh Bansal

**Email**: bansalutkarsh3@gmail.com

**Github**: utkbansal

**Blog**: http://utkarshbansal.me

**Timezone**: UTC + 5:30

Why are you interested in working with us?
------------------------------------------


Have you used MDAnalysis for your research already?
---------------------------------------------------

No, I haven't done any research yet.


Do you have any experience in programming?
------------------------------------------

Yes, I've been programming for over 3.5 years now. I mostly work with Python & iOS. You can find a lot of my work on my Github profile [here](https://github.com/utkbansal) .
I've also been actively engaged with [@Software-Incubator](https://github.com/Software-Incubator), the software development center of my college where I have
contributed to various projects which include websites, REST APIs, web based games and iOS applications which are all open
source.

Do you have any exams during GSoC or plan a vacation during the summer?
-----------------------------------------------------------------------

Yes, I have a couple of exams during the community bonding period but that will not be longer than a week (3rd week of May). I can continue
discussions during that period.


Relevant Discussions and References
===================================
* Port to pytest thread on mailing list. https://groups.google.com/forum/#!topic/mdnalysis-devel/uzTwpc8FiNs
* Discussions about this application. https://groups.google.com/forum/#!topic/mdnalysis-devel/5sXPZsJXecs
* Pytest documentation. http://doc.pytest.org/en/latest/
* Nose documentation. http://nose.readthedocs.io/en/latest/
