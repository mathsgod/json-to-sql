# JSON to SQL Converter

A PHP tool that converts database table definitions in JSON format to SQL CREATE TABLE statements.

## Features

- ✅ Batch conversion of multiple tables
- ✅ Support for complex field types and attributes
- ✅ Primary Key support
- ✅ Unique Key support
- ✅ Index support
- ✅ Foreign Key support
- ✅ AUTO_INCREMENT support
- ✅ Storage engine configuration (InnoDB, etc.)
- ✅ Charset and Collation support

## Requirements

- PHP >= 8.1
- Composer

## Installation

```bash
composer install
```

## Usage

### Basic Usage

1. Create a `db.json` file in your project root with your table definitions (see [JSON Format Specification](#json-format-specification) below).

2. Create a `convert.php` script to use the converter:

```php
<?php

require_once 'src/JsonToSql.php';

try {
    $converter = new JsonToSql();
    $sql = $converter->convertJsonToSql('db.json');
    
    echo $sql;
    
    // Optional: Save to file
    // file_put_contents('output.sql', $sql);
    
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
    exit(1);
}
```

### Output

The converted SQL statements will be output to the console. You can also save them to a file by uncommenting the `file_put_contents()` line or redirecting the output.

## JSON Format Specification

The JSON file should contain an array of tables, each with the following structure:

### Table Structure

```json
[
  {
    "name": "users",
    "charset": "utf8mb4",
    "collate": "utf8mb4_unicode_ci",
    "engine": "InnoDB",
    "columns": [
      {
        "name": "id",
        "type": "INT",
        "unsigned": true,
        "nullable": false,
        "auto_increment": true
      },
      {
        "name": "username",
        "type": "VARCHAR",
        "length": 255,
        "nullable": false
      },
      {
        "name": "email",
        "type": "VARCHAR",
        "length": 255,
        "nullable": false
      },
      {
        "name": "created_at",
        "type": "TIMESTAMP",
        "default": "CURRENT_TIMESTAMP",
        "nullable": false
      }
    ],
    "primary_key": ["id"],
    "unique_keys": [
      {
        "name": "uk_username",
        "columns": ["username"]
      },
      {
        "name": "uk_email",
        "columns": ["email"]
      }
    ],
    "indexes": [
      {
        "name": "idx_created_at",
        "columns": ["created_at"]
      }
    ],
    "foreign_keys": []
  }
]
```

### Column Attributes

| Attribute | Type | Description | Required |
|-----------|------|-------------|----------|
| `name` | string | Column name | ✅ |
| `type` | string | Column type (VARCHAR, INT, DATETIME, etc.) | ✅ |
| `length` | integer | Column length (for VARCHAR, CHAR, etc.) | ❌ |
| `unsigned` | boolean | Whether unsigned (for numeric types) | ❌ |
| `nullable` | boolean | Allow NULL (default: true) | ❌ |
| `auto_increment` | boolean | Auto increment | ❌ |
| `default` | string\|integer | Default value | ❌ |

### Table Attributes

| Attribute | Type | Description | Default |
|-----------|------|-------------|---------|
| `name` | string | Table name | - |
| `columns` | array | Array of columns | [] |
| `primary_key` | array | Primary key columns | [] |
| `unique_keys` | array | Array of unique keys | [] |
| `indexes` | array | Array of indexes | [] |
| `foreign_keys` | array | Array of foreign keys | [] |
| `engine` | string | Storage engine | InnoDB |
| `charset` | string | Character set | utf8mb4 |
| `collate` | string | Collation | null |

## Foreign Key Configuration

```json
"foreign_keys": [
  {
    "columns": ["user_id"],
    "reference_table": "users",
    "reference_columns": ["id"],
    "on_update": "CASCADE",
    "on_delete": "CASCADE"
  }
]
```

## Examples

Create your `db.json` file in the project root with table definitions following the JSON Format Specification below.

## License

MIT

## Author

Raymond Chong (mathsgod@yahoo.com)
