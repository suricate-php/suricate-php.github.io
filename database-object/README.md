# Database Object

## Defining an entity

To define an entity, you should at least create a class inherited from `\Suricate\DBObject` with the following properties and constructor :

```php
<?php
class MyEntity extends \Suricate\DBObject {
    /** @var string Linked SQL Table */
    protected $tableName = 'entities';
    /** @var string Unique ID of the SQL table */
    protected $tableIndex = 'id';
    /** @var string Database config name (optionnal) */
    protected $DBConfig = '';

    public function __construct()
    {
        parent::__construct();
        $this->dbVariables = [
            'id',
            'name',
            'parent_id',
        ];
    }
}
```

The object above refers to a SQL table called `entities`, and its primary key is `id`. SQL table contains fields `id`, `name` and `parent_id`.

## Methods available

##### load()

`load($identifier)` method load an entity from database referenced by the identifier passed as argument.
`$identifier` should be an existing unique id of the database.  
In case of unknown entry, `false` is returned.

```php
$entity = new myEntity();
$entity->load(1);
```

##### isLoaded()

`isLoaded()` returns a boolean when object has been correctly loaded from database. It shoudl return false otherwise (eg when creating an object that has not been already commited to database)

```php
<?php
$entity = new myEntity();
$entity->load(1);
if ($entity->isLoaded()) {
    echo "Entity loaded";
}
```

will output :

> Entity loaded

whereas :

```php
<?php
$entity = new MyEntity();
$entity->id = 3
$entity->name = 'my awesome entity';

if ($entity->isLoaded()) {
    echo "Entity loaded";
} else {
    echo "Entity not loaded";
}
echo "Saving ...<br/>";
$entity->save();
if ($entity->isLoaded()) {
    echo "Entity loaded";
} else {
    echo "Entity not loaded";
}
```

will output :

> Entity not loaded  
> Saving ...  
> Entity loaded

##### loadOrFail(\$identifier)

`loadOrFail($identifier)` is the same method as `load($identifier)` but instead of returning false in case of unknown record in database, it throws an `Suricate\Exception\ModelNotFoundException` exception.

```php
<?php
use Suricate\Exception\ModelNotFoundException;

echo "Loading entity id 999<br/>";
$entity = new myEntity();
try {
    $entity->load(999);
} catch (ModelNotFoundException $e) {
    echo $e->getMessage();
}
```

will result :

> Loading entity id 999  
> Model myEntity not found

##### loadOrCreate()

##### loadOrInstanciate()

##### loadFromSql()

##### instanciate()

##### create()

##### delete()

##### setInsertIgnore(\$flag)

Calling `setInsertIgnore(true)` will force save with the `INSERT IGNORE` syntax (for a newly created object).

```php
<?php
$entity = new MyEntity();
$entity->id = 3
$entity->name = 'my awesome entity';
$entity->setInsertIgnore(true)->save();
```

!> **setInsertIgnore()** is available since Suricate 0.3.2

##### save()

##### validate()

## Relations

Suricate can handle ONE-TO-ONE relations, ONE-TO-MANY relations and MANY-TO-MANY relations.
Relations are set inside a protected function of DBObject `setRelations()`, using the `relations`property of the object.

##### Defining a one to one relation :

One to One relation definition need at least the source field (`source` key) and the target Entity definition.
In the example below, the entity contains fields `id`, `category_id` and `name`.
Relation `category` is based on `category_id` field, and point to an DBObject defined in `\App\Models\Category`class.

```php
<?php
namespace App\Models;

class MyEntity extends \Suricate\DBObject {
    public function __construct()
    {
        parent::__construct();

        $this->dbVariables = [
            'id',
            'category_id',
            'name'
        ];
    }

    protected function setRelations()
    {
        $this->relations = [
            'category' => [
                'type'   => self::RELATION_ONE_ONE,
                'source' => 'category_id',
                'target' => '\App\Models\Category'
            ]
        ];
    }
}
```

Relation can be accessed through the `category` property of `MyEntity`

```php
<?php
$entity = new \App\Models\MyEntity();
$entity->load(1);

$categoryName = $entity->category->name;
```

##### Defining a one to many relation :

##### Defining a many to many relation :

## Export

#### toArray()

#### toJson()

```

```
