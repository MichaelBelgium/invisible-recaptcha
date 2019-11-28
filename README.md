Invisible reCAPTCHA
==========
![php-badge](https://img.shields.io/badge/php-%3E%3D%205.6-8892BF.svg)
[![packagist-badge](https://img.shields.io/packagist/v/albertcht/invisible-recaptcha.svg)](https://packagist.org/packages/albertcht/invisible-recaptcha)
[![Total Downloads](https://poser.pugx.org/albertcht/invisible-recaptcha/downloads)](https://packagist.org/packages/albertcht/invisible-recaptcha)
[![travis-badge](https://api.travis-ci.org/albertcht/invisible-recaptcha.svg?branch=master)](https://travis-ci.org/albertcht/invisible-recaptcha)

![invisible_recaptcha_demo](http://i.imgur.com/1dZ9XKn.png)

## Why Invisible reCAPTCHA?

Invisible reCAPTCHA is an improved version of reCAPTCHA v2(no captcha).
In reCAPTCHA v2, users need to click the button: "I'm not a robot" to prove they are human. In invisible reCAPTCHA, there will be not embed a captcha box for users to click. It's totally invisible! Only the badge will show on the buttom of the page to hint users that your website is using this technology. (The badge could be hidden, but not suggested.)

## Notice

* This supports multi captchas on page

## Installation

```
composer require albertcht/invisible-recaptcha
```

## Laravel 5

### Setup

Add ServiceProvider to the providers array in `app/config/app.php`.

```
AlbertCht\InvisibleReCaptcha\InvisibleReCaptchaServiceProvider::class,
```

> It also supports package discovery for Laravel 5.5.

### Configuration
Before you set your config, remember to choose `invisible reCAPTCHA` while applying for keys.
![invisible_recaptcha_setting](http://i.imgur.com/zIAlKbY.jpg)

Add `INVISIBLE_RECAPTCHA_SITEKEY`, `INVISIBLE_RECAPTCHA_SECRETKEY` to **.env** file.

```
// required
INVISIBLE_RECAPTCHA_SITEKEY={siteKey}
INVISIBLE_RECAPTCHA_SECRETKEY={secretKey}

// optional
INVISIBLE_RECAPTCHA_BADGEHIDE=false
INVISIBLE_RECAPTCHA_TIMEOUT=5
```

> If you set `INVISIBLE_RECAPTCHA_BADGEHIDE` to true, you can hide the badge logo.

### Usage

Before you render the captcha, please keep those notices in mind:

* `renderCaptchaSubmit()` or `@captchaSubmit` function needs to be called within a form element.
* There can only be one submit button in your form.
* `renderCss()` or `@captchaCss` provides css to hide the badges, in case you set `INVISIBLE_RECAPTCHA_BADGEHIDE` to true
* `renderJs()` or `@captchaScripts` loads the Google reCaptcha API.

##### Display reCAPTCHA in Your View

```php
{!! app('captcha')->renderCaptchaSubmit($text, $cssClass = '', $badgePosition = 'inline') !!}

// or you can use this in blade
@captchaSubmit($text, $cssClass = '', $badgePosition = 'inline')
```

##### Render the css

```php
<head>
{!! app('captcha')->renderCss() !!}

// or you can use this in blade
@captchaCss
</head>
```

##### Render the Javascript

```php
<body>
//...

{!! app('captcha')->renderJs($lang = null) !!}

// or you can use this in blade
@captchaScripts($lang = null)

// blade directive, with language support:
@captchaScripts('en')
</body>
```

##### Validation

Add `'g-recaptcha-response' => 'required|captcha'` to rules array.

```php
$validate = Validator::make(Input::all(), [
    'g-recaptcha-response' => 'required|captcha'
]);

```

## CodeIgniter 3.x

set in application/config/config.php :
```php
$config['composer_autoload'] = TRUE;
```

add lines in application/config/config.php :
```php
$config['recaptcha.sitekey'] = 'sitekey'; 
$config['recaptcha.secret'] = 'secretkey';
// optional
$config['recaptcha.options'] = [
    'hideBadge' => false,
    'timeout' => 5
];
```

In controller, use:
```php
$data['captcha'] = new \AlbertCht\InvisibleReCaptcha\InvisibleReCaptcha(
    $this->config->item('recaptcha.sitekey'),
    $this->config->item('recaptcha.secret'),
    $this->config->item('recaptcha.options'),
);
```

In view, in your form:
```php
<?php echo $captcha->renderCaptchaSubmit($text, $cssClass = '', $badgePosition = 'inline'); ?>
```

Then back in your controller you can verify it:
```php
$captcha->verifyResponse($_POST['g-recaptcha-response'], $_SERVER['REMOTE_ADDR']);
```

## Without Laravel or CodeIgniter

Checkout example below:

```php
<?php

require_once "vendor/autoload.php";

$siteKey = 'sitekey';
$secretKey = 'secretkey';
// optional
$options = [
    'hideBadge' => false
    'timeout' => 5
];
$captcha = new \AlbertCht\InvisibleReCaptcha\InvisibleReCaptcha($siteKey, $secretKey, $options);

// you can override single option config like this
$captcha->setOption('hideBadge', true);

if (!empty($_POST)) {
    var_dump($captcha->verifyResponse($_POST['g-recaptcha-response'], $_SERVER['REMOTE_ADDR']));
    exit();
}

?>

<form action="?" method="POST">
    <?php echo $captcha->captchaSubmit('Submit'); ?>
</form>
```
## Showcases

* [Laravel Boilerplate](https://github.com/Labs64/laravel-boilerplate)

## Credits 

* anhskohbo (the author of no-captcha package)
* [Contributors](https://github.com/albertcht/invisible-recaptcha/graphs/contributors)

## Support on Beerpay
Hey dude! Help me out for a couple of :beers:!

[![Beerpay](https://beerpay.io/albertcht/invisible-recaptcha/badge.svg?style=beer-square)](https://beerpay.io/albertcht/invisible-recaptcha)  [![Beerpay](https://beerpay.io/albertcht/invisible-recaptcha/make-wish.svg?style=flat-square)](https://beerpay.io/albertcht/invisible-recaptcha?focus=wish)
