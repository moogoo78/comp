***********
Python
***********


snippets
====================

main
---------

.. code-block:: python

  #!/usr/bin/env python
  # -.- coding: utf-8 -.-

  import sys

  def main(arg1):
      print arg1

  if __name__ == '__main__':
      if len(sys.argv) > 1:
          main(sys.argv[1])

pprint
--------------

.. code-block:: python

  import pprint
  pp = pprint.PrettyPrinter(indent=4)
  pp.pprint(foo)



build-in
---------------

.. code-block:: python

  # walk
  for root, dirs, files in os.walk(src):
      print root, dirs, files

  os.listdir()

  ## list comprehensive
  # filter(function, iterable)
  [item for item in iterable if function(item)]

  # map(function, iterable, ...)
  # sum(iterable[, start])

  # all(iterable)

  def all(iterable):
      for element in iterable:
          if not element:
              return False
      return True

  # any(iterable)
  def any(iterable):
      for element in iterable:
          if element:
              return True
      return False



Tutorials
=================

functions are objects in Python, just like everything else. (If you find that confusing wait till you hear that classes are objects in Python, just like everything else!)

path::

  (Mac)$ /Library/Python/2.7/site-packages



引數
------------------
引數傳遞:

1. 傳值, 引數不回被改
2. 傳址標, 引數會被改 (list)

例如::

  def changer(a, b):
      a = 2
      b[0] = 'spam'

  X = 1
  L = [1, 2]
  changer(X, L)
  # >>> (1, ['spam', 2])

任意多引數::

  def func(*name): # tuple
      pass
  def func(**name): # dict
      pass


"is" vs "=="
----------------------

::

  >>> a = [1, 2, 3]
  >>> b = a

  >>> a is b
  True
  >>> a == b
  True

  >>> c = list(a)

  >>> a == c
  True
  >>> a is c
  False


* "is" expressions evaluate to True if two variables point to the same object
* "==" evaluates to True if the objects referred to by the variables are equal


Coding Style
-------------------------
* `The Pocoo Style Guide — Pocoo <http://www.pocoo.org/internal/styleguide/>`__
* `Google Python Style Guide <http://google-styleguide.googlecode.com/svn/trunk/pyguide.html>`__
* `Code Style — The Hitchhiker's Guide to Python <http://docs.python-guide.org/en/latest/writing/style/>`__

Comments (google style):

.. code-block:: python

    def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
        """Fetches rows from a Bigtable.

        Retrieves rows pertaining to the given keys from the Table instance
        represented by big_table.  Silly things may happen if
        other_silly_variable is not None.

        Args:
            big_table: An open Bigtable Table instance.
            keys: A sequence of strings representing the key of each table row
                to fetch.
            other_silly_variable: Another optional variable, that has a much
                longer name than the other args, and which does nothing.

        Returns:
            A dict mapping keys to the corresponding table row data
            fetched. Each row is represented as a tuple of strings. For
            example:

            {'Serak': ('Rigel VII', 'Preparer'),
             'Zim': ('Irk', 'Invader'),
             'Lrrr': ('Omicron Persei 8', 'Emperor')}

            If a key from the keys argument is missing from the dictionary,
            then that row was not found in the table.

        Raises:
            IOError: An error occurred accessing the bigtable.Table object.
        """
        pass



小括弧整理程式碼
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

  X = (A + B +
       C + D)

  if (A == 1 and
      B == 2 and
      C == 3):
         print 'spam' * 3

.. note:: 斜線結尾不好看, 很難注意


How-to
===============

變數決定class名稱
------------------------

.. code-block:: python

  all_class = { 'my_class' : my_class }
  object = all_class['my_class']()


只留ASCII
-----------------

.. code-block:: python

  print "".join(filter(lambda x: ord(x)<128, did))


有個dict有title和date二個key, 要指定用date來排序
------------------------------------------------------

.. code-block:: python

  list = []
  list.append({'title':'abc','date':1})
  list.append({'title':'def','date':2})
  list.append({'title':'ghi','date':0})
  print sorted(list, key=lambda x: x['date'])
  # [{'title': 'ghi', 'date': 0}, {'title': 'abc', 'date': 1}, {'title': 'def', 'date': 2}]
  print sorted(list, key=lambda x: x['date'], reverse=True)
  # [{'title': 'def', 'date': 2}, {'title': 'abc', 'date': 1}, {'title': 'ghi', 'date': 0}]


