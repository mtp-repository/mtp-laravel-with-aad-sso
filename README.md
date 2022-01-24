<p align="center"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"> <br> with Azure Active Directory (AAD) Single Sign On (SSO)</p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About

This repo is to help Laravel developer use the Azure Active Directory Admin Center to enable single sign-on (SSO) for an enterprise application that you added to your Azure Active Directory (Azure AD) tenant. After you configure SSO, your users can sign in by using their Azure AD credentials.

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

- [Adding users that can access the app](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/add-application-portal-assign-users)


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
    - Add column to users table: ALTER TABLE users ADD COLUMN azure_id VARCHAR(36) AFTER id;
    - Make password nullable: ALTER TABLE users MODIFY password varchar(255) null;
```
USE laravel;
ALTER TABLE users ADD COLUMN azure_id VARCHAR(36) AFTER id;
ALTER TABLE users MODIFY password varchar(255) null;
```

- 'No CSS and JS, app.js and app.css are not found'
Need to run on the project folder:
    - npm install
    - npm run dev (Make Sure Mix is installed properly)

- View changes does not reflect
```
php artisan cache:clear
```
    - or maunally delete the cache view in storage/framework/views [reference](https://stackoverflow.com/questions/37503627/blade-view-not-reflecting-changes)

- For Azure App Service issue: getting 'Forbidden' page and not picking up the .htacces file
    - Check the PHP version, Once I selected PHP 7.4 it worked, some compatibility issue on PHP 8

- For production env, use https
    - On .env change APP_ENV to production
    - On .env change APP_DEBUG to false
    - Code changes: https://stackoverflow.com/questions/35827062/how-to-force-laravel-project-to-use-https-for-all-routes

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
