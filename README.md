## Script: Grant_And_Audit_Table_Permissions.sql

**Description**:
This script provides two key functions:
1. Granting explicit permissions (SELECT, INSERT, UPDATE, DELETE) to a user on a specific table.
2. Listing all object-level permissions that have been granted to a particular user.

---

### 🔐 Part 1: Grant Permissions on a Specific Table

```sql
USE YourDatabaseName;  -- Replace with your database name

GRANT SELECT, INSERT, UPDATE, DELETE 
ON [dbo].[TableName] 
TO [UserName];  -- Replace with the target SQL user or login
```

This grants full DML (Data Manipulation Language) access on the table to the user.

✅ You can also grant partial permissions if needed:
```sql
GRANT SELECT, UPDATE ON [dbo].[TableName] TO [UserName];
```

---

### 🔎 Part 2: List All Object-Level Permissions for a Specific User

```sql
SELECT 
    princ.name AS UserName,
    princ.type_desc AS UserType,
    perm.permission_name,
    perm.state_desc AS PermissionState,
    obj.name AS ObjectName,
    obj.type_desc AS ObjectType
FROM sys.database_permissions perm
JOIN sys.database_principals princ 
    ON perm.grantee_principal_id = princ.principal_id
LEFT JOIN sys.objects obj 
    ON perm.major_id = obj.object_id
WHERE princ.name = 'YourUserName'  -- Replace with the actual user name
ORDER BY perm.permission_name;
```

---

### 📋 Output Columns (Permissions List):

| Column            | Description                                      |
|-------------------|--------------------------------------------------|
| `UserName`        | Name of the user or role                         |
| `UserType`        | Type (SQL user, Windows group, etc.)             |
| `permission_name` | The granted permission (e.g., SELECT, EXECUTE)   |
| `PermissionState` | `GRANT`, `DENY`, or `REVOKE`                     |
| `ObjectName`      | Name of the object (table, proc, etc.)           |
| `ObjectType`      | Type of the object (e.g., USER_TABLE, PROCEDURE) |

---

### ✅ Use Case:
- Grant a user access to modify or view specific data
- Audit security on sensitive tables or procedures
- Help with troubleshooting permission issues

