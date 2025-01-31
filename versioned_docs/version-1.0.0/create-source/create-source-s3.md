---
id: create-source-s3
title: Ingest data from S3 buckets
description: Ingest data from S3 buckets
slug: /create-source-s3
---
Use the SQL statement below to connect RisingWave to an Amazon S3 source.

## Syntax

```sql
CREATE SOURCE [ IF NOT EXISTS ] source_name 
schema_definition
WITH (
   connector='s3',
   connector_parameter='value', ...
)
FORMAT data_format ENCODE data_encode (
   without_header = 'true' | 'false',
   delimiter = 'delimiter'
); 
```

:::info
For CSV data, specify the delimiter in the `delimiter` option in `ENCODE properties`.
:::

import rr from '@theme/RailroadDiagram'

export const svg = rr.Diagram(
    rr.Stack(
        rr.Sequence(
            rr.Terminal('CREATE SOURCE'),
            rr.Optional(rr.Terminal('IF NOT EXISTS')),
            rr.NonTerminal('source_name', 'skip')
        ),
        rr.NonTerminal('schema_definition', 'skip'),
        rr.Sequence(
            rr.Terminal('FORMAT'),
            rr.NonTerminal('format', 'skip')
        ),
        rr.Sequence(
            rr.Terminal('ENCODE'),
            rr.NonTerminal('encode', 'skip'),
            rr.Optional(
                rr.Sequence(
                rr.Terminal('('),
                rr.NonTerminal('encode_parameter', 'skip'),
                rr.Terminal(')'),
                ),
            ),
        ),
        rr.Sequence(
            rr.Terminal('WITH'),
            rr.Terminal('('),
            rr.Stack(
                rr.Stack(
                    rr.Sequence(
                        rr.Terminal('connector'),
                        rr.Terminal('='),
                        rr.NonTerminal('s3', 'skip'),
                        rr.Terminal(','),
                    ),
                    rr.OneOrMore(
                        rr.Sequence(
                            rr.NonTerminal('connector_parameter', 'skip'),
                            rr.Terminal('='),
                            rr.NonTerminal('value', 'skip'),
                            rr.Terminal(','),
                        ),
                    ),
                ),
                rr.Terminal(')'),
            ),
        ),
        rr.Terminal(';'),
    )
);


<drawer SVG={svg} />



**schema_definition**:
```sql
(
   column_name data_type [ PRIMARY KEY ], ...
   [ PRIMARY KEY ( column_name, ... ) ]
)
```

## Parameters

|Field|Notes|
|---|---|
|s3.region_name	|Required. The service region.|
|s3.bucket_name	|Required. The name of the bucket the data source is stored in.	|
|s3.credentials.access| Conditional. This field indicates the access key ID of AWS. It must be used with `s3.credentials.secret`. If not specified, RisingWave will automatically try to use `~/.aws/credentials`.|
|s3.credentials.secret| Conditional. This field indicates the secret access key of AWS. It must be used wtih `s3.credentials.access`. If not specified, RisingWave will automatically try to use `~/.aws/credentials`.|
|match_pattern| Conditional. This field is used to find object keys in `s3.bucket_name` that match the given pattern. Standard Unix-style glob syntax is supported. |
|s3.endpoint_url| Conditional. The host URL for an S3-compatible object storage server. This allows users to use a different server instead of the standard S3 server. |

:::note
Empty cells in CSV files will be parsed to `NULL`.
:::

|Field|Notes|
|---|---|
|*data_format*| Supported data format: `PLAIN`. |
|*data_encode*| Supported data encodes: `CSV`, `JSON`. |
|*without_header*| Whether the first line is header. Accepted values: `'true'`, `'false'`. Default: `'true'`.|
|*delimiter*| How RisingWave splits contents. For `JSON` encode, the delimiter is `\n`. |

## Example
Here are examples of connecting RisingWave to an S3 source to read data from individual streams.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="csv" label="CSV" default>

```sql
CREATE TABLE s(
    id int,
    name varchar,
    age int,
    primary key(id)
) 
WITH (
    connector = 's3',
    s3.region_name = 'ap-southeast-2',
    s3.bucket_name = 'example-s3-source',
    s3.credentials.access = 'xxxxx',
    s3.credentials.secret = 'xxxxx'
) FORMAT PLAIN ENCODE CSV (
    without_header = 'true',
    delimiter = ','
);
```

</TabItem>
<TabItem value="json" label="JSON" default>

```sql
CREATE TABLE s3( 
    id int,
    name TEXT,
    age int,
    mark int,
)
WITH (
    connector = 's3',
    match_pattern = '%Ring%*.ndjson',
    s3.region_name = 'ap-southeast-2',
    s3.bucket_name = 'example-s3-source',
    s3.credentials.access = 'xxxxx',
    s3.credentials.secret = 'xxxxx',
    s3.endpoint_url = 'https://s3.us-east-1.amazonaws.com'
) FORMAT PLAIN ENCODE JSON;
```

</TabItem>
</Tabs>