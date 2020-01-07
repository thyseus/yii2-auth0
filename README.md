ATTENTION:
==========

This Project has moved to gitlab: https://gitlab.com/thyseus/yii2-auth0 
Please do not use this repository anymore, thank you.

thyseus\auth0
=============
Yii2 Auth0

this is a fork of the abandoned anli/yii2-auth0 project. It makes it compatible with more recent auth0 plugins.

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

    php composer.phar require --prefer-dist anli/yii2-auth0 "*"

or add

    "anli/yii2-auth0": "*"

to the require section of your `composer.json` file.

Run migration with:

    php yii migrate/up --migrationPath=@vendor/anli/yii2-auth0/migrations

Configuration
-----

Update the `modules` section with:

    'auth0' => array_merge([
        'class' => 'anli\auth0\Module',
        'adminEmails' => ['anli@simbiosis.com.sg'],
    ], require(__DIR__ . '/auth0-local.php')),

Create a new file in `config/auth0-local.php`:

    <?php
    if (YII_ENV_DEV) {
        return [
            'serviceId' => '',
            'domain' => '',
            'clientId' => '',
            'clientSecret' => '',
            'redirectUrl' => '',
            'apiTokens' => [
                'usersRead' => '',
                'usersUpdate' => '',
            ]
        ];
    }

    return [
        'serviceId' => '',
        'domain' => '',
        'clientId' => '',
        'clientSecret' => '',
        'redirectUrl' => '',
        'apiTokens' => [
            'usersRead' => '',
            'usersUpdate' => '',
        ]
    ];

Add to your `.gitignore` file:

    /config/auth0-local.php

Login to auth0 and update the `Allowed Callback Urls` in your setting page.

Update the `components` section in the config with:

    'user' => [
        'identityClass' => 'anli\auth0\models\User',
        'loginUrl' => ['auth0/user/login'],
    ],
    'tenant' => [
        'class' => 'anli\auth0\components\Tenant',
    ],

Usage
-----

Update your `url` section for your login button to `[/auth0/user/login]`.

Update your `url` section for your logout button to `[/auth0/user/logout]`.

To show the login user, use:

    Html::encode(Yii::$app->user->identity->username);

To show the login tenant, use:

    Html::encode(Yii::$app->tenant->identity->name);

To auto update the tenant_id, add to the `behaviors` section of your model with:

    use anli\auth0\behaviors\TenantBehavior;
    ...
    'tenant' => [
        'class' => TenantBehavior::className(),
    ],

FAQs
-----

If you encounter the following error

    \JWT not found

Change the `firebase/php-jwt` version to `v2.2.0`:

    cd @vendor/firebase/php-jwt
    git checkout v2.2.0

Update the `@vendor/composer/autoload_classmap.php` with:

    'BeforeValidException' => $vendorDir . '/firebase/php-jwt/Exceptions/BeforeValidException.php',
    'JWT' => $vendorDir . '/firebase/php-jwt/Authentication/JWT.php',

If you encounter the following error:

    Cannot handle token prior to 2015-08-05T10:42:34+0200

And your system time forward a few minutes.

If you encounter the following error:

    cURL error 60: SSL certificate problem: self signed certificate in certificate chain

Download [CA](http://curl.haxx.se/ca/cacert.pem) to;

    C:\xampp\php\ca\cacert.pem

and update C:\xampp\php\php.ini with

    curl.cainfo=C:\xampp\php\ca\cacert.pem

Restart your apache2 server.
