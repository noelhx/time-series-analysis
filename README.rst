====================
Time Series Analysis
====================
Contains tools for analyzing time-series data.

Command-Line Tools
------------------
The *analyze_series* command is meant to be used in the command-line in typical `*nix` fashion, expecting:

* one number per line
* input on standard input or as a file argument to analyze_series

*analyze_series* will analyze the points in the provided series and print the following descriptive statistics:

* median
* mean, sample standard deviation
* upper and lower control limits
* points and associated values falling outside of control limits

*analyze_series* can be used to help implement a run-by-run control program using `Control Charts`_.  *analyze_series* currently supports `'Rule 1'`_ testing for points that are
+/- 3-sigma from the series' mean.  Support for detecting suspicious runs above/below the mean is planned (`issue #3`_).

Usage
~~~~~
execute analyze_series -h for full usage details.

excerpt::

    usage: analyze_series [-h] [-v] [--assert-last-point-in-control] [input_file]

    positional arguments:
      input_file            input file to process, defaults to standard input

    optional arguments:
      -h, --help            show this help message and exit
      -v, --verbose         print additional debug information
      --assert-last-point-in-control
                            assert the last point in the series is in-control and
                            exit with status code '2' if last point is not in
                            control


Examples
~~~~~~~~~
Analyze a file on the filesystem::

    $ ./analyze_series resources/examples/multiple_control_limit_violation.txt
    median: 0.0
    mean: 0.0357142857143
    std dev: 1.41375432756
    lower control limit: -4.20554869697
    upper control limit: 4.2769772684
    points outside of lcl: [(3, -5.0)]
    points outside of ucl: [(28, 7.0)]


Analyze data arriving on standard input::

    $ cat resources/examples/under_control.txt | ./analyze_series
    median: 10.05
    mean: 10.05
    std dev: 0.873689494805
    lower control limit: 7.42893151558
    upper control limit: 12.6710684844
    points outside of lcl: None
    points outside of ucl: None


Analyze data and return error if last point is not in control::

    $ cat resources/examples/upper_control_limit_violation.txt | ./analyze_series --assert-last-point-in-control; echo "exit status: $?"
    median: 0.0
    mean: 0.142857142857
    std dev: 1.11269728053
    lower control limit: -3.19523469873
    upper control limit: 3.48094898444
    points outside of lcl: None
    points outside of ucl: [(27, 4.0)]
    stderr: last point (index=27, value=4.0) is out of control
    exit status: 1


Acknowledgement and Thanks
--------------------------
The time-series-analysis project builds-upon and is made-easy by the wonderful work of:

* Python_
* NumPy_ - the fundamental package for scientific computing with Python
* pandas_ - provides high-performance, easy-to-use data structures and data analysis tools for Python


Thank you!

.. _Python: http://www.python.org
.. _NumPy: http://www.numpy.org
.. _pandas: http://pandas.pydata.org
.. _Control Charts: http://en.wikipedia.org/wiki/Control_chart
.. _'Rule 1': http://en.wikipedia.org/wiki/Western_Electric_rules
.. _issue #3: https://github.com/skuenzli/time-series-analysis/issues/3
