# Clone plugin overview


??? example "Key takeaways"

    Rapid Instance Cloning: The Clone plugin enables exceptionally fast copying of entire MySQL instances, significantly reducing the time required for data replication and environment setup, especially for large datasets.
    
    Data Consistency and Replication: It ensures a consistent data state during cloning and automatically transfers replication coordinates, simplifying the setup of replica servers.
    
    Simplified and Automated Process: The plugin streamlines the cloning process through easy-to-use commands and automation, minimizing manual steps and reducing the risk of errors.


The Clone plugin lets you clone data from either a local server or from a remote server. The plugin creates a physical snapshot of the data stored in InnoDB, which includes schemas, tables, tablespaces, and data dictionary metadata. The cloned data is a functional data directory and can be used for provisioning a server.

The following table lists the cloning operation types:

| Cloning operation type | Description |
|---|---|
| Local | Clones the data from the server where the operation is initiated to either a directory on the same server or a server node. |
| Remote | Clones the data from the donor to the joiner over the network. |

When replicating a large number of transactions, the Clone plugin may be a more efficient solution. 

## Install the Clone plugin

The Clone plugin must be installed on both the donor and the joiner servers at either server startup or at runtime. To install the plugin at runtime, run the following command:

```{.bash data-prompt="mysql>"}
mysql> INSTALL PLUGIN clone SONAME 'mysql_clone.so';
```

Review the INFORMATION_SCHEMA.PLUGINS table or run the `SHOW PLUGINS` command to verify the installation. The following is an example of querying the PLUGINS table.

```{.bash data-prompt="mysql>"}
mysql> SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME='clone';
```

The result lists the Clone plugin and the status.

For more information about the Clone plugin, see:

* [Load the Clone plugin](load-clone-plugin.md)

* [Use the Clone plugin](clone-plugin-usage.md)

* [Clone plugin limitations](clone-plugin-limitations.md)

## Key terms

| Key Term | Definition |
| :------- | :--------- |
| Binary Log Position | The location within the binary log files. |
| Clone Plugin | A MySQL feature for quickly copying entire MySQL instances. |
| Cloning | The process of creating a duplicate of a MySQL server. |
| DDL (Data Definition Language) | Commands that define database structure (e.g., CREATE TABLE, TRUNCATE TABLE). |
| Data Dictionary Metadata | Information about database objects (tables, schemas, etc.). |
| Donor Instance | The source MySQL server from which data is cloned. |
| GTID (Global Transaction Identifier) | A unique identifier created for each transaction committed on a server. It is used to track transactions across multiple servers in replication setups. |
| GTID Set | A set of Global Transaction Identifiers. |
| InnoDB | The primary storage engine supported by the Clone Plugin. |
| Incremental State Transfer (IST) | The process of transferring only the missing changes from a donor to a recipient. |
| Instance | A running MySQL server. |
| Recipient Instance | The destination MySQL server that receives the cloned data. |
| Replication Coordinates | Information needed for setting up replication (binary log position, GTID set). |
| State Snapshot Transfer (SST) | The process of copying all data from a donor to a recipient. |