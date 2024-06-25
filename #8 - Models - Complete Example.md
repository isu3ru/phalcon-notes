---
title: '#8 - Models - Complete Example'
created: '2024-03-04T10:19:36.852Z'
modified: '2024-03-04T10:21:41.821Z'
---

# #8 - Models - Complete Example

Here's a complete example of Phalcon models demonstrating custom methods, relationships, and other configurations:

Let's consider a simple blogging application with two main entities: `User` and `Post`. Each user can have multiple posts, and each post belongs to a user.

```php
// User.php
use Phalcon\Mvc\Model;

class User extends Model
{
    public function initialize()
    {
        // Define a one-to-many relationship with the Post model
        $this->hasMany(
            'id',
            'Post',
            'user_id',
            [
                'alias' => 'posts',
                'reusable' => true // whether the relationship can be reused
            ]
        );
    }

    // Custom method to get user's full name
    public function getFullName()
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}

// Post.php
use Phalcon\Mvc\Model;

class Post extends Model
{
    public function initialize()
    {
        // Define a many-to-one relationship with the User model
        $this->belongsTo(
            'user_id',
            'User',
            'id',
            [
                'alias' => 'user'
            ]
        );
    }

    // Custom method to get the author's username of the post
    public function getAuthorUsername()
    {
        return $this->user->username;
    }
}
```

In this example:

- The `User` model has a one-to-many relationship with the `Post` model, where each user can have multiple posts. It also includes a custom method `getFullName()` to retrieve the user's full name.
- The `Post` model has a many-to-one relationship with the `User` model, indicating that each post belongs to a single user. It includes a custom method `getAuthorUsername()` to retrieve the username of the post's author.

With these models, you can perform operations such as fetching posts for a user, getting the author's username for a post, and accessing custom methods like `getFullName()` to retrieve additional information about users.

This example demonstrates how you can define relationships and add custom methods to your Phalcon models to suit the requirements of your application.
