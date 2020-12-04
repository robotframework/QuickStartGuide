.. default-role:: code

=======================================================
Руководство по быстрому началу работы с Robot Framework
=======================================================

Copyright © Nokia Networks. Распространяется под лицензией
`Creative Commons Attribution 3.0 Unported`__.

__ http://creativecommons.org/licenses/by/3.0/

.. contents:: Table of contents:
   :local:
   :depth: 2

Введение
========

Об этом руководстве
-------------------
*Руководство по быстрому началу работы с Robot Framework* представляет
наиболее важные возможности `Robot Framework <http://robotframework.org>`_.
Вы можете просто просмотреть его и представленные в нем примеры, но его
можно использовать и как `исполняемое демо`__. Все описанные здесь возможности
фреймворка более подробно разобраны в `Руководстве пользователся Robot Framework`_.

__ `Executing this guide`_
.. _Руководстве пользователся Robot Framework: http://robotframework.org/robotframework/#user-guide

Обзор Robot Framework
---------------------

`Robot Framework`_ это фреймоворк автоматизированного тестирования общего назначения
с открытым исходным кодам. Он предназначен для приемочного тестирования и разработки
на основе приемочого тестирования (ATDD). Фреймворк использует удобный синтаксис
табулированных тестовых данных, а длля построения тестов использует подход на базе
ключевых слов. Возможности фреймворка могут быть расширены за счет бибилиотек, написанных
на  Python или Java. Пользователи могут создавать собственные ключевые слова высокого
уровня из уже имеющихся, используя тот же синтаксис, что используется при создании
тестовых сценариев.

Robot Framework не привязан к операционным системам или приложения. Его ядро реализовано
на `Python <http://python.org>`_ и может быть запущено также на
`Jython <http://jython.org>`_ (JVM) и `IronPython <http://ironpython.net>`_
(.NET). Фреймворк имеет развитую экосистему из различных библиотек и сопутствующих
инструментов, разработанных в отдельных от него проектах.

За дальнейшей информацие о Framework и его экосистеме обращайтесь к
http://robotframework.org. Там вы найдете еще больше документации, демонстрационных
проектов, списки тестовых библиотек, сопутствующих инструментов и тому подобное.

Demo application
----------------

The sample application for this guide is a variation on a classic login
example: it is a command-line based authentication server written in Python.
The application allows a user to do three things:

- Create an account with a valid password.
- Log in with a valid user name and password.
- Change the password of an existing account.

The application itself is in `<sut/login.py>`_ file and can be executed with
a command `python sut/login.py`. Attempting to log in with a non-existent
user account or with an invalid password results in the same error message::

    > python sut/login.py login nobody P4ssw0rd
    Access Denied

After creating a user account with valid password login succeeds::

    > python sut/login.py create fred P4ssw0rd
    SUCCESS

    > python sut/login.py login fred P4ssw0rd
    Logged In

There are two requirements that a password must fulfill to be valid: it must
be between 7-12 characters long, and it must contain lower and upper case
letters and numbers, but it must not contain special characters. Trying to
create a user with invalid password fails::

    > python sut/login.py create fred short
    Creating user failed: Password must be 7-12 characters long

    > python sut/login.py create fred invalid
    Creating user failed: Password must be a combination of lowercase and
    uppercase letters and numbers

Changing password with invalid credentials results in the same error message
as logging in with invalid credentials. The validity of new password is
verified and if not valid, an error message is given::

    > python sut/login.py change-password fred wrong NewP4ss
    Changing password failed: Access Denied

    > python sut/login.py change-password fred P4ssw0rd short
    Changing password failed: Password must be 7-12 characters long

    > python sut/login.py change-password fred P4ssw0rd NewP4ss
    SUCCESS

The application uses a simple database file to keep track on user statuses.
The file is located in operating system dependent temporary directory.

Executing this guide
====================

These instructions explain how to run this guide yourself. If you are not
interested in that, you can nevertheless `view the results`__ online.

__ `Viewing results`_

Installations
-------------

The recommended approach to install Robot Framework on Python_ is using `pip
<http://pip-installer.org>`_. Once you have both of these preconditions
installed, you can simply run::

    pip install robotframework

See `Robot Framework installation instructions`_ for alternative installation
approaches and more information about installation in general.

This demo is written using reStructuredText__ markup language with Robot
Framework test data in code blocks. Executing tests in this format requires
installing additional docutils__ module::

    pip install docutils

