---
id: view-configure-system-parameters
title: View and configure system parameters
description: View the system parameters and configure their values if needed.
slug: /view-configure-system-parameters
---

## What are system parameters?

System parameters in RisingWave refer to the parameters that advanced users can use to adjust how internal components work in RisingWave.

Currently, these system parameters are availble in RisingWave.

| Parameter           |    Description    |
|---|---|
|`barrier_interval_ms`     | The time interval of the periodic barriers.|
|`checkpoint_frequency`      | Specify the number of barriers for which a checkpoint will be created. The value must be a positive integer.|
|`sstable_size_mb`          | The target size of SSTable.|
|`block_size_kb`          | The size of each block in bytes in SSTable.|
|`bloom_false_positive`     | False positive rate of bloom filter in SSTable.|
|`state_store`             | The state store URL. |
|`data_directory`           | The remote directory for storing data and metadata objects.|
|`backup_storage_url`       | The URL of the remote storage for backups.|
|`backup_storage_directory` | The directory of the remote storage for backups.|
|`telemetry_enabled` | Whether to enable telemetry or not. For more information, see [Telemetry](/telemetry.md).|

## How to view system parameters?

You can use the `SHOW PARAMETERS` command to view the system parameters, along with their current values.

The `Mutable` column indicates whether the parameter can be altered using the [ALTER SYSTEM SET](#how-to-adjust-system-parameters) command after the system is running. `t` means it can be altered using the `ALTER SYSTEM SET` command while `f` means it cannot be altered using the command.

```sql
SHOW PARAMETERS;
```

```
           Name           |     Value      | Mutable 
--------------------------+----------------+---------
 barrier_interval_ms      | 1000           | f
 checkpoint_frequency     | 10             | t
 sstable_size_mb          | 256            | f
 block_size_kb            | 64             | f
 bloom_false_positive     | 0.001          | f
 state_store              | hummock+memory | f
 data_directory           | hummock_001    | f
 backup_storage_url       | memory         | f
 backup_storage_directory | backup         | f
 telemetry_enabled        | true           | t
```

## How to configure system parameters?

Mutable and immutable parameters are configured differently.

You can configure mutable parameters using the `ALTER SYSTEM SET` command in `psql`.

The full syntax of the `ALTER SYSTEM SET` statement is:

```sql
ALTER SYSTEM SET parameter_name { TO | = } { value | 'value' | DEFAULT };
```

Immutable parameters need to be initialized in the CLI of the meta node. `state_store` and `data_directory` need to be initialized before starting a cluster.

To configure parameter settings in the CLI of the meta node, navigate to the directory where RisingWave is installed and run the following command:

```shell
meta-node --<parameter_name> <value>
```

For example, to initialize the setting of `data_directory`:

`meta-node --data_directory "hummock_001"`

:::note

As RisingWave reads system parameters at different times, there is no guarantee that a parameter value change takes effect immediately. We recommend that you adjust system parameters before running a streaming query after your RisingWave cluster starts.

:::
