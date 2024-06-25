---
title: '#10 - Prevent initialize function overrides'
created: '2024-03-04T10:14:55.672Z'
modified: '2024-03-04T10:22:50.032Z'
---

# #10 - Prevent initialize function overrides

Let's say we have a `User` model, and we want to add a method to retrieve the user's full name.

```php
use Phalcon\Mvc\Model;

class User extends Model
{
    public function getFullName()
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}
```

In this example, we're not overriding the `initialize()` method. Instead, we're adding a new method `getFullName()` to the `User` model to retrieve the user's full name by concatenating the `first_name` and `last_name` properties.

This approach ensures that the `initialize()` method remains intact for its intended purpose, while allowing us to extend the functionality of the `User` model with custom methods specific to our application's requirements.


