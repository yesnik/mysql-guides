# Manage MySQL DB structure

## Change column's type

```sql
ALTER TABLE claims_indeces MODIFY claim_id INT(11) UNSIGNED NOT NULL;

-- Define column's default value
ALTER TABLE my_table MODIFY hits bigint(20) UNSIGNED NOT NULL DEFAULT '0';
```

## Add column

```sql
ALTER TABLE claims_indeces ADD COLUMN add_status INT(11) AFTER `status`;
```

## Change emum column

```sql
ALTER TABLE claims MODIFY COLUMN crm_category ENUM('11','Z1','Z2') DEFAULT NULL;
```

