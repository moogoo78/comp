Sphinx - reST (reStructuredText)
====================================

`reStructuredText <http://docutils.sourceforge.net/rst.html>`__ 是Python `Docutils: Documentation Utilities <http://docutils.sourceforge.net/index.html>`__ 的一部分, 而 `Sphinx <http://sphinx.pocoo.org/>`__ 可以更方便產生，管理文件的工具


縮排用二個或三個空白都可以，Emacs的ReST預設用三個

使用
----------

安裝::

   $ pip install Sphinx

某目錄下::

   $ sphinx-quickstart

會自動設定好conf.py, 和相關檔案

產生網頁檔::

   $ make html


語法   
---------

文件結構
~~~~~~~~~

**分段**

用一串連續的=*-^"~等字符來標示，通常用::

  ==========
  全文標題
  ==========

  大標題
  ========

  小標題
  ---------

  段落
  ^^^^^^


補充 heading convension:

- # with overline, for parts
- * with overline, for chapters
- =, for sections
- -, for subsections
- ^, for subsubsections
- “, for paragraphs

.. note:: 中文字用的分段標示要長一點點

**分隔**

像HTML裡的 ``<hr>``::

  ----



**修飾字 Inline markup**

*斜體*::

  *italic*

.. note:: 中文不能斜

**粗體**::

  **bold**

``有底色``::

  ``有底色`` 

"修飾字"的前後都要空格，如要跟後面的字相連可以用 ``\``\跳脫，如: **test**\123::

  **test**\123

**定義**

定義::

  what
    Definition of “what”. We add a few words to show the line wrapping.
  how
    Definition of “how”.


what
  Definition of “what”. We add a few words to show the line wrapping.
how
  Definition of “how”.


**條列 Lists**

標示字符用*, +, -,等都可以::

  * 第一項

    * 子項目不是只有縮排, 上面要空一行才可以

  * 第二項

  1. uno
  2. dos

  a) eins
  b) zwai

**文字區塊 Blocks**

以下是區塊, 要空一行和縮排::

   跟你說::

     嘻嘻

註解/警告::

  .. note:: this is a note.
  .. warning:: this is a warning.

.. note:: this is a note.
.. warning:: this is a warning.


**Syntax Highlighting**


Include Source, 會把原始碼印出來。

Python::

  .. iteralinclude:: example.py
    :linenos:


PHP::
 
   .. literalinclude:: example.php
     :language: php
     :prepend: <?php
     :append: ?>


**連結**

外部連結::

   `連結的標題 <http://example.com>`__
   `<http://example.com>`


內部連結, 在某個rst文件裡的任何一個地方定義 (底線開頭)::

   .. _my-reference-label:

連結::

   詳細內容請看 :ref:`標題文字<my-reference-label>` 或 :ref:`my-reference-label` (不用底線)

或是用::

   :doc:`reST`

會自動把文件標題抓出來: 

`Inline markup — Sphinx 1.3.1+ documentation <http://sphinx-doc.org/markup/inline.html#cross-referencing-arbitrary-locations>`__

**表格 (Tables)**


**(Field list)**

**Explicit Markup**

**extensions**

**graphviz**

conf.py的extension加入::

  extensions = ['sphinx.ext.graphviz']

語法::

  .. graphviz::

     digraph foo {
        "bar" -> "baz";
     }



**Comments 註解**

單行::

  .. This is a comment.

區塊::

  ..
     This whole indented block
     is a comment.

     Still in the comment.


**autodoc/automodule**

project 目錄下有 docs, mymod

自動產生 rst::
  
  $ sphinx-apidoc -F -o docs mymod

如果是 HTTP API 可以用 `sphinxcontrib.httpdomain <https://pythonhosted.org/sphinxcontrib-httpdomain/>`__ , 還有 ``sphinxcontrib.autohttp.flask`` 自動產生 flask 的 API Doc
  
  
修改 conf.py(project/docs/source)::

  sys.path.insert(0, os.path.abspath('../../mymod'))

然後在 docs 下 make html 就好了


.. note:: -o: output files directory -F: using sphinx-quickstart  create full project (conf.py Makefile...)
          
* [sphinx-apidoc manual page — Sphinx 1.2.3 documentation](http://sphinx-doc.org/man/sphinx-apidoc.html)


參考資料
------------
* `reST – reStructuredText — Bits and Pieces <http://people.ee.ethz.ch/~creller/web/tricks/reST.html>`__ - 簡潔扼要, 還有附Emacs的ReST mode
* `Documenting Your Project Using Sphinx — an_example_pypi_project v0.0.5 documentation <http://packages.python.org/an_example_pypi_project/sphinx.html#restructured-text-rest-resources>`__ - 
* `3.6.1. Restructured Text (reST) and Sphinx CheatSheet — openalea.doc v0.8.0 documentation <http://openalea.gforge.inria.fr/doc/openalea/doc/_build/html/source/sphinx/rest_syntax.html>`__
* `Documenting Your Project Using Sphinx — an_example_pypi_project v0.0.5 documentation <http://packages.python.org/an_example_pypi_project/sphinx.html>`__


Themes
----------

**rtd_theme**

conf.py::

  import sphinx_rtd_theme
  html_theme = 'sphinx_rtd_theme'
  html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

Publish
-----------

* `Publishing sphinx-generated docs on github — sphinxdoc-test v0.1 documentation <http://daler.github.io/sphinxdoc-test/includeme.html>`__ github pages
