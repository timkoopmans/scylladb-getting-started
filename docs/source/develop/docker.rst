============
Using Docker
============

Run on Docker
-------------

Docker simplifies the deployment and management of ScyllaDB. By using Docker containers, developers can easily create isolated ScyllaDB instances for development, testing, and production. Running ScyllaDB in Docker is the simplest way to experiment with ScyllaDB and we highly recommend it.

Running a Single Node
=====================

.. code-block:: none

  docker run --name scylla -d scylladb/scylla

When you execute this command, Docker will start a new container named "scylla" in detached mode using the ScyllaDB image, allowing it to run in the background.

.. note::

   It will take a minute or so on a decent internet connection to pull the image from Docker hub and start the container. Read on for more details on how to check your node logs and status.

Connecting to your Node
=======================

You can connect to your node with CQLSH using the following command:

      .. code-block:: none

        docker exec -it scylla cqlsh

      The output of this command will look something like this:

      .. code-block:: none

        Connected to  at 172.17.0.2:9042.
        [cqlsh 5.0.1 | Cassandra 3.0.8 | CQL spec 3.3.1 | Native protocol v4]
        Use HELP for help.
        cqlsh>

View your Node Logs
===================

To view the running logs of your node, copy and run this command:

.. code-block:: none

  docker logs -f scylla

The output of this command will look something like this:

.. code-block:: none

  INFO  2023-11-13 04:18:44,449 [shard  0] init - starting the view builder
  INFO  2023-11-13 04:18:44,455 [shard  0] init - starting native transport
  INFO  2023-11-13 04:18:44,456 [shard  0] cql_server_controller - Starting listening for CQL clients on 172.17.0.2:9042 (unencrypted, non-shard-aware)
  INFO  2023-11-13 04:18:44,456 [shard  0] cql_server_controller - Starting listening for CQL clients on 172.17.0.2:19042 (unencrypted, shard-aware)
  INFO  2023-11-13 04:18:44,457 [shard  0] init - serving
  INFO  2023-11-13 04:18:44,457 [shard  0] init - Scylla version 5.2.9-0.20230920.5709d0043978 initialization completed.

Node logs can be useful for troubleshooting and support.

Checking Node Status
====================

You can verify that the cluster is up and running with the following command:

.. code-block:: none

  docker exec -it scylla nodetool status

The output of this command will look something like this:

.. code-block:: none

  Datacenter: datacenter1
  =======================
  Status=Up/Down
  |/ State=Normal/Leaving/Joining/Moving
  --  Address     Load       Tokens       Owns    Host ID                               Rack
  UN  172.17.0.2  632 KB     256          ?       8075882e-3b49-42a4-a742-4caf072844ff  rack1

The status "UN" stands for "Up and Normal". It indicates the node is in a healthy state and actively participating in the data distribution and replication processes.
