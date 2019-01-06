# Database Object

## Defining an entity

## Methods available

##### load()

##### isLoaded()

##### loadOrFail()

##### loadOrCreate()

##### loadOrInstanciate()

##### loadFromSql()

##### instanciate()

##### create()

##### delete()

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
