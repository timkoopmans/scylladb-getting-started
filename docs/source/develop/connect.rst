============
Connect Apps
============

To connect your application to ScyllaDB, developers need to:

#. Install the relevant driver for your application language: This step involves setting up the driver that is compatible with ScyllaDB. The driver acts as the link between your application and ScyllaDB, enabling your application to communicate with the database.

#. Run the provided example code: Once the driver is installed, you can copy the relevant example code provided below.

.. tabs::

  .. group-tab:: Rust

    .. code-block:: rust

      use std::path::Path;

      use anyhow::Result;
      use scylla::CloudSessionBuilder;

      #[tokio::main]
      async fn main() -> Result<()> {
          println!("Connecting to SNI proxy as described in cloud config yaml ...");

          let session = CloudSessionBuilder::new(Path::new("/file/downloaded/from/cloud/connect-bundle.yaml"))
              .unwrap()
              .build()
              .await
              .unwrap();

          session.query("CREATE KEYSPACE IF NOT EXISTS ks WITH REPLICATION = {'class' : 'SimpleStrategy', 'replication_factor' : 1}",
                          &[]).await.unwrap();
          session
              .query("DROP TABLE IF EXISTS ks.t;", &[])
              .await
              .unwrap();

          println!("Ok.");

          Ok(())
      }

  .. group-tab:: Go

    .. code-block:: go

      package main

      import (
      	"fmt"
      	"log"

      	"github.com/gocql/gocql"
      	"github.com/gocql/gocql/scyllacloud"
      )

      const (
      	connectionBundlePath = "/file/downloaded/from/cloud/connect-bundle.yaml"
      )

      func main() {
      	cluster, err := scyllacloud.NewCloudCluster(connectionBundlePath)
      	if err != nil {
      		log.Fatalf("Failed to create cloud cluster config: %s", err)
      	}
      	cluster.PoolConfig.HostSelectionPolicy = gocql.DCAwareRoundRobinPolicy("us-east-1")

      	session, err := cluster.CreateSession()
      	if err != nil {
      		log.Fatalf("Failed to connect to cluster: %s", err)
      	}

      	defer session.Close()

      	var query = session.Query("SELECT * FROM system.clients")

      	if rows, err := query.Iter().SliceMap(); err == nil {
      		for _, row := range rows {
      			fmt.Printf("%v\n", row)
      		}
      	} else {
      		log.Fatalf("Query error: %s", err)
      	}
      }

  .. group-tab:: Java

    .. code-block:: java

      package org.example;

      import java.io.File;
      import java.io.IOException;

      import com.datastax.driver.core.*;
      import com.datastax.driver.core.policies.DCAwareRoundRobinPolicy;

      public class Main {
          public static void main(String[] args) throws IOException {
              File bundleFile = new File("/file/downloaded/from/cloud/connect-bundle.yaml");

              Cluster cluster = Cluster.builder()
                      .withLoadBalancingPolicy(DCAwareRoundRobinPolicy.builder().withLocalDc("us-east-1").build())
                      .withScyllaCloudConnectionConfig(bundleFile)
                      .build();

              for (Host host: cluster.getMetadata().getAllHosts()) {
                  System.out.printf("Datacenter: %s, Host: %s, Rack: %s\n",
                          host.getDatacenter(), host.getEndPoint(), host.getRack());
              }

              Session session = cluster.connect();

              System.out.println("Connected to cluster " + cluster.getMetadata().getClusterName());
              ResultSet resultSet = session.execute("SELECT * FROM system.clients LIMIT 10");

              for (Row row: resultSet.all()) {
                  System.out.println(row.toString());
              }

              session.close();
              cluster.close();
          }
      }

  .. group-tab:: Python

    .. code-block:: python

      from cassandra.cluster import Cluster, ExecutionProfile, EXEC_PROFILE_DEFAULT
      from cassandra.policies import DCAwareRoundRobinPolicy, TokenAwarePolicy

      PATH_TO_BUNDLE_YAML = '/file/downloaded/from/cloud/connect-bundle.yaml'


      def get_cluster():
          profile = ExecutionProfile(
              load_balancing_policy=TokenAwarePolicy(
                  DCAwareRoundRobinPolicy(local_dc='us-east-1')
              )
          )

          return Cluster(
              execution_profiles={EXEC_PROFILE_DEFAULT: profile},
              scylla_cloud=PATH_TO_BUNDLE_YAML,
              )


      print('Connecting to cluster')
      cluster = get_cluster()
      session = cluster.connect()

      print('Connected to cluster', cluster.metadata.cluster_name)

      print('Getting metadata')
      for host in cluster.metadata.all_hosts():
          print('Datacenter: {}; Host: {}; Rack: {}'.format(
              host.datacenter, host.address, host.rack)
          )

      cluster.shutdown()
