===============
Install Drivers
===============

To interact with ScyllaDB, developers need to install the appropriate drivers for their programming language. These drivers facilitate communication between the application and the ScyllaDB database, enabling data manipulation and retrieval.

All ScyllaDB Drivers are Shard-Aware and provide additional benefits over third-party drivers.

It is always recommended to use the Scylla drivers over third-party drivers. For a full list of ScyallDB CQL drivers please see our `documentation. <https://opensource.docs.scylladb.com/stable/using-scylla/drivers/cql-drivers/index.html>`_ Following is a list of ScyllaDB Drivers.

.. tabs::

  .. group-tab:: Rust

    For Rust developers, there are specialized drivers available that provide asynchronous, non-blocking access to ScyllaDB. These drivers are designed to leverage Rust's performance and safety features, ensuring efficient and secure database operations. The installation typically involves adding the driver as a dependency in your Cargo.toml file and configuring it to connect to your ScyllaDB instance.

    Run the following Cargo command in project directory:

     .. code-block:: none

       cargo add scylladb

     Or add the relevant version to your Cargo.toml following instructions on `crates.io <https://crates.io//>`_.

  .. group-tab:: Go

    For Golang developers, ScyllaDB has forked the GoCQL Driver which includes enhanced capabilities, taking advantage of ScyllaDB’s unique architecture.

    This is a drop-in replacement to gocql, and it reuses the ``github.com/gocql/gocql`` import path. To install the driver, add the following line to your project go.mod file:

    .. code-block:: none

      replace github.com/gocql/gocql => github.com/scylladb/gocql latest

    and run:

    .. code-block:: none

      go mod tidy

    For more detailed instructions please see our `documentation. <https://opensource.docs.scylladb.com/stable/using-scylla/drivers/cql-drivers/scylla-go-driver.html//>`_

  .. group-tab:: Java

     For Java developers, ScyllaDB has forked the Java Driver from DataStax which includes enhanced capabilities, taking advantage of ScyllaDB’s unique architecture.

     Maven is the suggested tool for managing dependencies and the driver artifacts are published in Maven central, under the group id `com.scylladb <http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.scylladb%22>`_. You should include the following dependencies:

     .. code-block:: none

       <dependency>
         <groupId>com.scylladb</groupId>
         <artifactId>java-driver-core</artifactId>
         <version>${driver.version}</version>
       </dependency>

       <dependency>
         <groupId>com.scylladb</groupId>
         <artifactId>java-driver-query-builder</artifactId>
         <version>${driver.version}</version>
       </dependency>

       <dependency>
         <groupId>com.scylladb</groupId>
         <artifactId>java-driver-mapper-runtime</artifactId>
         <version>${driver.version}</version>
       </dependency>

     For more detailed instructions please see our `documentation. <https://java-driver.docs.scylladb.com/stable/>`_

  .. group-tab:: Python

     For Python developers, ScyllaDB has forked the Python Driver from DataStax which includes enhanced capabilities, taking advantage of ScyllaDB’s unique architecture.

     ``pip`` is the suggested tool for installing packages. It will handle installing all Python dependencies for the driver at the same time as the driver itself. To install the driver:

     .. code-block:: none

       pip install scylla-driver

     For more detailed instructions please see our `documentation. <https://python-driver.docs.scylladb.com/stable//>`_