list 找出最常出現
---------------------------
利用 build-in function 的 max, set, count (另外 collections 也有 most_commons 的函式可用)

.. code-block:: python

    max(set(cards), key=cards.count)


Function Arguments
---------------------------


argument unpacking

.. code-block:: python

    def myfunc(x, y, z):
        print(x, y, z)

    tuple_vec = (1, 0, 1)
    dict_vec = {'x': 1, 'y': 0, 'z': 1}

    # >>> myfunc(*tuple_vec)
    # 1, 0, 1

    # >>> myfunc(**dict_vec)
    # 1, 0, 1


regular expression
--------------------------

re.search()跟re.match()的不同, match()是字串開頭也要符合, search()只要字串中間有符合的pattern就可以了

只留a-z, A-Z, 0-9:

.. code-block:: python

  re.sub(r'[^a-zA-Z_0-9]', '', did)

parse出每個sql欄位
-----------------------

.. code-block:: python

  # (1, 'The Three Little Pigs', '三隻小豬', 350, '這是有關於三隻...', 28, 16000, 'en', 280, ''),
  m = re.match(r'\((\d+), \'(.*)\', \'(.*)\', (\d+), \'(.*)\', (\d+), (\d+), \'(.*)\', (\d+), \'(.*)\'\)', i)

* `7.2. re — Regular expression operations — Python v2.7.6 documentation <http://docs.python.org/2/library/re.html#search-vs-match>`__
* `Regular Expressions Are Nothing to Fear – Doug Hellmann <https://doughellmann.com/blog/2017/02/13/regular-expressions-are-nothing-to-fear/>`__ 好範例

印出a到z
-----------------

.. code-block:: python

  a2z = [chr(a) for a in range(97, 123)]

read csv
----------------

.. code-block:: python

  with open('foo.csv', newline='\n') as csvfile:
    spamreader = csv.reader(csvfile, delimiter=',')
    for row in spamreader:
        print(', '.join(row))



sort list(用mydict的foo去排序)
----------------------------------

.. code-block:: python

  output = sorted(mydict, key = lambda x:x['foo'])

  xs = {'a': 4, 'b': 3, 'c': 2, 'd': 1}
  sorted(xs.items(), key=lambda x: x[1])



lambda function
------------------------

.. code-block:: python

  lower = (lambda x, y: x if x < y else y)
  lower ('bb', 'aa') # aa


try/except
--------------------

exceptions and/or logging

.. code-block:: python

  import sys

  class SillyWalkMinistry(Exception):
      """ handle exception """
      pass

  try:
      do_silly(value)
  except AttributeError as e:
      log.info('')
      do_invisible(v)
  except Exception as e:
      log.debug(str(e))
      raise SillyWalkMinistry(e)


   # all exception
   sys.exc_info()[0]


decorator
---------------------

沒用 from functools import wraps 的話, function的資訊會跑掉, 重複(reentrant) 會有問題, 傳參數的話會變只有最後一個

via: http://stackoverflow.com/questions/308999/what-does-functools-wraps-do

.. code-block:: python

  # -.- encoding: utf-8 -.-

  from functools import wraps
  def logged(func):
      @wraps(func)
      def with_logging(*args, **kwargs):
          print func.__name__ + " was called"
          return func(*args, **kwargs)
      return with_logging

  @logged
  def f(x):
     """does some math"""
     return x + x * x

  print f.__name__  # prints 'f', 沒wraps -> with_logging
  print f.__doc__   # prints 'does some math' 沒wraps -> None

  print '-----'

  def logged_param(param):
      def with_logging(func):
          #@wraps(func)
          def log_p(*args, **kwargs):
              print func.__name__ + " was called, ", param
              return func(*args, **kwargs)
          return log_p
      return with_logging

  @logged_param('foo')
  def f2(x):
     """does some math2"""
     return x + x * x

  print f2.__name__  # prints 'f'
  print f2.__doc__   # prints 'does some math'
  print f2(2)

  @logged_param('bar')
  def f3(x):
      """ math3 """
      return x + x * x

  print f3(2)

  print f2(2)


