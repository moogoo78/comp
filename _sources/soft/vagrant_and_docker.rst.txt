Vagrant, Docker
=====================

Vagrant
----------

方便快速產生虛擬機環境的工具,

需安裝: VirtualBox



    



QuickStart
~~~~~~~~~~~~~


設定檔 Vagrantfile (init 後產生, Ruby synctax)

快速:

.. code-block:: bash

    $ vagrant init chef/centos-6.5 # 他只是產生 box 設定爲 chef/centos-xx 的 Vagrantfile
    $ vagrant up
    # or
    $ vagrant init debian/jessie64; vagrant up --provider virtualbox

    $ vagrant ssh
    
    # box 另外裝 centos
    $ vagrant box add chef/centos-7.0
    # 列出現在有 image
    $ vagrant box list

    ## 2016
    $ vagrant box add {url}
    $ vagrant box list # 看 title 名稱
    $ vagrant init {title}
    $ vagrant up
  
* `A list of base boxes for Vagrant - Vagrantbox.es <http://www.vagrantbox.es/>`__
* `Discover Vagrant Boxes | Atlas by HashiCorp <https://atlas.hashicorp.com/boxes/search>`__

要改 Vagrantfile::

    Vagrant.configure("2") do |config|
        config.vm.box = "chef/centos-6.5"
        config.vm.network "forwarded_port", guest: 5000, host: 8080 # host 的 127.0.0.1:8080 會連到 guest 的 port 5000
    end

    
其他commend:

.. code-block:: bash
  
    $ vagrant reload
    $ vagrant list
    $ vagrant status
    $ vagrant halt


ssh 進入, 開始弄環境

python version::

    Python 2.6.6 (r266:84292, Nov 22 2013, 12:16:22) 
    [GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2

    # 系統
    $ yum update -y
    $ sudo yum install -y git vim # tools
    $ sudo yum install -y gcc mysql-python mysql-devel python-devel # for mysql-python
    
python 開發::

    $ wget https://bootstrap.pypa.io/get-pip.py
    $ sudo python get-pip.py
    $ sudo pip install virtualenv virtualenvwrapper

加到 .bashrc

    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/Devel
    source /usr/bin/virtualenvwrapper.sh

記得加完後, sourec .bashrc

## flask

設定成 external 外面才可以連進來

    ::: python
    app.run(host='0.0.0.0')


Docker
---------

Mac OS X

1. download Boot2Docker-x.x.x.pkg and install
2. init docker

.. code-block:: bash
   
    $ boot2docker init
    $ boot2docker start

看到以下訊息, 要設定幾個環境變數:

> To connect the Docker client to the Docker daemon, please set: export DOCKER_HOST=tcp://192.168.59.103:2375

測試:

.. code-block:: bash
   
    $ docker run hello-world

安裝最新 centos:

.. code-block:: bash
                
    $ docker run centos:centos6 /bin/bash

remove all

- `How To Remove Docker Images, Containers, and Volumes | DigitalOcean <https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes>`__
