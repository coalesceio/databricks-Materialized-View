# Databricks Materialized View 

The Databricks Materialized View UDN is a versatile Node that allows you to develop and deploy a Materialized View in Databricks.

A [materialized view](https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-syntax-ddl-create-materialized-view) is a view where precomputed results are available for query and can be updated to reflect changes in the input. Each time a materialized view is refreshed, query results are recalculated to reflect changes in upstream datasets. All materialized views are backed by a DLT pipeline. You can refresh materialized views manually or on a schedule.

## Node Configuration

The Databricks Materialized View Stage has 4 configuration groups:

* [Node Properties](#node-properties)
* [General Options](#general-options)
* [Materialized View Options](#materialized-view-options)
* [Schedule Option](#schedule-options)

### Node Properties
There are four configs within the **Node Properties** group.

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | Storage Location where the Materialized View will be created |
| **Node Type** | Name of template used to create node objects |
| **Deploy Enabled** | If TRUE the Node will be deployed or redeployed when changes are detected<br/>If FALSE the Node will not be deployed or will be dropped during redeployment |

### Key Points and Limitations

The following limitations apply to the Databricks Materialized View Node:

* Materialized View automatically infers the structure of the source table and defines the data type of columns.
* Multi-source is not supported with this version of Databricks Materialized View in Coalesce
* Column-level constraints like NOT NULL and column-level descriptions are also not supported with this version of Databricks Materialized View in Coalesce

### General Options

| **Option** | **Description** |
|------------|----------------|
| **Create As** | Choose materialization type-by default it is MATERIALIZED VIEW  |
|**Schedule refresh**| True / False toggle<br/>- **True**: Schedule Option will be visible<br/>- **False**:  Schedule Option will be disabled|

### Materialized View Options
Materialized View Options is available only when Infer MV Structure toggle is False.

| **Option** | **Description** |
|------------|----------------|
| **Table properties** |  |
| **Partition by** | True / False Toggle<br/>- **True**: Enables the Column Dropdown and Textbox to add columns for partitioning <br/>- **False**: Disables the Dropdown and Textbox to add columns  |
|**Table Constraints**| True / False Toggle<br/>- **True**: Primary key and Foreign Key toggle will visible<br/>- **False** : Primary key and Foreign Key toggle will Disable |
|**Primary key**|Visible when Table Constraints is True<br/> options for Primary Key  <br/>- Primary key Name : Primary Key name is added in short <br/>- Primary key columns : Column Dropdown and Textbox to add columns box appear to select primary key  |
|**Foreign Key**|Visible when Table Constraints is True<br/> options for Foreign Key  <br/>- Foreign key attributes : Text box appear where we can add Foreign Key Name,Parent table name,Foreign key columns|
|**Other constraints**|True / False Toggle <br/> Constraints : Text box appear where we can add constraints on selected column<br/>Options<br/>-ColumnName :  From column dropdown we can add columns<br/>-Expected Expression : can give specific Expression<br/>-ON VIOLATION : Two options can be selected from dropdown - Fail Update, Drop Row    |

### Schedule Options

Schedule Options is available only when Schedule Refresh toggle is True

| **Option** | **Description** |
|------------|----------------|
|**Task Schedule**| Options in Task Schedule<br/>- Periodic Schedule<br/>- CRON    |
|**Schedule refesh-time period**|Available when Task Schedule is Set to Periodic Schedule<br/>Options in Schedule refesh-time period<br/>-Every Hours<br/>-Every Days<br/>-Every Weeks |
|**Specific interval of periodic refresh(integer value)**|Available when Task Schedule is Set to Periodic Schedule |
|**CRON string**|Available when Task Schedule is Set to CRON|
|**CRON TIME ZONE**|Available when Task Schedule is Set to CRON |


## Deployment

### Initial Deployment
When deployed for the first time into an environment Materialized View will execute three stages:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Materialized View** | This stage will execute a CREATE OR REPLACE statement and create a Materialized View in the target environment |

### Redeployment

After the Materialized View has deployed for the first time into a target environment, subsequent deployments may result in either altering the Materialized View or recreating the Materialized View.

If a Materialized View is to be altered this will run the following stage:

#### Altering the Materialized View

The following config changes trigger ALTER statements:

1. Add schedule
2. Alter schedule
3. Drop schedule

These execute the two stages:

| **Stage** | **Description** |
|-----------|----------------|
| **Alter Materialized View** | Executes ALTER to modify parameters |

#### Recreating the Materialized View

If anything changes other than the configuration options specified above then the Materialized View will be recreated by running a CREATE OR REPLACE statement.

### Undeployment

If a Materialized View is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher-level environment then the Materialized View in the target environment will be dropped.

This is executed as a single stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Materialized View** | Removes the materialized view |

## Code

* [Node definition](https://github.com/coalesceio/databricks-Materialized-View/blob/main/nodeTypes/MaterializedView-368/definition.yml)
* [Create template](https://github.com/coalesceio/databricks-Materialized-View/blob/main/nodeTypes/MaterializedView-368/create.sql.j2)
* [Run template](https://github.com/coalesceio/databricks-Materialized-View/blob/main/nodeTypes/MaterializedView-368/run.sql.j2)