Notice that Robot Framework 3.0 is the first Robot Framework version to support
Python 3. See the aforementioned `installation instructions`_ for information
about Python 2 vs Python 3.

.. _`Robot Framework installation instructions`:
   https://github.com/robotframework/robotframework/blob/master/INSTALL.rst
.. _`installation instructions`: `Robot Framework installation instructions`_
__ http://docutils.sourceforge.net/rst.html
__ https://pypi.python.org/pypi/docutils

Execution
---------

After installations you still need to get the demo itself. It is easiest to
download a certain release__ or grab the `latest content`__ and extract the
package somewhere, but it is also possible to clone the `project repository`__.

After installations and with all other preconditions in place, you can run
the demo on the command line by using the `robot` command::

    robot QuickStart.rst
    
If you use Robot Framework 2.9 or older, instead of the `robot` command you
need to use `pybot`::

    pybot QuickStart.rst

You can also configure the execution with various command line options::

    robot --log custom_log.html --name Custom_Name QuickStart.rst

For a list of available options run `robot --help`.

__ https://github.com/robotframework/QuickStartGuide/releases
__ https://github.com/robotframework/QuickStartGuide/archive/master.zip
__ https://github.com/robotframework/QuickStartGuide

Viewing results
---------------

Running the demo generates the following three result files. These files are
linked to pre-executed files available online, but executing the demo creates
them locally.

`report.html <http://robotframework.org/QuickStartGuide/report.html>`__
    Higher level test report.
`log.html <http://robotframework.org/QuickStartGuide/log.html>`__
    Detailed test execution log.
`output.xml <http://robotframework.org/QuickStartGuide/output.xml>`__
    Results in machine readable XML format.

Test cases
==========

Workflow tests
--------------

Robot Framework test cases are created using a simple tabular syntax. For
example, the following table has two tests:

- User can create an account and log in
- User cannot log in with bad password

.. code:: robotframework

    *** Test Cases ***
    User can create an account and log in
        Create Valid User    fred    P4ssw0rd
        Attempt to Login with Credentials    fred    P4ssw0rd
        Status Should Be    Logged In

    User cannot log in with bad password
        Create Valid User    betty    P4ssw0rd
        Attempt to Login with Credentials    betty    wrong
        Status Should Be    Access Denied

Notice that these tests read like manual tests written in English rather
than like automated test cases. Robot Framework uses the keyword-driven
approach that supports writing tests that capture the essence of the actions
and expectations in natural language.

Test cases are constructed from keywords and their possible arguments. The
syntax requires that keywords and arguments, as well as settings and their
values, are separated by at least two spaces or by a tab character. It is
generally recommended to use four spaces to make the separator more explicit,
and in some cases aligning arguments or other values may make the data easier
to understand. For more details about the syntax see `Robot Framework User
Guide`_.

Higher-level tests
------------------

Test cases can also be created using only high-level keywords that take no
positional arguments. This style allows using totally free text which is
suitable for communication even with non-technical customers or other project
stakeholders. This is especially important when using the `acceptance
test-driven development`__ (ATDD) approach or any of its variants and created
tests act also as requirements.

Robot Framework does not enforce any particular style for writing test cases.
One common style is the *given-when-then* format popularized by
`behavior-driven development`__ (BDD):

.. code:: robotframework

    *** Test Cases ***
    User can change password
        Given a user has a valid account
        When she changes her password
        Then she can log in with the new password
        And she cannot use the old password anymore

__ http://en.wikipedia.org/wiki/Acceptance_test-driven_development
__ http://en.wikipedia.org/wiki/Behavior_driven_development

Data-driven tests
-----------------

Quite often several test cases are otherwise similar but they have slightly
different input or output data. In these situations *data-driven tests*
allows varying the test data without duplicating the workflow. With Robot
Framework the `[Template]` setting turns a test case into a data-driven test
where the template keyword is executed using the data defined in the test case
body:

.. code:: robotframework

    *** Test Cases ***
    Invalid password
        [Template]    Creating user with invalid password should fail
        abCD5            ${PWD INVALID LENGTH}
        abCD567890123    ${PWD INVALID LENGTH}
        123DEFG          ${PWD INVALID CONTENT}
        abcd56789        ${PWD INVALID CONTENT}
        AbCdEfGh         ${PWD INVALID CONTENT}
        abCD56+          ${PWD INVALID CONTENT}

In addition to using the `[Template]` setting with individual tests, it would
be possible to use the `Test Template` setting once in the settings table like
`setups and teardowns`_ defined later in this guide. In our case that
would ease creating separate named tests for invalid length password cases and
for other invalid cases. However, that would require moving those tests to a
separate file, because otherwise the template would also be applied to other
tests in this file.

