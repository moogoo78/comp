*********
Redis
*********

How to
========

.. code-block:: bash

   $ redis-cli


.. code-block::

   # check database
   INFO keyspace
   # use which database
   SELECT 0
   SELECT 1
   # display key with patterns
   KEYS *
   # delete key
   DEL to-be-delete-key


