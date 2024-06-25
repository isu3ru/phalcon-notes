---
title: '#5 - Database Metadata'
created: '2024-03-03T10:04:51.618Z'
modified: '2024-03-03T10:20:14.521Z'
---

# #5 - Database Metadata

Database meta data

The line of code:

```php
$metadata = $this->modelsMetadata->getMetaData(new User());
```

fetches metadata information about the `User` model from the Phalcon's models metadata manager. 

The `$metadata` variable will contain information about the structure of the `User` model, such as its 
- table name,
- column names,
- data types,
- primary key,

and any other relevant details.

The exact content of the `$metadata` variable would typically include an array or object with various properties representing the metadata of the `User` model. 

Some common properties you might find in the `$metadata` variable include:

- **Source**: The name of the database table associated with the model (`User`).
- **Fields**: An array containing information about each column in the table, such as name, data type, length, and whether it's part of the primary key.
- **Primary Key**: Information about the primary key of the table, such as its name and which column(s) it consists of.
- **Identity Column**: If the table has an auto-incrementing primary key, this property indicates which column serves as the identity column.
- **Data Types**: Information about the data types of each column.
- **Not Null Fields**: A list of columns that cannot contain null values.
- **Data Types Bind**: Mapping of Phalcon data types to the corresponding SQL data types used in the database.

This metadata can be useful for various purposes such as dynamically generating queries, performing validations, or handling database migrations. It allows developers to access information about the structure of models without explicitly querying the database.

Here's an example of what the `$metadata` variable might contain after executing the code:

```php
$metadata = $this->modelsMetadata->getMetaData(new User());

// Example content of $metadata variable
[
    'source' => 'users',
    'attributes' =>
    [
        'id' => [
            'type' => 0,
            'columnMap' => [
                0 => 'id',
            ],
            'columnType' => 0,
            'notNull' => true,
            'primary' => true,
            'size' => 11,
            'scale' => 0,
            'default' => NULL,
            'identity' => true,
        ],
        'username' => [
            'type' => 2,
            'columnMap' => [
                0 => 'username',
            ],
            'columnType' => 2,
            'notNull' => true,
            'primary' => false,
            'size' => 50,
            'scale' => 0,
            'default' => NULL,
            'identity' => false,
        ],
        'email' => [
            'type' => 2,
            'columnMap' => [
                0 => 'email',
            ],
            'columnType' => 2,
            'notNull' => true,
            'primary' => false,
            'size' => 100,
            'scale' => 0,
            'default' => NULL,
            'identity' => false,
        ],
        // Additional fields...
    ],
    'primaryKeys' => [
        0 => 'id',
    ],
    'nonPrimaryKeys' => [
        0 => 'username',
        1 => 'email',
    ],
    'identityField' => 'id',
]
```

In this example, `$metadata` contains information about the `User` model's table (`users`). It includes details about each column (`id`, `username`, `email`, etc.), such as data type, size, whether it's part of the primary key, and if it's nullable. Additionally, it provides information about the primary key (`id`) and the identity field (`id`), which is typically an auto-incrementing column used as the primary key.
