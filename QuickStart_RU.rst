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
можно использовать и как `исполняемый пример`__. Все описанные здесь возможности
фреймворка более подробно разобраны в `Robot Framework User Guide`_.

__ `Запуск руководства как набора тестов`_
.. _Robot Framework User Guide: http://robotframework.org/robotframework/#user-guide

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

Robot Framework не привязан к операционным системам или приложениям. Его ядро реализовано
на `Python <http://python.org>`_ и может быть запущено также на
`Jython <http://jython.org>`_ (JVM) и `IronPython <http://ironpython.net>`_
(.NET). Фреймворк имеет развитую экосистему из различных библиотек и сопутствующих
инструментов, разработанных в отдельных от него проектах.

За дальнейшей информацией о Robot Framework и его экосистеме обращайтесь к
http://robotframework.org. Там вы найдете еще больше документации, демонстрационных
проектов, списки тестовых библиотек, сопутствующие инструменты и тому подобное.

Демонстрационное приложение
---------------------------

Образец приложения в этом руководстве — один из вариантов классического входа
в систему: это сервер аутентификации с интерфейсом командной строки, написанный на
Python. Это приложение позволяет пользователю делать три вещи:

- Завести учетную запись с подходящим паролем.
- Осуществить вход с верным паролем и именем пользователя.
- Сменить пароль у существующей учетной записи пользователя.

Само приложение находится в файле `<sut/login.py>`_ и может быть запущено командой
`python sut/login.py`. Попытка входа с помощью несуществующей учетной записи или
неверного пароля приведет к следующему сообщению об ошибке::

    > python sut/login.py login nobody P4ssw0rd
    Access Denied

А после создания учетной записи вход с верным поролем будет успешным::

    > python sut/login.py create fred P4ssw0rd
    SUCCESS

    > python sut/login.py login fred P4ssw0rd
    Logged In

Имеются следующие ограничения, которым отвечает валидный пароль: он должен быть
длинной от 7 до 12 знаков и содержать буквы в верхнем и нижнем регистре,
а также цифры, но не должен содержать специальных символов. Попытка создать
пользователя с невалидным паролем приведет к сообщению об ошибке::

    > python sut/login.py create fred short
    Creating user failed: Password must be 7-12 characters long

    > python sut/login.py create fred invalid
    Creating user failed: Password must be a combination of lowercase and
    uppercase letters and numbers


Изменение пароля с неверными учетными данными приведет к такому же сообщению
об ошибке, как и попытка войти с неверным паролем или именем пользователя.
Валидность обновленного пароля также проверяется, и, если он не соответствует
ограничениям, то будет выведено сообщение об ошибке::

    > python sut/login.py change-password fred wrong NewP4ss
    Changing password failed: Access Denied

    > python sut/login.py change-password fred P4ssw0rd short
    Changing password failed: Password must be 7-12 characters long

    > python sut/login.py change-password fred P4ssw0rd NewP4ss
    SUCCESS

Приложение использует простую базу данных в файле для сохранения статуса пользователя.
База помещается в каталог для временных файлов, местополежние его зависит от используемой
операционной системы.


Запуск руководства как набора тестов
====================================

Эта инструкция описывает как самостоятельно запустить это руководство (как набор тестов).
Если это вам не интересно, вы можете посмотреть готовый `результат выполнения`__ на сайте.

__ `Просмотр результатов`_

Установка
---------

Рекомендуемый способ установки Robot Framework на Python_ — это использование пакетного
менеджера `pip <http://pip-installer.org>`_. Если и Python, и pip у вас уже установлены,
вам достаточно выполнить команду::

    pip install robotframework

См. `Robot Framework installation instructions`_ для знакомства с альтернативными методами,
и в целом за дополнительной сведениями об установке.

Это демонстрационное руководство использует язык разметки reStructuredText__ с тестовыми
данными Robot Framework внутри блоков для кода. Выполнение тестов в этом формате потребует
установки дополнительного модуля docutils__::

    pip install docutils

Обратите внимание на то, что Robot Framework 3.0 это первая версия Robot Framework, которая
поддерживает Python 3. Обращайтесь к уже упомянутой `installation instructions`_ за
информацией о разнице между Python 2 и Python 3.

.. _`Robot Framework installation instructions`:
   https://github.com/robotframework/robotframework/blob/master/INSTALL.rst
.. _`installation instructions`: `Robot Framework installation instructions`_
__ http://docutils.sourceforge.net/rst.html
__ https://pypi.python.org/pypi/docutils

Выполнение
----------