Notice also that the error messages in the above example are specified using
variables_.

Keywords
========

Test cases are created from keywords that can come from two sources. `Library
keywords`_ come from imported test libraries, and so called `user keywords`_
can be created using the same tabular syntax that is used for creating test
cases.

Library keywords
----------------

All lowest level keywords are defined in test libraries which are implemented
using standard programming languages, typically Python or Java. Robot Framework
comes with a handful of `test libraries`_ that can be divided to *standard
libraries*, *external libraries* and *custom libraries*. `Standard libraries`_
are distributed with the core framework and included generic libraries such as
`OperatingSystem`, `Screenshot` and `BuiltIn`, which is special because its
keywords are available automatically. External libraries, such as
Selenium2Library_ for web testing, must be installed separately. If available
test libraries are not enough, it is easy to `create custom test libraries`__.

To be able to use keywords provided by a test library, the keywords must be
imported using the `Library` setting. Tests in this guide need keywords from
the standard `OperatingSystem` library (e.g. `Remove File`) and from a custom
made `LoginLibrary` (e.g.  `Attempt to login with credentials`). Both of these
libraries are imported in the settings table below:

.. code:: robotframework

    *** Settings ***
    Library           OperatingSystem
    Library           lib/LoginLibrary.py

.. _Test libraries: http://robotframework.org/#libraries
.. _Standard libraries: http://robotframework.org/robotframework/#standard-libraries
.. _Selenium2Library: https://github.com/rtomac/robotframework-selenium2library/#readme
__ `Creating test libraries`_

User keywords
-------------

One of the most powerful features of Robot Framework is the ability to easily
create new, higher-level keywords from other keywords. The syntax for creating
these so called *user-defined keywords*, or *user keywords* for short, is
similar to the syntax that is used for creating test cases. All the
higher-level keywords needed in previous test cases are created in this
keyword table:

.. code:: robotframework

    *** Keywords ***
    Clear login database
        Remove file    ${DATABASE FILE}

    Create valid user
        [Arguments]    ${username}    ${password}
        Create user    ${username}    ${password}
        Status should be    SUCCESS

    Creating user with invalid password should fail
        [Arguments]    ${password}    ${error}
        Create user    example    ${password}
        Status should be    Creating user failed: ${error}

    Login
        [Arguments]    ${username}    ${password}
        Attempt to login with credentials    ${username}    ${password}
        Status should be    Logged In

    # Keywords below used by higher level tests. Notice how given/when/then/and
    # prefixes can be dropped. And this is a comment.

    A user has a valid account
        Create valid user    ${USERNAME}    ${PASSWORD}

    She changes her password
        Change password    ${USERNAME}    ${PASSWORD}    ${NEW PASSWORD}
        Status should be    SUCCESS

    She can log in with the new password
        Login    ${USERNAME}    ${NEW PASSWORD}

    She cannot use the old password anymore
        Attempt to login with credentials    ${USERNAME}    ${PASSWORD}
        Status should be    Access Denied

User-defined keywords can include actions defined by other user-defined or
library keywords. As you can see from this example, user-defined keywords can
take parameters. They can also return values and even contain FOR loops. For
now, the important thing to know is that user-defined keywords enable test
creators to create reusable steps for common action sequences. User-defined
keywords can also help the test author keep the tests as readable as possible
and use appropriate abstraction levels in different situations.

Variables
=========

Defining variables
------------------

Variables are an integral part of the Robot Framework. Usually any data used in
tests that is subject to change is best defined as variables. Syntax for
variable definition is quite simple, as seen in this variable table:

.. code:: robotframework

    *** Variables ***
    ${USERNAME}               janedoe
    ${PASSWORD}               J4n3D0e
    ${NEW PASSWORD}           e0D3n4J
    ${DATABASE FILE}          ${TEMPDIR}${/}robotframework-quickstart-db.txt
    ${PWD INVALID LENGTH}     Password must be 7-12 characters long
    ${PWD INVALID CONTENT}    Password must be a combination of lowercase and uppercase letters and numbers

Variables can also be given from the command line which is useful if
the tests need to be executed in different environments. For example
this demo can be executed like::

   robot --variable USERNAME:johndoe --variable PASSWORD:J0hnD0e QuickStart.rst

In addition to user defined variables, there are some built-in variables that
are always available. These variables include `${TEMPDIR}` and `${/}` which
are used in the above example.

