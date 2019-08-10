# Events

Suricate, since version 0.3.0, comes with an Event Listener support.

## Configuration

Event Listeners are configured in [application ini file](/directory-structure/?id=the-config-directory), under the section `[EventDispatcher]` :

```
[EventDispatcher]
Dispatcher1.event = \App\Events\TestEvent;
Dispatcher1.listeners[] = '\App\Events\TestListener1';
Dispatcher2.event = test.dd
Dispatcher2.listeners[] = '\App\Events\TestListener2';
```

Each event must be associed to one or more listeners. An Event can be defined in two ways : it can be a simple string such as `my.event`, or if you prefer, as as class, inherited from the base `\Suricate\Event` class.

## Create an event

### Create event as a string

All you need to create a event type as a string is to declare it in config file :

```
[EventDispatcher]
Dispatcher1.event = my.event
Dispatcher1.listeners[] = '\App\Events\TestListener';
```

### Create event as an object

In the second method, the minimum definition of an event, is as follows, and should be stored in `/app/events` directory:

```php
<?php

declare(strict_types=1);

namespace App\Events;

class TestEvent extends \Suricate\Event
{
    const EVENT_TYPE = 'event.test';
}
```

In the code above, `EVENT_TYPE` is the name of the event and should be unique.
In the object way, events are holding their own payload: it's up to you to define the content of the event. When dispatched, those kind of event are their own payload and are passed to EventListener.

## Create an EventListener

All Suricate Event Listeners are created with objects that extends abstract class `\Suricate\Interfaces\EventListener`:

```php
<?php

declare(strict_types=1);

namespace Suricate\Event;

abstract class EventListener
{
    protected $payload;

    public function __construct(mixed $payload)
    {
        $this->payload = $payload;
    }

    /**
     * Event listener handle method
     *
     * @return boolean|void returning false stop event propagation
     */
    abstract public function handle();
}
```

You should at least implement the `handle()` method that'll be responsible of the event handling.  
Depending of the event type you choosed earlier (string or object) the type of the payload may vary. In a string event, the payload should be passed as an argument to the dispatch function, whereas in an object way, the Event object dispatched is the payload itself.

## Chaining listeners

You can define multiple event listeners for a given event type, as follows:

```
[EventDispatcher]
Dispatcher1.event = \App\Events\TestEvent;
Dispatcher1.listeners[] = '\App\Events\TestListener1';
Dispatcher1.listeners[] = '\App\Events\TestListener2';
Dispatcher1.listeners[] = '\App\Events\TestListener3';
```

When a `\App\Events\TestEvent` event is dispatched, it goes through all listeners defined in the config files, in the same order. (Eg above through TestListener1, TestListener2 and TestListener3)

However, you can stop event propagation directly in the event listener's `handle()` method, by returning `false`. For example, if TestListener::handle() method above return false, event won't go through TestListener3.

## Listeners order and priority

As explained above, listeners are chained according to their order in configuration file.  
You can change this order by appending `|XX` after the class name, where `XX` should be an integer describing priority (the lower number, the higher priority)

```
[EventDispatcher]
Dispatcher1.event = \App\Events\TestEvent;
Dispatcher1.listeners[] = '\App\Events\TestListener1|20';
Dispatcher1.listeners[] = '\App\Events\TestListener2|10';
```

## Adding a listener programatically

You can add one or more Event Listener at runtime using the following code :

```php
<?php
Suricate::EventDispatcher()->addListener($event, string $listener, int $priority = 0)
```

- `$event` parameter can be a event type (`my.event` for example) string or a class name (`\App\Events\TestEvent`)
- `$listener` is the classname of the object responsible of event handling (`\App\Events\TestListener1` for example)
- `$priority` is the event listener priority, the lower number, the higher priority

## Dispatching an event

Suricate comes with an handy helper to dispatch an event :

```php
<?php
dispatchEvent($event, $payload = null);
```

Simply pass an instance of your event as for first argument, or a simple string:

```php
<?php
dispatchEvent('my.event', ['id' => 1, 'title' => 'lorem ipsum']);
// or
$event = new \App\Events\TestEvent();
$event->setId(1);
$event->setTitle('lorem ipsum');
dispatchEvent($event);
```

In the second way, the event payload is the event object itself, whereas in the first way, you can pass an optional payload according to your needs.

However, you can still call Suricate own dispatcher like other services, as follows:

```php
<?php

Suricate::EventDispatcher()->fire($event, $payload);
```