另一例:

.. code-block:: python

    from time import time

    # Imperative Programming
    def speak(topic):
        print "My speach is " + topic

    def timer(fn):
        def inner(*args, **kwargs):
            t = time()
            fn(*args, **kwargs)
            print "took {time}".format(time=time()-t)

        return inner

    speaker = timer(speak)
    speaker("FP with Python")

    # Decorator (Functional Programming)
    @timer
    def speak(topic):
        print "My speach is " + topic

    speak("FP with Python")

    # > My speach is FP with Python
    # > took 5.96046447754e-06



Profiling
---------------
`My Python Code is Slow? Tips for Profiling – Marco Bonzanini <http://marcobonzanini.com/2015/01/05/my-python-code-is-slow-tips-for-profiling/>`__

* unix shell: time ``time python -c "import profile_test;``
* python basic module: time.time(), timeit
* cProfile ``pstats``, line_profiler ``kernprof -v -l profile_test.py``


Package
---------------

`PyPI Mirror Status <http://www.pypi-mirrors.org/>`__


~/.pip/pip.conf

.. code-block:: text

  [global]
  index-url = http://d.pypi.python.org/simple

  [install]
  use-mirrors = true
  mirrors =
      http://d.pypi.python.org
      http://b.pypi.python.org



Reference
====================
* `Victor Stinner’s Notes — Victor Stinner's Notes 1.0 documentation <https://vstinner.readthedocs.io/>`__ (TP 推薦)
* `Arrow: better dates and times for Python — Arrow 0.4.4 documentation <http://crsmithdev.com/arrow/>`__
* `Mosky Liu, Pinkoi | SlideShare <http://www.slideshare.net/moskytw>`__ good tutorial
* `Intermediate Python — Python Tips 0.1 documentation <http://book.pythontips.com/en/latest/index.html>`__ 好用進階, tips
* `How a Python function can find the name of its caller « Python recipes « ActiveState Code <http://code.activestate.com/recipes/579105-how-a-python-function-can-find-the-name-of-its-cal/>`__ 得到 caller 的名字


string
-------------

format::

  '{0:.2f}'.format(init_scale) # 小數點後兩位
  '{0:02d}'.format(dtime.tm_mon) # 補2位0
  '{:.1%}'.format(1/3.0) # 百分比 '33.3%'

`Python String Formatting Best Practices – Real Python <https://realpython.com/python-string-formatting/>`__

replace::

  string.replace('old', 'new')


coding::

  u = unichr(40960) + u'abcd' + unichr(1972)   # type(u), unicode
  utf8_version = u.encode('utf-8')             # Encode as UTF-8
  type(utf8_version), utf8_version
  # (<type 'str'>, '\xea\x80\x80abcd\xde\xb4')
  u2 = utf8_version.decode('utf-8')            # Decode using UTF-8
  u == u2                                      # The two strings match
  True

  ### format
  'foo %s bar' % (foostr.decode('utf-8'))


`Unicode HOWTO — Python 2.7.12 documentation <https://docs.python.org/2/howto/unicode.html>`__


日期/時間 time
-----------------------------

**time format**

.. code-block:: python

  import time
  from datetime import datetime
  TIME_STR = time.strftime('%Y%m%d-%H%M%S') # 時間字串

  dtime = datetime.utcnow() # UTC 格式
  ts = time.mktime(dtime.timetuple()) # datetime to unix timestamp
  ts = time.time() # unix timestamp now
  datetime.fromtimestamp(ts) # unix timestamp to datetime

  dtime.strftime('%Y-%m-%d') # datetime => string
  datetime.strptime(dtime, '%Y-%m-%d') # => string to datetime (格式要一樣)

  datetime.strptime(dtime.strftime('%Y-%m-%d'), '%Y-%m-%d') # 取得年月日0分0時0秒

  iso_time = time.strftime("%Y-%m-%dT%H:%M:%S", tuple_time)

  # 常用
  ## 取得當周的第1天跟最後一天
  d = datetime(2014, 1, 15)
  w = d.weekday()
  t1 = d + timedelta(days=-w)
  t2 = d + timedelta(days=+(7-w))


  ## time
  time.gmtime(n) # 輸入 unix epoch time 秒數, 回傳 time struct
  time.time() # unix epoch time (unix-base 和 windows 系統一樣) in seconds
  time.clock() # unix-base: returns the current processor time (current thread, in seconds). Windows: returns the wall-clock time expressed in seconds elapsed since the first call to this function, based on the Win32 function QueryPerformanceCounter

  # timeit


