# Sms notifications channel for Laravel
This package makes it easy to send notifications using [mobizon.kz](//mobizon.kz) with Laravel.

## Installation

You can install the package via composer:

```bash
composer require aizhar777/laravel_mobizon
```

Publish the configs:
```bash
php artisan vendor:publish --provider="Aizhar777\Mobizon\MobizonServiceProvider"
```

Then you must install the service provider:
```php
// config/app.php
'providers' => [
    ...
    Aizhar777\Mobizon\MobizonServiceProvider::class,
],
```

Install your Api key in the config:
```php
// config/mobizon.php
return [
    'alphaname' => null,
    'secret' => 'Your secret API key',
];
```

## Usage

You can use the channel in your `via()` method inside the notification:

```php
use Illuminate\Notifications\Notification;
use Aizhar777\Mobizon\MobizonMessage;
use Aizhar777\Mobizon\MobizonChanel;

class AccountApproved extends Notification
{
    public function via($notifiable)
    {
        return [MobizonChanel::class];
    }

    public function toMobizon($notifiable)
    {
        return MobizonMessage::create("Task #{$notifiable->id} is complete!");
    }
}
```

In your notifiable model, make sure to include a routeNotificationForMobizone() method, which return the phone number.

```php
public function routeNotificationForMobizone()
{
    return $this->phone;
}
```