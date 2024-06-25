---
tags: [DB, ORM, Phalcon]
title: '#4 - ORM - Relationships'
created: '2024-03-02T18:56:46.084Z'
modified: '2024-03-04T10:18:00.214Z'
---

# #4 - ORM - Relationships

To define relationships between models, you can use Phalcon's fluent interface to set up relationships directly when declaring the model classes. 

For this we need to use the `initialize()` function.

Here's an example:

```php
use Phalcon\Mvc\Model;

class User extends Model
{
    // Define a one-to-many relationship with the Post model
    public function initialize()
    {
        $this->hasMany(
            'id',
            'Post',
            'user_id',
            [
                'alias' => 'posts',
                'reusable' => true // Optional: Indicates whether the relationship can be reused
            ]
        );
    }
}
```

In the above code:

- We're defining a one-to-many relationship between the `User` model and the `Post` model.
- The `hasMany()` method is used to specify the relationship. It takes several parameters:
  - The first parameter (`'id'`) is the local field in the `User` model that relates to the foreign key in the `Post` model.
  - The second parameter (`'Post'`) is the related model.
  - The third parameter (`'user_id'`) is the foreign key field in the related model.
  - The fourth parameter (`['alias' => 'posts']`) specifies an alias for the relationship, which can be used to access related records.
  - The fifth parameter (`['reusable' => true]`) is optional and indicates whether the relationship can be reused.

By defining relationships this way, you avoid the need to override the `initialize()` method explicitly, keeping your code clean and organized. Additionally, this approach allows you to easily see and manage relationships directly within the model class definition.

---

Some examples of relationships in Phalcon ORM:

1. **One-to-Many Relationship**:

Suppose we have two models: `Author` and `Book`. An author can have multiple books, but a book belongs to only one author.

```php
class Author extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->hasMany('id', 'Book', 'author_id', ['alias' => 'Books']);
    }
}

class Book extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->belongsTo('author_id', 'Author', 'id', ['alias' => 'Author']);
    }
}
```

With this setup, you can easily retrieve books by an author or the author of a book:

```php
// Find author and their books
$author = Author::findFirst(1);
foreach ($author->books as $book) {
    echo $book->title;
}

// Find book and its author
$book = Book::findFirst(1);
echo $book->author->name;
```

---

2. **Many-to-Many Relationship**:

Suppose we have models `User` and `Role`, and there's a many-to-many relationship between them. A user can have multiple roles, and a role can be assigned to multiple users.

```php
class User extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->hasManyToMany(
            'id',
            'UserRole',
            'user_id',
            'role_id',
            'Role',
            'id',
            ['alias' => 'Roles']
        );
    }
}

class Role extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->hasManyToMany(
            'id',
            'UserRole',
            'role_id',
            'user_id',
            'User',
            'id',
            ['alias' => 'Users']
        );
    }
}

class UserRole extends \Phalcon\Mvc\Model
{
    public function initialize()
    {
        $this->belongsTo('user_id', 'User', 'id');
        $this->belongsTo('role_id', 'Role', 'id');
    }
}
```

With this setup, you can retrieve roles for a user and users assigned to a role:

```php
// Find user and their roles
$user = User::findFirst(1);
foreach ($user->roles as $role) {
    echo $role->name;
}

// Find role and its users
$role = Role::findFirst(1);
foreach ($role->users as $user) {
    echo $user->username;
}
```

These examples illustrate how to define and use relationships in Phalcon ORM effectively. Adjust them according to your application's specific needs and database structure.
