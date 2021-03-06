# BigBlueButton Server API Library for Laravel 
[![License](https://img.shields.io/packagist/l/joisarjignesh/bigbluebutton.svg)](https://github.com/joisarjignesh/bigbluebutton/LICENSE.md)
[![Latest Version on Packagist](https://img.shields.io/packagist/v/joisarjignesh/bigbluebutton.svg?style=flat-square)](https://packagist.org/packages/joisarjignesh/bigbluebutton)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://travis-ci.org/joisarjignesh/bigbluebutton)
[![Quality Score](https://img.shields.io/scrutinizer/g/joisarjignesh/bigbluebutton.svg?style=flat-square)](https://scrutinizer-ci.com/g/joisarjignesh/bigbluebutton)
[![Total Downloads](https://img.shields.io/packagist/dt/joisarjignesh/bigbluebutton.svg?style=flat-square)](https://packagist.org/packages/joisarjignesh/bigbluebutton)
![Laravel Framework](https://img.shields.io/badge/laravel-%3E%3D5.5-blue)

Package that provides easily communicate between BigBlueButton server and laravel framework
## Requirements
- Laravel 5.5 or above.

## Installation
You can install the package via composer:

```bash
composer require joisarjignesh/bigbluebutton
```
After install package publish config file
```
php artisan vendor:publish --tag=bigbluebutton-config
```

## Usage
- Define in config/bigbluebutton.php file

``` 
BBB_SECURITY_SALT=bbb_secret_key   
BBB_SERVER_BASE_URL=https://example.com/bigbluebutton/
``` 
 
 After Define salt and url clear old configurations 
 ```
php artisan config:clear
```
 
- You can create meeting in three ways.

1.By Passing Array
```php
\Bigbluebutton::create([
    'meetingID' => 'tamku',
    'meetingName' => 'test meeting',
    'attendeePW' => 'attendee',
    'moderatorPW' => 'moderator'
]); 
```

2.By passing CreateMeetingParameters object for customize create meeting 
```php
use BigBlueButton\Parameters\CreateMeetingParameters;

$meetingParams = new CreateMeetingParameters($meetingID, $meetingName);
$meetingParams->setModeratorPassword('moderatorPassword');
$meetingParams->setAttendeePassword('attendeePassword');

\Bigblubutton::create($meetingParams);
```

3.By passing array it will return CreateMeetingParameters object for overwrite methods
```php
$createMeeting = \Bigbluebutton::initCreateMeeting([
    'meetingID' => 'tamku',
    'meetingName' => 'test meeting',
    'attendeePW' => 'attendee',
    'moderatorPW' => 'moderator',
]);

$createMeeting->setDuration(100); //overwrite default configuration
\Bigbluebutton::create($createMeeting);
``` 

- In create meeting also pass presentation,meta information and endCallbackUrl
  ```php
  \Bigbluebutton::create([
      'meetingID' => 'tamku',
      'meetingName' => 'test meeting',
      'attendeePW' => 'attendee',
      'moderatorPW' => 'moderator',
      'meta' => [ //must be pass as key and value array
          ['key' => 'value'],
          ['key1' => 'value1'],    
      ],
      'endCallbackUrl'  => 'www.example.com',
      'presentation'  => [ //must be array
          ['link' => 'https://www.example.com/doc.pdf', 'fileName' => 'doc.pdf'], //first will be default and current slide in meeting
          ['link' => 'https://www.example.com/php_tutorial.pptx', 'fileName' => 'php_tutorial.pptx'],
      ],
  ]); 
  ```  
    
 

- Get meeting info
```php
use JoisarJignesh\Bigbluebutton\Facades\Bigbluebutton;

Bigbluebutton::getMeetingInfo([
    'meetingID' => 'tamku',
    'moderatorPW' => 'moderator' //moderator password set here
]);
```


- Join meeting ( by default it will redirect into BigBlueButton Server And Join Meeting)
```php
use JoisarJignesh\Bigbluebutton\Facades\Bigbluebutton;

return response()->to(
 Bigbluebutton::join([
    'meetingID' => 'tamku',
    'userName' => 'disa',
    'password' => 'attendee' //which user role want to join set password here
 ])
);
```

- Join meeting but does want to redirect into BigBlueButton server
```php
\Bigbluebutton::join([
    'meetingID' => 'tamku',
    'userName' => 'disa',
    'password' => 'attendee', //which user role want to join set password here
    'redirect' => false, //it will not redirect into bigblueservr
]);
```


- Close meeting
```php
use JoisarJignesh\Bigbluebutton\Facades\Bigbluebutton;

Bigbluebutton::close([
    'meetingID' => 'tamku',
    'moderatorPW' => 'moderator' //moderator password set here
]);
```

- Start metting (if will check first meeting is there or not if not then create meeting and join meeting else meeting
 is there then it directly join a meeting user join as moderator)
 ```php
 $url = \Bigbluebutton::start([
     'meetingID' => 'tamku',
     'moderatorPW' => 'moderator', //moderator password set here
     'attendeePW' => 'attendee', //attendee password here
     'userName' => 'John Deo',//for join meeting 
     //'redirect' => false // only want to create and meeting and get join url then use this parameter 
 ]);

return response()->to($url);
 ```
 
- Get all meetings  
```php
\Bigbluebutton::all();
```


- Is meeting running
```php
Bigbluebutton::isMeetingRunning([
    'meetingID' => 'tamku',
]);
```

- Get recordings 
```php
\Bigbluebutton::getRecordings([
    'meetingID' => 'tamku',
    //'meetingID' => ['tamku','xyz'], //pass as array if get multiple recordings 
    //'recordID' => 'a3f1s',
    //'recordID' => ['xyz.1','pqr.1'] //pass as array note :If a recordID is specified, the meetingID is ignored.
    // 'state' => 'any' // It can be a set of states separate by commas  
]);
```

- Publish Recordings
```php
\Bigbluebutton::publishRecordings([
   'recordID' => 'a3f1s',
    //'recordID' => ['xyz.1','pqr.1'] //pass as array if publish multiple recordings
   'state' => true //default is true  
]);
```

- Delete recordings 
```php
\Bigbluebutton::deleteRecordings([
    //'recordID' => 'a3f1s',
    'recordID' => ['a3f1s','a4ff2'] //pass array if multiple delete recordings
]);
```

### More Information Read This [wiki](https://github.com/bigbluebutton/bigbluebutton-api-php/wiki) 
### For Bigbluebutton Api Testing See This [ApiMate](https://mconf.github.io/api-mate/) 
### See Bigbluebutton Official dev Api   [Bigbluebutton](https://docs.bigbluebutton.org/dev/api.html) 

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email jigneshjoisar@gmail.com instead of using the issue tracker.

## Credits

- [Jignesh Joisar](https://github.com/joisarjignesh)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.



