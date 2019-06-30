# Services

## Error

## Cache

##### File

##### APC

##### Memcache

## Session

##### Cookies

##### Memcache

##### Native

## Curl

## I18n

## Logger

## Redis Worker

You can create a simple queue system based on REDIS fifos with `\Suricate\Worker\Redis` class.  
Redis worker is based on [Predis client](https://github.com/nrk/predis), and thus must be installed as a dependency of your project.  
All you need is to create a child class that inherits `\Suricate\Worker\Redis` class that built as follows :

```php
class MyWorker extends \Suricate\Worker\Redis
{
    protected $redisHost = 'redis';
    protected $redisPort = 6379;
    protected $redisFifoName = 'fifo:test';
    protected $maxChildren = 10;

    public function handleJob($payload)
    {
        $this->log('job received: ' . json_encode($payload));
    }
}
```

`$redisHost` and `$redisPort` are the hostname:port of your redis server.  
`$redisFifoName`is the name of the Redis key that's act as a FIFO.  
You can enqueue jobs with RPUSH command, whereas the worker consume jobs with a BLPOP command.  
A sample enqueue can be written like this:

```php
$redisSrv = new \Predis\Client([
    'scheme' => 'tcp',
    'host' => 'redis',
    'port' => '6379',
    'read_write_timeout' => 0
]);

$job = ['type' => "myJobType", "payload" => "1234"];

$redisSrv->rpush("fifo:test", json_encode($job));

```
