# Laravel a Framework for building Modern PHP APPs

## Routing and Controllers

### Page 27 ~ Page 36 (2023-02-22)

1. Routing Handling
   1. Define a closure(function) in the route
   ```PHP
    Route::get('/', function () {
		return 'Hello, World!';
	});
   ```
   2. Pass the **controller** and **method** to the route
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

    <!-- 4.4 Subdomain Routing
    ```php

    ```
    
    ```
    
    ``` -->


