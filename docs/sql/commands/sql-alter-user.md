---
id: sql-alter-user
title: ALTER USER
description: Modify properties of a user.
slug: /sql-alter-user
---

Use the `ALTER USER` command to modify the name, password, system permissions, and other properties of an existing user.

## Syntax

```sql title="Alter user name"
ALTER USER user_name 
    RENAME TO new_user_name
```

```sql title="Alter user properties"
CREATE USER user_name [ [ WITH ] system_permission [ ... ]['PASSWORD' { password | NULL }] ];
```

## Parameters

| Parameter or clause | Description           |
| ------------------- | --------------------- |
| *user_name* | The name of the user to be modified. |
| *new_user_name* | The new name of the user. |
| **system_permission* | See [System permissions](/sql/commands/sql-create-user.md#system-permissions).|

## Example

The following statement rename the user `user1` to `user001`.

```sql
ALTER USER user1 RENAME TO user001;
```

The following statement modifies the password and privileges of `user001`.

```sql
ALTER USER user001 NOSUPERUSER CREATEDB PASSWORD '4d2Df1ee5';
```
