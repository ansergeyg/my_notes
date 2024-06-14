**Nuget package manager notes:**

**Composer package manager notes:**

Sometimes when you install a new package/module you may face dependency incompatibility issues.

Example:

```
# composer require some_repo/some_module -W
./composer.json has been updated
Running composer update some_repo/some_module --with-all-dependencies
Gathering patches for root package.
Loading composer repositories with package information
Updating dependencies                                 
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - laminas/laminas-diactoros[2.11.1, ..., 2.24.x-dev] require psr/http-message ^1.0 -> found psr/http-message[1.0, 1.0.1, 1.1] but these were not loaded, likely because it conflicts with another require.
    - laminas/laminas-diactoros[2.25.0, ..., 2.26.x-dev] require psr/http-message ^1.1 -> found psr/http-message[1.1] but these were not loaded, likely because it conflicts with another require.
    - some_repo/some_module 2.3.0 requires laminas/laminas-diactoros ^2.11.1 -> satisfiable by laminas/laminas-diactoros[2.11.1, ..., 2.26.x-dev].
    - Root composer.json requires some_repo/some_module 2.3.0 -> satisfiable by some_repo/some_module[2.3.0].


Installation failed, reverting ./composer.json and ./composer.lock to their original content.
# composer why psr/http-message
guzzlehttp/psr7                 2.6.2  requires psr/http-message (^1.1 || ^2.0) 
mpdf/mpdf                       v8.2.3 requires psr/http-message (^1.0 || ^2.0) 
mpdf/psr-http-message-shim      v2.0.1 requires psr/http-message (^2.0)         
open-telemetry/sdk              1.0.8  requires psr/http-message (^1.0.1|^2.0)  
php-http/httplug                2.4.0  requires psr/http-message (^1.0 || ^2.0) 
psr/http-client                 1.0.3  requires psr/http-message (^1.0 || ^2.0) 
psr/http-factory                1.1.0  requires psr/http-message (^1.0 || ^2.0) 
sweetrdf/rdf-interface          2.0.0  requires psr/http-message (^1.0 || ^2.0) 
symfony/psr-http-message-bridge v6.4.8 requires psr/http-message (^1.0|^2.0)   
```

We can see that this package differs:

```
mpdf/psr-http-message-shim      v2.0.1 requires psr/http-message (^2.0)
```

It should have been something like:

```
mpdf/psr-http-message-shim      v2.0.1 requires psr/http-message (^10 || ^2.0)
```
How to find it?

If you're using composer you can run command:

```
composer why some_repo/some_module
```
You need to compare allowed versions among all packages that use your dependency package. In this case **mdf/psr-http-message-shim** wasn't really compatible with the lower version of the
***psr/http-message*** package.


Ever wondered if you could patch composer.json file itself?

For example, we could fix the problem above by editing some composer.json file in one of the packages above.

Turns out it's impossible right now:

Because patching occurs after Composer calculates dependencies and installs packages, changes to an underlying dependency's composer.json file introduced in a patch will have no effect on installed packages.

If you need to modify a dependency's composer.json or its underlying dependencies, you cannot use this plugin. Instead, you must do one of the following:

Work to get the underlying issue resolved in the upstream package.
Fork the package and specify your fork as the package repository in your root composer.json
Specify compatible package version requirements in your root composer.json
