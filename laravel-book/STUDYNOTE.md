# Laravel a Framework for building Modern PHP APPs

## Routing and Controllers

### Chapter 3 Routing and Controllers

<!-- ************************************************************** -->
<!-- ### Page 27 ~ Page 36 (2023-02-22) -->
<!-- ************************************************************** -->

1. Routing Handling

   1.1. Define a closure(function) in the route

   ```PHP
    Route::get('/', function () {
        return 'Hello, World!';
    });

   ```

   1.2. Pass the **controller** and **method** to the route

   ```PHP
    Route::get('/', 'WelcomeController@index');
   ```

2. Route Parameters

   ```PHP
    Route::get('users/{id}/friends', function ($id) {
    //
    });

    //Optional parameter with question mark(?)
    Route::get('users/{id?}', function ($id = 'fallbackId') {
    //
    });

    //Regular expression route constraints
    Route::get('users/{id}', function ($id) {
    //
    })->where('id', '[0-9]+');
    Route::get('users/{username}', function ($username) {
    //
    })->where('username', '[A-Za-z]+');
    Route::get('posts/{id}/{slug}', function ($id, $slug) {
    //
    })->where(['id' => '[0-9]+', 'slug' => '[A-Za-z]+']);
   ```

3. Route names

    **route** & **name**

   ```PHP
    // Defining a route with name() in routes/web.php:
    Route::get('members/{id}', 'MembersController@show')->name('members.show');
    // Linking the route in a view using the route() helper:
    <a href="<?php echo route('members.show', ['id' => 14]); ?>">
   ```

4. Route groups

    4.1 Middleware

    ```php
        //Restricting a group of routes to logged-in users only
        Route::middleware('auth')->group(function() {
            Route::get('dashboard', function () {
                return view('dashboard');
            });
            Route::get('account', function () {
                return view('account');
            });
        });

        //Applying the rate limiting middleware to a route
        Route::middleware('auth:api', 'throttle:60,1')->group(function () {
            Route::get('/profile', function () {
                //
            });
        });

    ```

    4.2 Path Prefiexes

    ```php
        Route::prefix('dashboard')->group(function () {
            Route::get('/', function () {
                // Handles the path /dashboard
            });
            Route::get('users', function () {
                // Handles the path /dashboard/users
            });
        });
    ```

    4.3 Fallback Routes

    ```php
        Route::any('{anything}', 'CatchAllController')->where('anything', '*');
    ```

    <!-- ************************************************************** -->
    <!-- 2023-02-24(Page 37- Page 46) -->
    <!-- ************************************************************** -->

    4.4 Subdomain Routing

    ```php
    //1st, You may want to present different sections of 
    //the application (or entirely different applications) to different subdomains. 
    //Example 3-14. Subdomain routing
    Route::domain('api.myapp.com')->group(function () {
        Route::get('/', function () {
            //
        });
    });

    //you might want to set part of the subdomain as a parameter, 
    //Example 3-15. Parameterized subdomain routing

    Route::domain('{account}.myapp.com')->group(function () {
        Route::get('/', function ($account) {
        //
        });
        Route::get('users/{id}', function ($account, $id) {
        //
        });
    });

    ```

    4.5 Namespace Prefixes

    ```php
    // App\Http\Controllers\UsersController
    Route::get('/', 'UsersController@index');

    Route::namespace('Dashboard')->group(function () {
        // App\Http\Controllers\Dashboard\PurchasesController
        Route::get('dashboard/purchases', 'PurchasesController@index');
    });
    ```

    4.6 Name Prefixes

    ```php
    //Example 3-17. Route group name prefixes
    //URL: users/comments/show/{id}
    //php route('users.comments.show', [id=>'21']);
    Route::name('users.')->prefix('users')->group(function () {
        Route::name('comments.')->prefix('comments')->group(function () {
            Route::get('{id}', function () {
            })->name('show');
        });
    });
    ```

5. Signed Routes

    ```php
    //In order to build a signed URL to access a given route, the route must have a name.
    Route::get('invitations/{invitation}/{answer}', 'InvitationController')->name('invitations');

    // 1st. Generate a normal link
    URL::route('invitations', ['invitation' => 12345, 'answer' => 'yes']);

    // 2nd. Generate a signed link
    URL::signedRoute('invitations', ['invitation' => 12345, 'answer' => 'yes']);

    // 3rd. Generate an expiring (temporary) signed link
    URL::temporarySignedRoute('invitations', 
    now()->addHours(4), ['invitation' => 12345, 'answer' => 'yes']);
    ```

6. Views

    ```php
    //Example 3-18. Simple view() usage
    Route::get('/', function () {
        return view('home');
    });
    //Example 3-19. Passing variables to views
    Route::get('tasks', function () {
        return view('tasks.index')
        ->with('tasks', Task::all());
    });

    //Example 3-20. Route::view()
    // Returns resources/views/welcome.blade.php
    Route::view('/', 'welcome');
    // Passing simple data to Route::view()
    Route::view('/', 'welcome', ['User' => 'Michael']);
    ```

7. Controller

    7.1 Use **artisan** to create a controller

    ```bash
    php artisan make:controller [ControllerName]
    ```

    <!-- ************************************************************** -->
    <!-- 2023-02-24(Page 47- Page 56) -->
    <!-- ************************************************************** -->