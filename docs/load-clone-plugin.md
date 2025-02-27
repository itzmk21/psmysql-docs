# Load the Clone plugin

??? example "Key takeaways"

    * Location of Plugin Library File: Ensure the `mysql_clone.so` file is in the MySQL plugin directory specified by the `plugin_dir` system variable.
    
    * Loading the Plugin: You can load the plugin either at server startup using the `--plugin-load-add` option in the `my.cnf` file or at runtime using the `INSTALL PLUGIN` SQL statement. Both methods ensure the plugin loads correctly.
    
    * Verification and Error Handling: Verify the plugin installation using the `INFORMATION_SCHEMA.PLUGINS` table or the `SHOW PLUGINS` statement. If the plugin fails to initialize, check the server error log for diagnostic messages.
    
You cannot use the `--plugin-load-add` option to load the clone plugin during an upgrade from a previous MySQL version. Attempting to do so will raise an error. Upgrade the server first, then start it with `plugin-load-add=mysql_clone.so`.

## Load the plugin

Locate the Plugin Library File: Ensure the plugin library file (`mysql_clone.so`) is in the MySQL plugin directory (`plugin_dir` system variable). Set `plugin_dir` at server startup if necessary.

=== "At server startup"

    Load the Plugin at Server Startup: Use the `--plugin-load-add` option to name the library file. Add the following lines to your `my.cnf` file:
    
      ```ini
      [mysqld]
      plugin-load-add=mysql_clone.so
      ```
      Restart the server to apply the new settings.
      
  
=== "At runtime"

    Load the Plugin at Runtime: Alternatively, you can load the plugin at runtime with the following SQL statement:
    
      ```sql
      INSTALL PLUGIN clone SONAME 'mysql_clone.so';
      ```
      
      This command loads the plugin and registers it in the `mysql.plugins` table, ensuring it loads automatically at subsequent server startups without `--plugin-load-add`.

## Verify Plugin installation

Check the `INFORMATION_SCHEMA.PLUGINS` table or use the `SHOW PLUGINS` statement to verify the plugin installation:

```sql
SELECT PLUGIN_NAME, PLUGIN_STATUS
FROM INFORMATION_SCHEMA.PLUGINS
WHERE PLUGIN_NAME = 'clone';
```
If the plugin fails to initialize, check the server error log for diagnostic messages.

## Control plugin activation state

If the plugin is registered with `INSTALL PLUGIN` or loaded with `--plugin-load-add`, use the `--clone` option at server startup to control its activation state. To load the plugin at startup and prevent it from being removed at runtime, add these options:

```ini
[mysqld]
plugin-load-add=mysql_clone.so
clone=FORCE_PLUS_PERMANENT
```
   
To prevent the server from running without the clone plugin, use `--clone` with `FORCE` or `FORCE_PLUS_PERMANENT`. These options ensure the server startup fails if the plugin does not initialize successfully.
   
## Key terms
   
| **Term**                        | **Description**                                                                                                      |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Clone Plugin                    | A MySQL plugin that enables the creation of a physical copy of a MySQL server instance.                               |
| Donor                           | The MySQL instance that provides data for the cloning process.                                                       |
| Error Log                       | A log file that records errors and diagnostic messages related to the MySQL server and plugins.                       |
| INSTALL PLUGIN                  | A SQL statement used to load and register a plugin at runtime.                                                       |
| MySQL Configuration File (my.cnf)| The main configuration file for MySQL server settings.                                                              |
| Network Bandwidth               | The capacity of the network to transfer data during the cloning process.                                             |
| Performance Schema              | A MySQL feature that provides insight into server performance and operations, including plugin status.                |
| Plugin Library File             | The file containing the plugin code, such as `mysql_clone.so`.                                                        |
| Plugin_dir                      | A system variable that specifies the directory where MySQL looks for plugin library files.                            |
| Recipient                       | The MySQL instance that receives data during the cloning process.                                                    |
| Server Startup                  | The process of starting the MySQL server, during which plugins can be loaded using configuration options.             |
| SHOW PLUGINS                    | A SQL statement used to display information about installed plugins.                                                 |
| State Snapshot Transfer (SST)   | The process of transferring data between nodes in a Percona XtraDB Cluster.                                           |
| Verification                    | The process of checking that the plugin is correctly installed and functioning using tools like `SHOW PLUGINS`.       |


