# Automatic cron run can sometimes use different url.

You would expect both automatic cron run and manual cron run (via some website UI) both behave equally however it may not be the case.
Sometimes the url you build in code using request api (provided either by Symfony or any other library) can differ depending on the environment the
caller is working on.

Clear examples: CLI call or http level call.

Imagine that hou have queues that run httprequest to some external sever. After that our server is expecting responses from that external server.

In this specific case CLI call (when automatic cron runs) falls back to **http**,

whereas browser UI call (when we manually run cron or queue) uses **https**

# Why automatic cron generated HTTP callback URLs

The module derived its public callback address from the current request.
That request differed between browser and CLI execution.

The acceptance logs showed the following on 10 September 2026:

| Execution | PHP SAPI | Callback scheme | Job ID |
| --- | --- | --- | --- |
| Automatic cron | cli | HTTP | 1162196143 |
| Manual cron button | fpm-fcgi | HTTPS | 1162190507 |

Both submissions were accepted with HTTP status 200. Their callback hostname
and paths matched; the scheme differed. After explicitly requiring HTTPS,
automatic translation delivery worked.

## Browser execution: the incoming request supplies HTTPS

UI cron and UI queue execution run inside a browser request. Drupal creates
its request from the web server's request data.

Exact excerpt from drupal/index.php

~~~php
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
~~~

Symfony detects HTTPS from trusted proxy headers or the web server's HTTPS
variable. The successful manual-cron logs included X-Forwarded-Proto: https.

Exact method from
Symfony Request.php:

~~~php
    public function isSecure(): bool
    {
        if ($this->isFromTrustedProxy() && $proto = $this->getTrustedValues(self::HEADER_X_FORWARDED_PROTO)) {
            return \in_array(strtolower($proto[0]), ['https', 'on', 'ssl', '1'], true);
        }

        $https = $this->server->get('HTTPS');

        return !empty($https) && 'off' !== strtolower($https);
    }
~~~

## CLI execution: the launcher supplies a bootstrap URI

CLI has no incoming browser request. A CLI launcher constructs a synthetic
request using its bootstrap URI. For example, Drush explicitly defaults a
missing scheme to HTTP.

Exact excerpt from
Drush DrupalBoot8.php:

~~~php
        $uri = rtrim($this->uri, '/') . '/';

        $parsed_url = parse_url($uri);

        // Account for users who omit the http:// prefix.
        if (empty($parsed_url['scheme'])) {
            $this->uri = 'http://' . $this->uri;
            $uri = 'http://' . $uri;
            $parsed_url = parse_url($uri);
        }
~~~

Later in that same method:

~~~php
        $request = Request::create($uri, 'GET', [], [], [], $server);
        $request->overrideGlobals();
~~~

The acceptance logs confirm an HTTP CLI request context. They do not identify
the scheduler's exact launcher or configuration source. Drush illustrates the
bootstrap mechanism; these logs alone do not prove that acceptance uses Drush.
The synthetic GET method also does not describe the outgoing translation
submission, which uses POST.

## Why the original code was vulnerable

The original line in
PService.php was:

~~~php
    $this->host = $this->request->getCurrentRequest()->getSchemeAndHttpHost();
~~~

Symfony's exact methods show that the result depends on the current request:

~~~php
    public function getScheme(): string
    {
        return $this->isSecure() ? 'https' : 'http';
    }
~~~

~~~php
    public function getSchemeAndHttpHost(): string
    {
        return $this->getScheme().'://'.$this->getHttpHost();
    }
~~~

Consequently, the same module code received these different origins:

~~~text
UI:        https://accept.somedom.com
Automatic: http://accept.somedom.com
~~~

## Evidence: the callback builder did not explicitly select HTTP

The payload builder simply reused the derived host and appended route paths.
Exact excerpt from PService.php:

~~~php
      'requesterCallback' => $this->host . $callback_ok_url,
      'destinations' => [
        'httpDestinations' => [
          $this->host . $callback_ok_url,
        ],
      ],
      'error-callback' => $this->host . $callback_error_url,
~~~

The httpDestinations field name does not force the HTTP scheme. Its URL value
determines HTTP versus HTTPS. The outgoing submission endpoint was HTTPS in
both runs; it is separate from the callback URLs supplied inside the payload.

## Applied fix

Exact replacement in PService.php:

~~~php
    // eTranslation callbacks must use the public HTTPS endpoint.
    $this->host = 'https://' . $this->request->getCurrentRequest()->getHttpHost();
~~~

The getHttpHost() method returns a hostname and optional port, without a
scheme, so this cannot concatenate HTTPS with an existing HTTP scheme.
The fix retains the request's hostname, which matched the public hostname
in both acceptance runs.

The original getter behaved correctly. The bug was using a context-dependent
request origin as a canonical public callback address. Requiring HTTPS fixed
the observed failure; the exact reason HTTP delivery failed, such as a
redirect or network restriction, was not separately established.
