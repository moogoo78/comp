< Python >
#####################
:date: 2013-07-10 14:02
:category: computer
:tags: docs



Quick Start
============

path::

  (Mac)$ /Library/Python/2.7/site-packages


snippets:

.. code-block:: python

  #!/usr/bin/env python
  # -.- coding: utf-8 -.-

  import sys
  if __name__ == '__main__':
      if len(sys.argv) > 1:
          main(sys.argv[1])


struct dict:

.. code-block:: python

  dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}

if/else三元運算 (ternary operator)::

  A = Y if X else Z


regular expression
------------------

只留a-z, A-Z, 0-9:

.. code-block:: python

  re.sub(r'[^a-zA-Z_0-9]', '', did)

parse出每個sql欄位
.. code-block:: python

  # (1, 'The Three Little Pigs', '三隻小豬', 350, '這是有關於三隻...', 28, 16000, 'en', 280, ''),
  m = re.match(r'\((\d+), \'(.*)\', \'(.*)\', (\d+), \'(.*)\', (\d+), (\d+), \'(.*)\', (\d+), \'(.*)\'\)', i)

list compension
-----------------------
印出a到z

.. code-block:: python

  a2z = [chr(a) for a in range(97, 123)]

  # 也可以加上tenary operator [ Y if X else Z for i in LIST]

sort list(用mydict的foo去排序)::

  output = sorted(mydict, key = lambda x:x['foo'])


lambda function::

  lower = (lambda x, y: x if x < y else y)
  lower ('bb', 'aa') # aa

string
--------------
format::

  '{0:.2f}'.format(init_scale) # 小數點後兩位
  '{0:02d}'.format(dtime.tm_mon) # 補2位0

replace ::

  string.replace('old', 'new')


日期/時間 time
--------------------

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

* `Measure Time in Python - time.time() vs time.clock() - Python Central <http://www.pythoncentral.io/measure-time-in-python-time-time-vs-time-clock/>`__


file
--------

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




Basic
====================

overview
-------------
functions are objects in Python, just like everything else. (If you find that confusing wait till you hear that classes are objects in Python, just like everything else!)


pprint::

  import pprint
  pp = pprint.PrettyPrinter(indent=4)
  pp.pprint(foo)


引數
---------
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


build-in functions
--------------------
filter(function, iterable)::

  [item for item in iterable if function(item)]

map(function, iterable, ...)::

  #

sum(iterable[, start])::

  #

all(iterable)::

  def all(iterable):
      for element in iterable:
          if not element:
              return False
      return True

any(iterable)::

  def any(iterable):
      for element in iterable:
          if element:
              return True
      return False


regex
---------
re.search()跟re.match()的不同, match()是字串開頭也要符合, search()只要字串中間有符合的pattern就可以了

`7.2. re — Regular expression operations — Python v2.7.6 documentation <http://docs.python.org/2/library/re.html#search-vs-match>`__

json
------

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
===================

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


IO / shell / commond line
================================
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

coding
===============

UnicodeEncodeError::

  import sys
  reload(sys)
  sys.setdefaultencoding('utf-8')

* `宅之力: 解決方法: UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128) <http://blog.wahahajk.com/2009/08/unicodedecodeerror-ascii-codec-cant.html>`__

shell
=========

多種方法:

* os.system()
* os.popen()
* subprocess.Popen()
* subprocess.call()

參考:

* `shell - Calling an external command in Python - Stack Overflow <http://stackoverflow.com/questions/89228/calling-an-external-command-in-python>`__

subprocess::

  subprocess.call('ls -al', shell=True)


Tips
=======

syntax
-----------------
變數決定class名稱::

  all_class = { 'my_class' : my_class }
  object = all_class['my_class']()



coding
------------------
只留ASCII::

  print "".join(filter(lambda x: ord(x)<128, did))


array排序
------------------
有個dict有title和date二個key, 要指定用date來排序::

  list = []
  list.append({'title':'abc','date':1})
  list.append({'title':'def','date':2})
  list.append({'title':'ghi','date':0})
  print sorted(list, key=lambda x: x['date'])
  # [{'title': 'ghi', 'date': 0}, {'title': 'abc', 'date': 1}, {'title': 'def', 'date': 2}]
  print sorted(list, key=lambda x: x['date'], reverse=True)
  # [{'title': 'def', 'date': 2}, {'title': 'abc', 'date': 1}, {'title': 'ghi', 'date': 0}]



list 找出最常出現
--------------------

利用 build-in function 的 max, set, count (另外 collections 也有 most_commons 的函式可用)::

.. code-block:: python
  
    max(set(cards), key=cards.count)


simple http server
---------------------
在當下目錄::

  $ python -m SimpleHTTPServer # 預設的port 8000, http://127.0.0.1:8000


Coding Style
===============
* `The Pocoo Style Guide — Pocoo <http://www.pocoo.org/internal/styleguide/>`__
* `Google Python Style Guide <http://google-styleguide.googlecode.com/svn/trunk/pyguide.html>`__
* `Code Style — The Hitchhiker's Guide to Python <http://docs.python-guide.org/en/latest/writing/style/>`__

Comments (google style)
----------------------------

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

Practice
====================
exceptions and/or logging

.. code-block:: python

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





整理
===========

小括弧整理程式碼::

  X = (A + B +
       C + D)

  if (A == 1 and
      B == 2 and 
      C == 3):
         print 'spam' * 3

.. note:: 斜線結尾不好看, 很難注意

reference
==============

Tutorial
----------
`Mosky Liu, Pinkoi | SlideShare <http://www.slideshare.net/moskytw>`__



decorator
==============

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


Package
================

easy_install
-------------------

upgrade pip::

  easy_install --upgrade pip

pip
-----------

pip::

  pip --version

~/.pip/pip.conf::

  [global]
  index-url = http://d.pypi.python.org/simple

  [install]
  use-mirrors = true
  mirrors =
      http://d.pypi.python.org
      http://b.pypi.python.org


連不到d.pypi.python.org...時::

  pip install -i http://pypi.python.org/simple PACKAGE


`PyPI Mirror Status <http://www.pypi-mirrors.org/>`__


推薦
-------

* `Arrow: better dates and times for Python — Arrow 0.4.4 documentation <http://crsmithdev.com/arrow/>`__



Image, PIL, Pillow
========================

在 Mac (OSX 10.9) 上用 pip (python 2.7) 裝 Pillow / PIL 失敗::

  # 用 homebrew 安裝
  $ brew install Homebrew/python/pillow
  # error: 顯示要link jpeg
  $ brew link jpeg --overwrite jpeg # 可能之前有舊的東西