После установки модулей вам потребуется само демо. Самый простой способ — это
загрузить нужный релиз__ или `архив с последней версией`__ и распаковать
где-либо, а также можно клонировать `репозиторий проекта`__.

После того как вы установите все нужные компоненты, вы сможете запустить
демонстрацию использую команду `robot`::

    robot QuickStart.rst
    
Если вы используете Robot Framework 2.9 или более старый, вместо команды `robot`
нужно будет использовать `pybot`::

    pybot QuickStart.rst

Вы также можете настроить исполнение используя различные опции командной строки::

    robot --log custom_log.html --name Custom_Name QuickStart.rst

Для получения списка доступных опций выполните команду `robot --help`.

__ https://github.com/robotframework/QuickStartGuide/releases
__ https://github.com/robotframework/QuickStartGuide/archive/master.zip
__ https://github.com/robotframework/QuickStartGuide

Просмотр результатов
--------------------

Запуск демонстрационных тестов приведет к созданию трех файлов с отчетами.
Здесь ссылки ведут на уже готовые отчеты на сайте, а запуск демо-тестов
приведет к созданию аналогичных файлов локально.

`report.html <http://robotframework.org/QuickStartGuide/report.html>`__
    Обобщенный отчет о выполненных тестах.
`log.html <http://robotframework.org/QuickStartGuide/log.html>`__
    Детализированный журнал выполнения тестов.
`output.xml <http://robotframework.org/QuickStartGuide/output.xml>`__
    Результаты тестов в машиночитаемом формате XML.

Тестовые сценарии
=================

Workflow тесты
--------------

Тестовые сценарии Robot Framework используют простой синтаксис на основе табуляции.
Например, этот сценарий содержит два теста:

- Пользователь может создать учетную запись и войти в систему
- Пользователь не может войти с неверным паролем

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

Обратите внимание, что эти тесты читаются как мануальные тесты записанные на
английском языке. Robot Framework использует подход на основе ключевых слов, который
поддерживает написание тестов, захватывающих саму суть действий и ожиданий, на
естественном языке.

Тестовые сценарии выстраиваются из ключевых слов и их аргументов. Синтаксис
требует, чтобы ключевые слова и аргументы, также как настройки и их значения,
отделялись двумя пробелами или символом табуляции. В общем случае, рекомендуется
использовать использовать четыре пробела, чтобы сделать разделитель более заметным,
а в некоторых случаях это позволяет выравнивать аргументы или другие значения, что
делает их более легкими для восприятия. За более детальным описанием синтаксиса
обращайтесь к  `Robot Framework User Guide`_.

Тесты верхнего уровня
---------------------

Тестовые сценарии могут быть созданы с использованием ключевых слов верхнего
уровня, которые не используют позиционных аргументов. Такой стиль не ограничивает
свободу в описании, что хорошо подходит для коммуникации даже с технически
неосведомленными пользователями или другими стейкхолдерами. Это может быть
особенно важно при использовании подхода `acceptance test-driven development`__
(ATDD) или его вариантов, когда тесты используются и как требования к создаваемому ПО.

Robot Framework не принуждает к использованию какого-либо определенного подхода
к написанию сценариев. Один из распространенных стилей это формат *given-when-then*,
ставший популярным благодаря `behavior-driven development`__ (BDD):

.. code:: robotframework

    *** Test Cases ***
    User can change password
        Given a user has a valid account
        When she changes her password
        Then she can log in with the new password
        And she cannot use the old password anymore

__ http://en.wikipedia.org/wiki/Acceptance_test-driven_development
__ http://en.wikipedia.org/wiki/Behavior_driven_development

Тестирование на основе данных
-----------------------------

Часто несколько тестовых сценариев бывают похожи друг на друга, за исключением разницы
во входящих или исходящих данных. В таких случаях *data-driven тесты* позволяют
варьировать тестовые данные без дублирования выполняемых действий. С помошью настройки
`[Template]` Robot Framework превращает тестовые сценарии в тесты управляемые данными.
Ключевое слов, используемое как шаблон, выполняется с использованием данных, определенных
в теле тестового сценария:

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

В дополнение к настройке `[Template]` для отдельных тестов, возможно также использовать
общую настройку `Test Template`, указав ее однажды указанная в таблице настроек такой как
`setups and teardowns`_ (рассматривается ниже). В нашем случае проще было создать
отдельные тесты для невалидной длины пароля и для сценариев с другими невалидными
данными. Тем не менее, это потребовало вынести эти тесты в отдельный файл, иначе этот
шаблон был бы применен и другим тестам в этом файле.

Обратите также внимание, что сообщения об ошибках в этом примере задаются с использованием
переменных_.

.. _`переменных`: `Переменные`_

