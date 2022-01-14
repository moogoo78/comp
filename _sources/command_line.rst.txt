Command 常用指令
=========================


macos 的 updatedb:

..code-block: bash

   sudo /usr/libexec/locate.updatedb

   set -e # Exit immediately if a command exits with a non-zero status.

package
----------------

APT::

  $ apt-get install foo
  $ apt-get remove foo --auto-remove
  $ apt-get remove foo [--purge]
  $ sudo apt-get autoremove --purge
  $ apt-cache policy foo # 看有什麼可以裝的版本

log::

  /var/log/apt/term.log
  /var/log/apt/history.log
  /var/log/dpkg.log

system disk
--------------

fdisk::
  
  $ fdisk -l
  $ fdisk /dev/sdx

  # 進入後用 m 看指令 (help)

FAT32::
    
  $ mkdosfs -F 32 -I /dev/sdb1
  $ dosfslabel /dev/sdb1 "NEWLABEL"


snippets
-------------


新增目錄, 順便移動過去

.. code-block:: bash

    mkdir /foo/bar && cd $_


複製檔案, 不用重複打兩次檔名

.. code-block:: bash

    cp /some/path/to/file.txt{,.bak}

`Bash Brace Expansion | Linux Journal <https://www.linuxjournal.com/content/bash-brace-expansion>`__


把"/t"分隔的檔案轉成sql語法, 用awk和sed

.. code-block:: bash

  awk -F '\t' '{print "INSERT INTO word (\"s3id\",\"content\",\"freq\",\"translate\",\"ex1\",\"ex1_translate\",\"ex2\",\"ex2_translate\") VALUES(\""$1"\",\""$2"\",\""$3"\",\""$4"\",\""$5"\",\""$6"\",\""$7"\",\""$8"\");"}' data_all_u.txt | sed -e 's/^M//g' > out.sql

.. note:: 先用awk轉好, 再用sed去除最後換行符號, "^M"在commend-line裡要Ctrl-V + Ctrl-M表示

忘記指令名稱, 但大概記得是幹嘛的

.. code-block:: bash

    apropos "some description"


剛剛那個指令要用 sudo

.. code-block:: bash

    sudo !!


more


* `Robert Muth: Better Bash Scripting in 15 Minutes <http://robertmuth.blogspot.tw/2012/08/better-bash-scripting-in-15-minutes.html?utm_source=hackernewsletter&utm_medium=email&utm_term=fav>`__
* `Sed - An Introduction and Tutorial <http://www.grymoire.com/Unix/sed.html?utm_source=hackernewsletter&utm_medium=email&utm_term=code>`__
* `5 Handy Bash Tricks in 2 Minutes - DEV Community 👩‍💻👨‍💻 <https://dev.to/jacobherrington/5-handy-bash-tricks-in-2-minutes-23ph?utm_source=digest_mailer&utm_medium=email&utm_campaign=digest_email>`__

顯示 \uxxx => unicode 文字:

.. code-block:: bash

   echo -e "$(curl http://127.0.0.1:5000/api/info)"



用 less +F 代替 tail -f (可以 Ctrl-c 跳出, F 再度回到追蹤模式)
   
檔案
---------------


備份每個檔案

.. code-block:: bash

    for file in * ; do cp "$file" "$file".bak; done



把檔案foo複製到以下全部目錄裡::

  $ find . -type d |xargs -n 1 cp -i foo

:xargs -n 1: 把每一行command line輸出當作一個參數
:cp -i: 詢問y or n

        
把目錄下的檔案變成字串輸出 (jpg embedded markdown syntax)::
        
  $ find *.JPG -exec echo "\![](/images/nature/150214/{})" \; > out.txt


算某目錄下的檔案數目

.. code-block:: bash

  $ find DIR_NAME -type f | wc -l

把一天前更動的檔案搬到...::


  $ find * -mtime -1; # 找出一天內的檔案
  $ find * -mtime -1 -exec mv {} PATH/TO/TARGET \; # 找出, 並搬
  # mtime: 更動, -n n天內, +n n天以上, atime: 存取 (access), ctime: 修改 (change)

** 取出檔案的某幾行**

  sed -n 144827,144878p access.log


把換行改成",":

