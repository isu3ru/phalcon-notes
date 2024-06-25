---
tags: [DB, ORM, Phalcon]
title: '#3 - ORM - Overview of Commonly Used Functions'
created: '2024-03-02T17:53:00.846Z'
modified: '2024-03-03T09:58:15.484Z'
---

# #3 - ORM - Overview of Commonly Used Functions

In the Phalcon PHP framework ORM, there are several commonly used methods to interact with the database.

1. **find()**: This method is used to retrieve records from the database based on certain conditions. For example:

```php
// Find a single record by its primary key
$user = User::find(1);

// Find multiple records using conditions
$users = User::find([
    "conditions" => "status = 'active'",
    "order" => "created_at DESC",
    "limit" => 10
]);
```

2. **findFirst()**: Similar to `find()`, but it returns only the first record matching the conditions:

```php
$user = User::findFirst("status = 'active'");
```

3. **save()**: This method is used to insert or update records in the database. If the record doesn't exist, it will be created, otherwise, it will be updated:

```php
// Create a new user
$user = new User();
$user->name = "John Doe";
$user->email = "john@example.com";
$user->save();

// Update an existing user
$user = User::findFirst(1);
$user->name = "Jane Doe";
$user->save();
```

4. **delete()**: This method is used to delete records from the database:

```php
// Delete a single record
$user = User::findFirst(1);
$user->delete();

// Delete multiple records
User::find("status = 'inactive'")->delete();
```

5. **count()**: This method is used to count the number of records in a table based on certain conditions:

```php
// Count all users
$totalUsers = User::count();

// Count active users
$activeUsers = User::count("status = 'active'");
```

6. **Relationships**:
   - **belongsTo()**: Defines a many-to-one relationship between two models.
   - **hasOne()**: Defines a one-to-one relationship between two models.
   - **hasMany()**: Defines a one-to-many relationship between two models.
   - **hasManyToMany()**: Defines a many-to-many relationship between two models using an intermediate model.

```php
class User extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->hasMany('id', 'Post', 'user_id', ['alias' => 'Posts']);
    }
}
```

7. **Validation**:
   - **validation()**: Defines validation rules for model attributes.
   - **validate()**: Performs validation on model data based on defined rules.
   - **getMessages()**: Retrieves validation messages after calling `validate()`.

```php
use Phalcon\Validation;
use Phalcon\Validation\Validator\Email as EmailValidator;

class User extends \Phalcon\Mvc\Model
{
    public function validation()
    {
        $validator = new Validation();

        $validator->add(
            'email',
            new EmailValidator([
                'message' => 'The email is not valid'
            ])
        );

        return $this->validate($validator);
    }
}
```

8. **Transactions**:
   - **begin()**: Starts a database transaction.
   - **commit()**: Commits the current database transaction.
   - **rollback()**: Rolls back the current database transaction.

```php
use Phalcon\Mvc\Model\Transaction;

$transactionManager = new \Phalcon\Mvc\Model\Transaction\Manager();
$transaction = $transactionManager->get();

$user = new User();
$user->setTransaction($transaction);
$user->name = "John";
$user->save();

// Commit transaction
$transaction->commit();
```

9. **Query Building**:
   - **createQuery()**: Creates a new query object for building complex queries.
   - **execute()**: Executes a raw SQL query.

```php
$query = $this->modelsManager->createQuery("SELECT * FROM User WHERE status = :status");
$result = $query->execute(['status' => 'active']);
```

10. **Metadata**:
   - **getModelsMetaData()**: Retrieves metadata for models, such as column names and data types.

```php
$metadata = $this->modelsMetadata->getMetaData(new User());
```