Using variables
---------------

Variables can be used in most places in the test data. They are most commonly
used as arguments to keywords like the following test case demonstrates.
Return values from keywords can also be assigned to variables and used later.
For example, the following `Database Should Contain` `user keyword`_ sets
database content to `${database}` variable and then verifies the content
using BuiltIn_ keyword `Should Contain`. Both library and user keywords can
return values.

.. _User keyword: `User keywords`_
.. _BuiltIn: `Standard libraries`_

.. code:: robotframework

    *** Test Cases ***
    User status is stored in database
        [Tags]    variables    database
        Create Valid User    ${USERNAME}    ${PASSWORD}
        Database Should Contain    ${USERNAME}    ${PASSWORD}    Inactive
        Login    ${USERNAME}    ${PASSWORD}
        Database Should Contain    ${USERNAME}    ${PASSWORD}    Active

    *** Keywords ***
    Database Should Contain
        [Arguments]    ${username}    ${password}    ${status}
        ${database} =     Get File    ${DATABASE FILE}
        Should Contain    ${database}    ${username}\t${password}\t${status}\n

Organizing test cases
=====================

Test suites
-----------

Collections of test cases are called test suites in Robot Framework. Every
input file which contains test cases forms a test suite. When `executing this
guide`_, you see test suite `QuickStart` in the console output. This name is
derived from the file name and it is also visible in reports and logs.

It is possible to organize test cases hierarchically by placing test case
files into directories and these directories into other directories. All
these directories automatically create higher level test suites that get their
names from directory names. Since test suites are just files and directories,
they are trivially placed into any version control system.

Setups and teardowns
--------------------

If you want certain keywords to be executed before or after each test,
use the `Test Setup` and `Test Teardown` settings in the settings table.
Similarly you can use the `Suite Setup` and `Suite Teardown` settings to
specify keywords to be executed before and/or after an entire test suite.

Individual tests can also have a custom setup or teardown by using `[Setup]`
and `[Teardown]` in the test case table. This works the same way as
`[Template]` was used earlier with `data-driven tests`_.

In this demo we want to make sure the database is cleared before execution
starts and that every test also clears it afterwards:

.. code:: robotframework

    *** Settings ***
    Suite Setup       Clear Login Database
    Test Teardown     Clear Login Database

Using tags
----------

Robot Framework allows setting tags for test cases to give them free metadata.
Tags can be set for all test cases in a file with `Force Tags` and `Default
Tags` settings like in the table below. It is also possible to define tags
for a single test case using `[Tags]` settings like in earlier__ `User
status is stored in database` test.

__ `Using variables`_

.. code:: robotframework

    *** Settings ***
    Force Tags        quickstart
    Default Tags      example    smoke

When you look at a report after test execution, you can see that tests have
specified tags associated with them and there are also statistics generated
based on tags. Tags can also be used for many other purposes, one of the most
important being the possibility to select what tests to execute. You can try,
for example, the following commands::

    robot --include smoke QuickStart.rst
    robot --exclude database QuickStart.rst

Creating test libraries
=======================

Robot Framework offers a simple API for creating test libraries using either
Python or Java, and the remote library interface also allows using other
programming languages. `Robot Framework User Guide`_ contains a detailed
description about the library API.

As an example, we can take a look at the `LoginLibrary` test library used in
this demo. The library is located at `<lib/LoginLibrary.py>`_, and its source
code is also copied below. Looking at the code you can see, for example, how
the keyword `Create User` is mapped to actual implementation of the method
`create_user`.

.. code:: python

    import os.path
    import subprocess
    import sys


    class LoginLibrary(object):

        def __init__(self):
            self._sut_path = os.path.join(os.path.dirname(__file__),
                                          '..', 'sut', 'login.py')
            self._status = ''

        def create_user(self, username, password):
            self._run_command('create', username, password)

        def change_password(self, username, old_pwd, new_pwd):
            self._run_command('change-password', username, old_pwd, new_pwd)

        def attempt_to_login_with_credentials(self, username, password):
            self._run_command('login', username, password)

        def status_should_be(self, expected_status):
            if expected_status != self._status:
                raise AssertionError("Expected status to be '%s' but was '%s'."
                                     % (expected_status, self._status))

        def _run_command(self, command, *args):
            command = [sys.executable, self._sut_path, command] + list(args)
            process = subprocess.Popen(command, stdout=subprocess.PIPE,
                                       stderr=subprocess.STDOUT)
            self._status = process.communicate()[0].strip()