.. code-block::

   tr -s '\r\n' ',' # -s (squeeze), 不然會有 2 個 ,



**改檔名**

大寫改小寫::

  $ for i in $( ls | grep [A-Z] ); do mv -i $i `echo $i | tr 'A-Z' 'a-z'`; done


只取數字, JPG改jpg::

  for i in *.JPG ; do mv "$i" `echo $i | tr -dc '[0-9]'`".jpg" ; done


for loop::

  for f in *.html; do
      base=`basename $f .html`
      mv $f $bae.php
  done


**找檔案**


複製某時間產生/修改的檔案

.. code-block:: bash

  find -type f -newerct 2020-01-01 # find file's create time order then 2020-01-01
  find -type d -not -newermt 2020-01-01 -exec -r "{}" <SOMEWHERE> \; # find directory's modified time less than 2020-01-01 and copy to <SOMEWHERE>


找目錄名稱::

  $ find / -type d -name "dir_name"

刪除5分鐘內建立的檔案::

  $ find -cmin -5 -type f -exec rm -rf {} \;

找出體積最大前十檔案/目錄:: 
  
  $ du -a /home | sort -n -r | head -n 10

via: `Linux 下找出體積最大的檔案/目錄 – 網絡技術日誌 <http://www.hkcode.com/linux-bsd-notes/693>`__

某目錄下的全部檔案的字串::

  $ grep -rl flaskext . |xargs sed -i -e 's/flaskext/flask.ext/'

  $ ls *.md | xargs sed -i -e 's/static\///' ## 把 所有的.md (markdown) 檔裡的 "static/" 去掉

via: `recursive search and replace old with new string, inside files | commandlinefu.com <http://www.commandlinefu.com/commands/view/4698/recursive-search-and-replace-old-with-new-string-inside-files>`__ 


列出各目錄大小(不顯示子目錄)::

  du -h | grep -v '[a-z]/.'

列出各目錄大小(不顯示子目錄)::

  du -h --max-depth=1


算檔案數量::

  find [PATH] -type f | wc -l
  tree
  # counting hidden files?

  
coding convert::

  # big5 to utf-8
  $ iconv -f big5 -t utf-8 big5.txt -o utf8.txt 

  # 簡體轉繁體
  $ cat test.txt | iconv -f gb2312 -t big5

  # 繁體轉簡體
  $ cat test.txt | iconv -f big5 -t gb2312

  # Big5 編碼跟 UTF-8 編碼之間的轉換,如 UTF-8 轉 Big5
  $ cat test.txt | iconv -f utf-8 -t big5

壓縮/解壓縮 compress/depress
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

  ## tar
  # compress (tar 打包, zip壓縮)
  $ tar -zcvf foo.tgz foo
  # decompress
  $ tar -zxvf foo.tgz

  $ tar -jcvf foo.tar.bz2 foo
  $ tar -jxvf too.tar.bz2

  ## bzip2
  $ bzip2 foo # 會把 foo 變成 foo.bz2
  $ bzip2 -d foo.bz2 # 把 foo.bz2 變 foo

bzip2 的壓縮率比較大, 運算時間長


**grep:**

:-i: 忽略大小寫
:-n: 輸出行號
:-c: 計算次數
:-a: binary 檔用 text 檔案
:-A: 顯示後幾行 (after)
:-B: 顯示前幾行 (before)
:-C: 顯示前後幾行
:-v: --invert-match
:-V: version
:-e: regex (pattern)

.. code-block:: bash

    $ grep -r --include \*.html foo . # find foo only in html file
    $ grep -v -e "bar" # exclude (failed)
 



* `AWK 简明教程 | 酷壳 - CoolShell.cn <http://coolshell.cn/articles/9070.html>`__



user account
------------------

.. code-block:: bash

   $ w # user currently on the machine and thier process
   $ who # who currently logged in
   $ users # users,  It reads all information from /var/run/utmp file.
   $ last {username}

   $ w -i # display IP
   $ who -u # display PID
   # check utmp, wtmp man

   $ kill -HUP {pid}

- `UNIX / Linux List Current Logged In Users - nixCraft <https://www.cyberciti.biz/faq/unix-linux-list-current-logged-in-users/>`__

