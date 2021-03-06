# CakePHP JWT Authenticate plugin

[![Build Status](https://img.shields.io/travis/ADmad/cakephp-jwt-auth/master.svg?style=flat-square)](https://travis-ci.org/ADmad/cakephp-jwt-auth)
[![Coverage](https://img.shields.io/coveralls/ADmad/cakephp-jwt-auth/master.svg?style=flat-square)](https://coveralls.io/r/ADmad/cakephp-jwt-auth)
[![Total Downloads](https://img.shields.io/packagist/dt/ADmad/cakephp-jwt-auth.svg?style=flat-square)](https://packagist.org/packages/ADmad/cakephp-jwt-auth)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE.txt)

Plugin containing AuthComponent's authenticate class for authenticating using
[JSON Web Tokens](http://jwt.io/). You can read about JSON Web Token
specification in detail [here](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-27).

## Requirements

* CakePHP 3.0+

## Installation

_[Composer]_

run: `composer require admad/cakephp-jwt-auth:1.0.x-dev`.

## Usage

In your app's `config/bootstrap.php` add:

```php
// In config/bootstrap.php
Plugin::load('JwtAuth');
```

## Configuration:

Setup the authentication class settings:

```php
    // In AppController::$components
    public $components = [
        'Auth' => [
            'authenticate' => [
                'JwtAuth.Jwt' => [
                    'parameter' => '_token',
                    'userModel' => 'Users',
                    'scope' => ['Users.active' => 1],
                    'fields' => [
                        'id' => 'id'
                    ]
                ]
            ]
        ]
    ];

    // Or in AppController::beforeFilter()
    $this->Auth->config('authenticate', [
        'JwtAuth.Jwt' => [
            'parameter' => '_token',
            'userModel' => 'Users',
            'scope' => ['Users.active' => 1],
            'fields' => [
                'id' => 'id'
            ]
        ]
    ]);
```

## Working

The authentication class checks for the token in two locations:

- `HTTP_AUTHORIZATION` environment variable:

  It first checks if token is passed using `Authorization` request header.
  The value should be of form `Bearer <token>`.

- The query string variable specified using `parameter` config:

  Next it checks if the token is present in query string. The default variable
  name is `_token` and can be customzied by using the `parameter` config shown
  above.

The payload of the token should either have key `id` or `record`. If
`id` key exists it's value will be used to query against the primary key field
of users table.

If `record` key exists it's value will be returned as user record. No check
will be done against the database.

## Further reading

For an end to end usage example check out [this](http://www.bravo-kernel.com/2015/04/how-to-add-jwt-authentication-to-a-cakephp-3-rest-api/) blog post by Bravo Kernel.
