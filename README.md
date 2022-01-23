<p align="center"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"> <br> with Azure Active Directory (AAD) Single Sign On (SSO)</p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Installation

### Prerequisite
    - Laravel up and running, if not refer to this link [Laravel Installation](https://laravel.com/docs/8.x/installation)
    - Users to authenticate with the application and "login" [Authentication Installation](https://laravel.com/docs/8.x/authentication)
    - Authenticate with OAuth providers using Laravel Socialite [Socialite Installation](https://laravel.com/docs/8.x/socialite)
    - Install NPM
    - Install Composer

### Setup App Registration
    - Just follow the steps here [Quickstart Register App](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app)
    - Copy the Client ID (overview page) and client secret.

### Setup Laravel
    - Install [metrogistics/laravel-azure-ad-oauth](https://github.com/metrogistics/laravel-azure-ad-oauth)        
```
composer require metrogistics/laravel-azure-ad-oauth:* -w
```        

    - On the env vars of Laravel, place the client ID and client secret.
```    
AZURE_AD_CLIENT_ID=XXXX
AZURE_AD_CLIENT_SECRET=XXXX
```

    - Finally update the database, make password field nullable and add a new field/column called 'azure_id with VARCHAR(36)'    
```
Add column to users table: ALTER TABLE users ADD COLUMN azure_id VARCHAR(36) AFTER id;
Make password nullable: ALTER TABLE users MODIFY password varchar(255) null;
```

## Usage
    - Access the [login page](http://yourdomain.com/login)
    <img src="https://raw.githubusercontent.com/mtp-repository/mtp-laravel-with-aad-sso/main/public/images/login.jpg" width="400">
    - Access [microsoft login page](http://yourdomain.com/login/microsoft)
    <img src="https://raw.githubusercontent.com/mtp-repository/mtp-laravel-with-aad-sso/main/public/images/mslogin.jpg" width="400">


## Known Issue
- 'Forbidden page'
https://stackoverflow.com/questions/18392741/apache2-ah01630-client-denied-by-server-configuration

- 'Error AADSTS50011 - The reply URL specified in the request does not match the reply URLs configured for the application'
https://docs.microsoft.com/en-us/troubleshoot/azure/active-directory/error-code-aadsts50011-reply-url-mismatch
Double check callback URL should be: http://localhost:8999/login/microsoft/callback

- 'Class "\App\User" not found'
solution: https://github.com/JosephSilber/bouncer/issues/539

- 'Class "Metrogistics\AzureSocialite\InvalidStateException" not found'
solution: https://github.com/metrogistics/laravel-azure-ad-oauth/issues/3

- 'Command "make:auth" is not defined.'
https://stackoverflow.com/questions/34545641/php-artisan-makeauth-command-is-not-defined

- 'Laravel\Socialite\Two\InvalidStateException'
Add column to users table: ALTER TABLE users ADD COLUMN azure_id VARCHAR(36) AFTER id;
Make password nullable: ALTER TABLE users MODIFY password varchar(255) null;

- 'No CSS and JS, app.js and app.css are not found'
Need to run on the project folder:
    - npm install
    - npm run dev (Make Sure Mix is installed properly)

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