網路
----------------
::

  $ lsof # list open files
  $ lsof -n # network files
  $ lsof -i # monitors network connections in real time
  $ iftop # shows bandwith usage per *connection*
  $ nethogs #shows the bandwith usage per *process*

  # iOS
  $ sudo lsof -i -P
  $ lsof -n -i4TCP:5000 | grep LISTEN # 找出port5000


rsync::

  $ rsync --ignore-existing -avzh --delete --progress --filter='-p .DS_Store' source dest
  $ rsync -av /etc /tmp () # 將 /etc/ 的資料備份到 /tmp/etc 內(local)
  $ rsync -av --delete src tgt # 把 src 備份到 tgt (tgt 有, src 沒有的檔案會砍掉)
  $ rsync -avP # -P: --partial # (續傳, 中斷的下次再傳, --partial --progress) --progress (顯示進度)
  $ rsync -av -e ssh user@host:/etc /tmp 將遠端 /etc 備份到local主機的 /tmp 內

  # 傳一本停住 (不同 fs timestamp 比對問題?)
  $ rsync -vrc # (不管 linux 的 permission, 用 checksum 而不是 timestamp, 比較慢)
  $ rsync -Prc (P 有 process 比 v 好)
  # -u: 不覆蓋比較新的檔案，只更新來源檔案時間大於目的檔案的時間
  # -z: 在傳輸的過程中進行壓縮
  # -c: checksum
  # --inplace (target disk tmp 不夠大就要用這個)
  # --bwlimit: 限制速度limit I/O bandwidth; KBytes per second
