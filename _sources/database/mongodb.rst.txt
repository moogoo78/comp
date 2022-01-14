MongoDB
==============

Installation
-------------------

debian
~~~~~~~

debian 版本比較舊, 預設 apt 跑不起來

`Install MongoDB Community Edition on Debian — MongoDB Manual 3.6 <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/>`__

.. code-block:: bash

  $ mongod --dbpath db
  $ mongod --dbpath db --smallfiles
  $ mongod --config /usr/local/etc/mongod.conf # macos


error message: Unrecognized field 'snapshot'

solution => docker

clients only:

  - buster (10): mongo-tools
  - jessie (8): mongo-clients

  
**dump/export/import**

.. code-block:: bash

  $ mongodump # 預設存成 dump 資料夾
  $ mondodump -h <host> -d <db_name> -c <collection_name> -o <output_path>
  $ mongorestore # 預設讀取 dump 資料夾
  $ mongorestore -d <db_name> --gzip <folder>
  $ mongorestore -h <host> -d <db_name> -c <collection_name> <bson_file_path>
  # export
  $ mongoexport -d <db_name> -c <collection_name> -q '{"foo":"bar"}' -o <out.json>
  $ mongoexport -d <db_name> -c <collection_name> -q '{"_id":{"$oid":"some_object_id"}}' -o <out.json>
  # import
  $ mongoimport -d <db_name> -c <collection_name> <out.json>

params

.. code-block::

   -h：         host
   -u：         user
   -p：         password
   --port：     port
   -d：        database
   -c：        collection
   -o：        output (path)



commands
----------------------

.. code-block::

   # show all databases
   show dbs

   # use database
   use myDatabase

   # show all collections
   show collections;

   # create a collection
   db.createCollection("myCollection");

   # update a collection
   # db.<collectionName>.save({"_id": new ObjectId("someObjectId", "field1": "value1", "field2": "value2")});
   # 其他欄位沒有特別寫會被洗掉



Snippets
--------------

select fieldname has value:

.. code-block::

   db.mycollection.find({ 'fieldname' : { $exists: true, $ne: null } });



group, sum:

.. code-block::

   db.<CollectionName>.aggregate([{$group:{_id: "$importNote", amt:{$sum: 1}}}])

   # {"_id": "foo", "amt": 100 }
   # {"_id": "bar", "amt": 300 }
   # {"_id": "baz", "amt": 200 }


aggregate/group/year-month:

.. code-block::

  db.getCollection('Annotations').aggregate( [
  {
    $project: {
      year: {$year: "$createTime"},
      month: {$month: "$createTime"},
    },
  },
  {
    $group: {
      _id: {
        year: '$year',
        month: '$month',
      },
      year: { $first : "$year" },
      month: {$first: "$month"},
      count: {
        $sum: 1
      },
    },
  },
  {
    $sort: {
      '_id.year': -1,
      '_id.month': -1
    }
  },
  ]);



check last modified (orderBy):

.. code-block::

   db.getCollection('Annotations').find().sort({createTime: -1})



