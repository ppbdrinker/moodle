PHPUnit testing support in Moodle
==================================


Documentation
-------------
* [Moodle Dev wiki](http://docs.moodle.org/dev/PHPUnit)
* [PHPUnit online documentaion](http://www.phpunit.de/manual/current/en/)
* [MDL-32323](http://tracker.moodle.org/browse/MDL-32323), [MDL-32149](http://tracker.moodle.org/browse/MDL-32149), [MDL-31857](http://tracker.moodle.org/browse/MDL-31857)


Installation
------------
1. install PHPUnit PEAR extension - see [PHPUnit docs](http://www.phpunit.de/manual/current/en/installation.html) for more details
2. edit main config.php - add $CFG->phpunit_prefix and $CFG->phpunit_dataroot - see config-dist.php for more details
3. execute `php admin/tool/phpunit/cli/util.php --install` from dirroot to initialise test database and dataroot
4. it is necessary to reinitialise the test database manually after every upgrade or installation of new plugins


Test execution
--------------
* optionally generate phpunit.xml by executing `php admin/tool/phpunit/cli/util.php --buildconfig` - it collects test cases from all plugins
* execute `phpunit` shell command from dirroot directory
* you can also execute a single test `phpunit core_phpunit_basic_testcase lib/tests/phpunit_test.php`
* or all tests in one directory `phpunit --configuration phpunit.xml lib/tests/*_test.php`
* it is possible to create custom configuration files in xml format and use `phpunit -c myconfig.xml`


How to add more tests?
----------------------
1. create `tests` directory in your plugin if does not already exist
2. add `*_test.php` files with custom class that extends `basic_testcase` or `advanced_testcase`
3. execute your new test, for example `phpunit core_phpunit_basic_testcase local/mytest/tests/mytest_test.php`
4. optionally build new phpunit.xml by executing `php admin/tool/phpunit/cli/util.php --buildconfig` and run all tests
5. core developers have to add all core unit test locations to `phpunit.xml.dist`


How to convert existing tests?
------------------------------
1. create new test file in `xxx/tests/yyy_test.php`
2. copy contents of the old test file
3. replace `extends UnitTestCase` with `extends basic_testcase`
4. fix setUp, tearDown, asserts, etc.
5. some old SimpleTest tests can be executed directly - mocking, database operations, assert(), etc. does not work, you may need to add `global $CFG;` before includes


FAQs
----
* Why is it necessary to execute the tests from the command line? PHPUnit is designed to be executed from shell, existing Moodle globals and constants would interfere with it.
* Why `tests` subdirectory? It is very unlikely that it will collide with any plugin name because plugin names use singular form.
* Why is it necessary to include core and plugin suites in configuration files? PHPUnit does not seem to allow dynamic loading of tests from our dir structure.


TODO
----
* add plugin callbacks to data generator
* convert remaining tests
* delete all simpletests
* hide old SimpleTests in UI and delete Functional DB tests
* shell script that prepares everything for the first execution
* optional support for execution of tests and cli/util.php from web UI (to be implemented via shell execution)