* `WorkingWithTime - Python Wiki <https://wiki.python.org/moin/WorkingWithTime>`__
* `Measure Time in Python - time.time() vs time.clock() - Python Central <http://www.pythoncentral.io/measure-time-in-python-time-time-vs-time-clock/>`__
* `Date and Time Representation in Python <http://www.seehuhn.de/pages/pdate>`__ good!


file
------------------


write::

  f = open('data.txt', 'w')
  f.write('Hello\n')
  f.close()

read::

  f = open('data.txt') # 預設是 'r'
  bytes = f.read() # 讀出內容
  # -------------
  for line in f:
      print line # 讀出每一行
  f.close()

status::

  os.path.getmtime(fpath) # last modified timestamp, (在 unix 可當做 create time, 另外: ``ctime``... )


tempfile::

  import tempfile

  temp = tempfile.NamedTemporaryFile # 可以用檔名取得
  print temp.name
  temp = tempfile.TemporaryFile # 只有 memory addr
  temp.close() # 會自動砍掉


json
---------------------

.. code-block:: python

  # obj to json string (serialize)
  json.dumps({'foo':'bar'}, ensure_ascii=False) # ensure_ascii = False (Default: True), 中文不會變成 u\xxxx 的 unicode 格式

  # obj to json fp
  json.dump({'foo':'bar'}, fp)

  # json fp to obj
  json.load(fp)

  # json string
  json.loads(s)


算數
-------------------

.. code-block:: python

  import random

  random.randint(0,9)
  # ''.join([str(random.randint(1,9)) for i in range(5)]) # 產生5個0-9的字串

  random.random() # 產生 [0.0, 1.0) 的亂數

  # test 百分比
  a = 0
  b = 0
  c = 0
  for i in range(10000):
      r = random.random()
      if r >= 0.95: # 5 %
          c += 1
      elif r >= 0.70 and r < 0.95: # 25 %
          b += 1
      else: # 70 %
          a+= 1
  print a, b,c, a/10000.0, b/10000.0, c/10000.0


.. code-block:: python

   # 交集
   list(set(list_foo) & set(list_bar))
   # 聯集
   list(set(list_foo) | set(list_bar))

   # 過濾重覆
   list(set(list_foo))


IO / shell / commond line
-----------------------------

`15.1. os — Miscellaneous operating system interfaces — Python v2.7.3 documentation <http://docs.python.org/2/library/os.html>`__

檢查目錄存在::

  os.path.exists('/etc/passwd')

subprocess::

  import subprocess
  subprocess.call(["ls", "-l"]) # 輸入是list, pipe要用popen, 安全一點
  subprocess.call(["ls -l"], shell=True) # 完全用系統的shell, pipe, wildcards, 家目錄~都可以用, 參數直接給字串就可以了, 也許會有輸入不乾淨(shell injection)的風險


常用::

  os.getcwd()
  os.mkdir(src)
  os.rename(src, dst)


shell 多種方法:

* os.system()
* os.popen()
* subprocess.Popen()
* subprocess.call()

參考:

* `shell - Calling an external command in Python - Stack Overflow <http://stackoverflow.com/questions/89228/calling-an-external-command-in-python>`__

subprocess::

  subprocess.call('ls -al', shell=True)


simple http server
--------------------------

.. code-block:: bash

  # python 2
  $ python -m SimpleHTTPServer # 預設的port 8000, http://127.0.0.1:8000
  # python 3
  $ python -m http.server


Modules / Libraries
=====================

MySQLdb
---------------

.. code-block:: python

    import MySQLdb

    db = MySQLdb.connect(host='localhost', user='root', passwd='123456', db='db_name', charset='utf8')
    # charset 沒設定預設是 latin-1

    cur = db.cursor()

    cur.execute("SELECT * FROM book")

    for row in cur.fetchall():
        print row[1]

    cur.fetchone()

    # 如果 INSERT 或 UPDATE就要
    db.commit()


常見 error

