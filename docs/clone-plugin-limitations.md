# Clone plugin limitations

??? example "Key takeaways"

    * The Clone Plugin does not support cloning encrypted tablespaces.
    
    * The Clone Plugin requires sufficient disk space on both the donor and recipient servers to complete the cloning process.
    
    * The Clone Plugin cannot clone instances with different MySQL versions between the donor and recipient servers.

The clone plugin clones instances within the same MySQL series only. It clones from MySQL 8.0.37 to 8.0.42, but not from 8.0 to 8.4. Before MySQL 8.0.37, it requires matching point release numbers.

Before MySQL 8.0.27, the clone plugin blocks DDL on the donor and recipient during cloning. This includes `TRUNCATE TABLE`. Use dedicated donor instances to work around this. DML continues during cloning.

From MySQL 8.0.27, the clone plugin allows concurrent DDL on the donor by default. The `clone_block_ddl` variable controls this.

The clone plugin clones from a donor to a hotfix instance of the same version from MySQL 8.0.26 onward.

The clone plugin clones one MySQL instance at a time. It does not clone multiple instances in one operation.

The clone plugin does not support the X Protocol port (mysqlx_port) for remote cloning.

The clone plugin does not clone MySQL server configurations. The recipient keeps its own settings.

The clone plugin does not clone binary logs.

The clone plugin clones InnoDB data only. It clones other storage engine tables, like MyISAM and CSV, as empty tables.

The clone plugin does not support connections to the donor through MySQL Router.

Local cloning does not clone general tablespaces created with absolute paths. This prevents file path conflicts.

## Key terms

| Term                            | Description                                                                                         |
|---------------------------------|-----------------------------------------------------------------------------------------------------|
| Cloning Encrypted Tablespaces   | The Clone Plugin does not support cloning of encrypted tablespaces.                                  |
| Disk Space                      | Sufficient disk space is required on both donor and recipient servers to complete the cloning process.|
| Different MySQL Versions        | The Clone Plugin cannot clone instances with different MySQL versions between donor and recipient.   |