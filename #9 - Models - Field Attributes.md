---
title: '#9 - Models - Field Attributes'
created: '2024-03-04T10:21:56.359Z'
modified: '2024-03-04T10:22:43.679Z'
---

# #9 - Models - Field Attributes

You can define attributes in a Phalcon model class to represent the columns in the corresponding database table. This is typically done by declaring public properties in the model class, where each property corresponds to a column in the database table.

Here's an example of how you can define attributes in a Phalcon model class:

```php
use Phalcon\Mvc\Model;

class User extends Model
{
    public $id;
    public $username;
    public $email;
    public $password;
    
    // Other properties...
    
    // Define relationships and other configurations...
}
```

In this example, each public property (`$id`, `$username`, `$email`, `$password`, etc.) corresponds to a column in the `users` table of the database. These properties will be automatically mapped to the respective columns when interacting with the database using Phalcon's ORM.

By defining attributes in the model class, you can access and manipulate the data stored in the corresponding database table through instances of the model class. Additionally, you can define relationships, validations, and other configurations within the same class to encapsulate all aspects of model behavior.