.. code-block:: bash

    # _mysql.so Library not loaded: libmysqlclient.16.dylib
    sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/local/lib/libmysqlclient.18.


Image, PIL, Pillow
----------------------------------
在 Mac (OSX 10.9) 上用 pip (python 2.7) 裝 Pillow / PIL 失敗

.. code-block:: bash

  # 用 homebrew 安裝
  $ brew install Homebrew/python/pillow
  # error: 顯示要link jpeg
  $ brew link jpeg --overwrite jpeg # 可能之前有舊的東西


Excel
----------------------

比較新, 功能強

檔名要 .xlsx, 不然認定格式不合

openpyxl::

  from openpyxl import load_workbook
  wb = load_workbook(filename='large_file.xlsx', read_only=True)
  sheet_name = wb.get_sheet_names()[0] # 預設第一個 sheet
  ws = wb[sheet_name]

  for row in ws.rows:
      for cell in row:
          print(cell.value)


xlrd::

   book = xlrd.open_workbook('foo.xlsx')
   sheet = book.sheet_by_name(u'工作表1')

   for i in range(1,sheet.nrows):
       title = sheet.row_values(i)[2]
       descr = sheet.row_values(i)[5]


* `The xlrd Module <https://secure.simplistix.co.uk/svn/xlrd/trunk/xlrd/doc/xlrd.html?p=4966>`__



Problems
===============

coding
----------------------

UnicodeEncodeError::

  import sys
  reload(sys)
  sys.setdefaultencoding('utf-8')

* `宅之力: 解決方法: UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128) <http://blog.wahahajk.com/2009/08/unicodedecodeerror-ascii-codec-cant.html>`__


連不到d.pypi.python.org...時
--------------------------------

.. code-block::

  pip install -i http://pypi.python.org/simple PACKAGE


Development
======================

via:: `(85) 這樣的開發環境沒問題嗎？ – Tzu-ping Chung – PyCon Taiwan 2018 - YouTube <https://www.youtube.com/watch?v=6Nl0IYkU0hU>`__

install problems
-----------------------------
- api 會裝在 /usr/lib
- sudo pip install 會裝在 /usr/local/lib
- pip install --user 會裝在 ~/.local/lib


Rules
---------------
- Do not use sudo. Ever.
- Don't use Hemebrew Python (每次更新都會改路徑)
- Prefer self-compiling if possible
- Don't add things to PATH; **link** them (in PATH)
- Always use virtual environments
- You can call scripts in venv directly
- Use **--user** when desperate


virtual environment/management
-----------------------------------

.. code-block:: bash

   $ mkdir -p ~/.local/bin ~/.local/venvs
   # pipenv or poetry
   $ python3 -m venv ~/.local/venvs/{pipenv|poetry}
   $ ~/.local/venvs/{pipenv|poetry}/bin/pip install {pipenv|poetry}
   $ ln -s ~/.local/venvs/{pipenv|poetry}/bin/{pipenv|poetry} ~/.local/bin

   $ {pipenv|poetry} --version



install python
------------------------

不推 shim (.python-version)，因為: if 使用系統 python，外部套件可能會有 build deps error


use python-build (pyenv plugin) or pythonup (mac/windows)


.. code-block:: bash

   $ python-build 3.9.6 ~/.local/pythons/3.9
   $ python-build 3.8.11 ~/.local/pythons/3.8
   $ ln -s ~/.local/pythons/3.9/bin/python3.9 ~/.local/bin
   $ ln -s ~/.local/pythons/3.8/bin/python3.8 ~/.local/bin

   $ ln -s ~/.local/bin/python3.8 ~/.local/bin/python3


   # hide default python
   python() {
       local PYTHON="$(which python)"
       if [[ "$PYTHON" == /usr/* ]]; then
           echo "nope" >&2 | echo >/dev/null
       else
           "PYTHON" "$@"
       if
   }



pyenv
----------------

.. code-block:: bash

   $ brew update
   $ brew install pyenv

   $ pyenv init
   # 複製貼上輸出到 ~/.zshrc

   $ pyenv versions # 目前可用的版本
   $ pyenv install -l # 看可安裝的版本
   $ pyenv install 2.7.15

   # pyenv rehash # 重 load shell
   $ pyenv local 2.7.15


