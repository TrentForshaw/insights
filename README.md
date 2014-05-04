# Insights

Convenient interface for using New Relic Insights.

## Install

Via Composer:

``` json
{
    "require": {
        "sobit/insights": "dev-master"
    }
}
```

## Usage

### 1. Creating your event

Create your event which extends ```\Sobit\Insights\AbstractEvent``` class and set preferred event type.

``` php
class MyEvent extends \Sobit\Insights\AbstractEvent
{
    public function getEventType()
    {
        return 'SomeEvent';
    }
}
```

Populate this class with attributes you want to be submitted to Insights:

``` php
class MyEvent extends \Sobit\Insights\AbstractEvent
{
    private $myAttribute;
    
    public function __construct($myAttribute)
    {
        $this->myAttribute = $myAttribute;
    }
    
    public function getEventType()
    {
        return 'SomeEvent';
    }
}
```

### 2. Submitting events

``` php
use Sobit\Insights\Client\Client;
use Sobit\Insights\EventManager;

// configuration values as per New Relic Insights
$accountId = 'YOUR ACCOUNT ID';
$insertKey = 'YOUR INSERT KEY';
$queryKey  = 'YOUR QUERY KEY';

// initialize core classes
$client       = new Client(new \GuzzleHttp\Client(), $accountId, $insertKey, $queryKey);
$eventManager = new EventManager($client, \JMS\Serializer\SerializerBuilder::create()->build());

// create your event
$event = new \MyEvent('some attribute');

// submit event to Insights
$eventManager->persist($event);
$eventManager->flush();
```

## License

The MIT License (MIT). Please see [License File](https://github.com/sobit/insights/blob/master/LICENSE) for more information.