* [詳全文_資料備份同步工具簡介— rsync](http://newsletter.ascc.sinica.edu.tw/news/read_news.php?nid=1742)


    
dns::

  dig foobar.com        # simple query
  dig +trace foobar.com # detail
  dig +short foobar.com # 不廢話
  dig foobar.com mx

*.* 表示the root of the hierarchy

* `使用 netstat 找出不正常的連線 | Tsung's Blog <http://blog.longwin.com.tw/2010/02/netstat-check-connect-2010/>`__


curl::

  curl http://example.com
  curl http://example -d"tag=foo&cat=bar" # 這個預設會送POST
  curl http://example/edit -XPOST # http method
  crul http://example/api -F user=mouse -F passwd=cheese
  curl --X POST -F "foo=bar" -F "myfile=@pathto.local" http://example.com/upload # 上傳檔案
 
  :-X: 	specify HTTP request method e.g. POST
  :-H: 	specify request headers e.g. "Content-type: application/json"
  :-d: 	specify request data e.g. '{"message":"Hello Data"}'
  :--data-binary: 	specify binary request data e.g. @file.bin
  :-i: 	shows the response headers
  :-u: 	specify username and password e.g. "admin:secret"
  :-v: 	enables verbose mode which outputs info such as request and response headers and errors

wget::

  wget -i url_list.txt # 下載文字檔內的 url

SSH
--------------

create id_rsa, id_rsa.pub::
  
  ssh-keygen -t rsa

.ssh/config::
  
  Host qll-staging
      HostName 123.123.123.123
      Port 22
      IdentityFile /path/to/foo.pem
      User ec2-user

  Host bitbucket-moogoo
      HostName bitbucket.org
      User git 
      PreferredAuthentications publickey
      IdentityFile path/to/id_rsa-bitbucket
      IdentitiesOnly yes 

        
Service
-------------------

關掉uwsgi的process::

  ps ca|grep uwsgi |awk '{ print $1}' | xargs --no-run-if-empty sudo kill -9


快速靜態檔案server::

  $ python -m SimpleHTTPServer 8080


Crontab
~~~~~~~~~~~~

`crontab.guru - the cron schedule expression editor <http://crontab.guru/>`__

分鐘 小時 日期 月份 週 

每5min一次::

  */5 * * * * /home/moogoo/test.sh
  5 0 * * *

每小時::

  01 * * * * /home/moogoo/test.sh

每天(半夜)::

  0 0 * * *

每週::

  0 0 * * 0

每月::

  0 0 1 * *


start::

  service crond start 



locale
-------------------------

語系::

  $ locale -a # 目前系統支援語系
  $ dpkg-reconfigure locales 安裝語系


  $ export LC_ALL="en_US.UTF-8"
  $ export LC_CTYPE="en_US.UTF-8"
  $ sudo dpkg-reconfigure locales  



VIM
-------------

行號::

  :set nonu
  :set nu

vim硬是要存檔::

  :w !sudo tee %


replace tabs with single space::

  :%s/\t/ /g


  
pandoc
----------

轉換各種檔案格式

ex: markdown 轉 rst

.. code-block:: bash

   pandoc -f markdown -t rst -o output.rst input.md

  
dot (grphviz)
-----------------------


example::

  digraph foo {
    hello [shape="diamond", label="hihi \nhello"]
    world
    hello -> world [label="Y"]
  }


輸出png::

  dot foo.dot -Tpng -o foo.png

.. note:: -T: format -o: output

.. note:: 註解用\/* \*/ 或 //，像C++一樣

ref:

* `Node Shapes | Graphviz - Graph Visualization Software <http://www.graphviz.org/content/node-shapes>`__
* `The DOT Language | Graphviz - Graph Visualization Software <http://www.graphviz.org/content/dot-language>`__
* `Gallery | Graphviz - Graph Visualization Software <http://www.graphviz.org/Gallery.php>`__


ffmpeg / avconv
-------------------------

debian 改用 avconv (apt-get install libav-tools)

`How to install FFmpeg on Debian? - Super User <http://superuser.com/questions/286675/how-to-install-ffmpeg-on-debian>`__

usage::

  ffmpeg -i [source] [target]

.. note:: -vcodec

.. note:: -s 100x100

.. note:: -t 10 (前10秒)

.. note:: -vf crop=100:100 (切中間100x100), crop=in_w-480:in_h(左右各切240)

.. note:: -aspect 4:3

          
列出所有codecs::

  $ ffmpeg -codecs

  
列出所有file format::

  $ ffmpeg -formats 


Snippets
~~~~~~~~~~~~~~~

**convert:**

.. code-block:: bash

  $ ffmpeg -i filename.webm -acodec libmp3lame -aq 4 filename.mp3
  
  # convert MTS to mp4
  $ ffmpeg -i 00026.MTS -vcodec mpeg4 -b:v 10M -acodec libfaac -b:a 192k out.mp4
  $ ffmpeg -i input.MTS -s hd720 -b 6000k -ar 44100 -ab 224k -ac 2 output.mp4  

  
**meta data:**

.. code-block:: bash

  # show meta data
  $ ffmpeg -i <foo.mp4> -f ffmetadata <out.txt>
  
  $ ffprobe

  
**manuplate:**

.. code-block:: bash

  # clip movie range, from 00:45:00 to 00:48:00 (經過 3 分鐘)
  $ ffmpeg -i 00026.MTS -vcodec mpeg4 -b:v 10M -acodec libfaac -b:a 192k -ss 00:45:00.0 -t 00:03:00.0 out.mp4

  # video 右上角加 watermark:   
  $ ffmpeg –i inputvideo.avi -vf "movie=watermarklogo.png [watermark]; [in][watermark] overlay=main_w-overlay_w-10:10 [out]" outputvideo.flv

via: `How to watermark a video using FFmpeg | iDude.net <http://www.idude.net/index.php/how-to-watermark-a-video-using-ffmpeg/>`__

**聲音:**  

.. code-block:: bash
                
  #聲音檔前面加 1 秒靜音:  
  $ ffmpeg -f lavfi -i aevalsrc=0:0:0:0:0:0::duration=1 silence.mp3 # 產生 1 秒靜音 mp3
  $ ffmpeg -i concat:"silence.mp3|original.mp3" -codec copy combined.mp3 # 合併
  
  # 大小聲
  $ ffmpeg -i silent.mp3 -af "volume=10dB" noise.mp3
  $ ffmpeg -i silent.mp3 -af "volume=-5dB" noise.mp3
  $ ffmpeg -f inputfile -vcodec copy -af "volume/10dB" outputfile

  # 剪接
  $ ffmpeg -ss 23:05 -t 8:20 -i file.mp3 out.mp3 # 從 23 分 5 秒開始, 剪 9 分 20 秒的內容

