---
title: '#11 - Query inside a Controller'
created: '2024-03-04T10:22:56.954Z'
modified: '2024-03-04T18:18:25.456Z'
---

# #11 - Query inside a Controller

```php
<?php

use Phalcon\Http\Request;
use Phalcon\Mvc\Controller;
use Phalcon\Mvc\Model\Manager;
use Phalcon\Mvc\View;

/**
 * @property Manager $modelsManager
 * @property Request $request
 * @property View    $view
 */
class Invoices extends Controller
{
    public function viewAction()
    {
        $invoiceId = $this->request->getQuery('id', 'int');
        $query     = $this
            ->modelsManager
            ->createQuery(
                'SELECT * FROM Invoices WHERE inv_id = :id:'
            )
        ;

        $invoices = $query->execute(
            [
                'id' => $invoiceId,
            ]
        );

        $this->view->setVar('invoices', $invoices);
    }
}
```