Ключевые слова
==============

Тестовые сценарии созадются из ключевых слов, которые аогут имет два источника.
`Библиотечные ключевые слова`_ могут импортироватся из библиотек, а так называемые
`пользовательские ключевые слова`_ могут быть созданы с использованием того же
синтаксиса, что используется при написании тестовых сценариев.

Библиотечные ключевые слова
---------------------------

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

Пользовательские ключевые слова
-------------------------------

Одна из наиболее сильных возможностей Robot Framework это способность легко
создавать новые ключевые слова верхнего уровня из других ключевых слов. Синтаксис
для создания *определяемых пользователем ключевых слов* или для краткости
*пользовательских ключевых слов* похож на ни синтаксис используемый для
создания тестов. Все ключевые слова верхнего уровня, необходимые для предыдущего
теста, задаются в этой таблице ключевых слов:

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

    # Ключевый слова ниже используются для тестов верхнего уровня. Обратите ввнимание
    # что префиксы given/when/then/and могут быть отброшены. А еще это пример комментария.

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

Пользовательские ключевые слова могут включать действия, задаваемые другими
пользовательскими или библиотечными ключевыми словами. Как видно из этого
примера, пользовательские ключевые слова могут принимать параметры. Они также
могут возвращать значения и даже включать в себя циклы FOR. Но этом этапе
для нас важно, что пользовательские ключевые слова позволяют авторам тестов
создавать повторно используемые шаги для общих последовательностей действий.
Пользовательские ключевые слова также могут помочь сделать тесты максимально
удобными для понимания и использовать подходящий уровень абстракции в различных
ситуациях.


Переменные
==========

Определение переменных
----------------------

Перменные это неотьемлимая часть Robot Framework. Обычно, любые данные,
используемые в тестах, и подверженные изменениям, будет лучше определить через
переменные. Синтаксис определения пременных прост, как это видно в этой таблице
с переменными:

.. code:: robotframework

    *** Variables ***
    ${USERNAME}               janedoe
    ${PASSWORD}               J4n3D0e
    ${NEW PASSWORD}           e0D3n4J
    ${DATABASE FILE}          ${TEMPDIR}${/}robotframework-quickstart-db.txt
    ${PWD INVALID LENGTH}     Password must be 7-12 characters long
    ${PWD INVALID CONTENT}    Password must be a combination of lowercase and uppercase letters and numbers

Перменные также могут задваться из командной строки, что может быть полезно
при запуске теста в разном окружении. Например этот демо-пример может быть запущен
таким способом::

   robot --variable USERNAME:johndoe --variable PASSWORD:J0hnD0e QuickStart.rst

В дополнение к переменным, определяемым пользователем, всегда доступные и некоторые
встроенные пременные. Эти переменные включают `${TEMPDIR}` и `${/}` как это показано
в примере выше.

Использование переменных
------------------------

В большинстве случаев, тестовые данные могут использовать переменные. Обычно они
используются в качестве аргументов ключевых слов, как это показано в примере ниже.
Значения, отдаваемые ключевыми словами, также могут присваиваться переменным
и использоваться далее. Напрмер, `пользовательское ключевое слово`_ `Database Should
Contain` задает значение для переменной `${database}` и затем верифицирует содержимое
используя `встроенное ключевое слово`_ `Should Contain`. Оба, — и библиотечное,
и пользовательское — ключевых слова могут возвращать значения.

.. _пользовательское ключевое слово: `Пользовательские ключевые слова`_
.. _встроенное ключевое слово: `Standard libraries`_

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
input file which contains test cases forms a test suite. Когда мы `запускаем это руководство`__
, you see test suite `QuickStart` in the console output. This name is
derived from the file name and it is also visible in reports and logs.

It is possible to organize test cases hierarchically by placing test case
files into directories and these directories into other directories. All
these directories automatically create higher level test suites that get their
names from directory names. Since test suites are just files and directories,
they are trivially placed into any version control system.

__ `Запуск руководства как набора тестов`_

Setups and teardowns
--------------------

If you want certain keywords to be executed before or after each test,
use the `Test Setup` and `Test Teardown` settings in the settings table.
Similarly you can use the `Suite Setup` and `Suite Teardown` settings to
specify keywords to be executed before and/or after an entire test suite.

Individual tests can also have a custom setup or teardown by using `[Setup]`
and `[Teardown]` in the test case table. This works the same way as
`[Template]` was used earlier with `data-driven тестами`__.

In this demo we want to make sure the database is cleared before execution
starts and that every test also clears it afterwards:

__ `Тестирование на основе данных`_

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

__ `Использование переменных`_

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
