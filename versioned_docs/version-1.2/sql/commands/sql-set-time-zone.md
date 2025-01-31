---
id: sql-set-time-zone
title: SET TIME ZONE
description: Set time zone.
slug: /sql-set-time-zone
---

Use the `SET TIME ZONE` command to specify the time zone that should be used to interpret date and time values.

## Syntax

```sql
SET TIME ZONE { time_zone | LOCAL | DEFAULT };
```


import rr from '@theme/RailroadDiagram'

export const svg = rr.Diagram(
    rr.Sequence(
        rr.Terminal('SET TIME ZONE'),
        rr.Choice(1,
            rr.NonTerminal('time_zone'),
            rr.Terminal('LOCAL'),
            rr.Terminal('DEFAULT'),
        ),
        rr.Terminal(';'),
    )
);

<drawer SVG={svg} />



## Parameters

| Parameter                 | Description           |
| ------------------------- | --------------------- |
| *time_zone*             | Specifies the time zone using a valid time zone name, such as "America/New_York" or "Asia/Shanghai". You can find a list of all possible *time_zone* values [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) |
| **LOCAL** | Sets the time zone to the system's local time zone. |
| **DEFAULT** | Sets the time zone to the server's default time zone. |



## Examples

```sql
SET TIME ZONE "America/New_York";
```

```sql
SET TIME ZONE LOCAL;
```

```sql
SET TIME ZONE DEFAULT;
```