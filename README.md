This plugin makes dealing with cookies easy. Provides an injectable service and tag to easily get, set, and delete cookies with one line.
It's [RFC 6265](http://tools.ietf.org/html/rfc6265) compliant.

## Installation

To install the cookie plug-in just add to `BuildConfig.groovy`:
```groovy
compile ':cookie:1.0'
```

## Configuration

You can configure in `Config.groovy` how long the default cookie age will be (in seconds) when not explicitly supplied while setting a cookie.
```groovy
grails.plugins.cookie.cookieage.default = 86400 // if not specified default in code is 30 days
```

## Usage

You have two ways to work with cookies:
* The cookie plug-in extends the `request` and `response` objects found in controllers, filters, etc to allow the following.
* The cookie plug-in provides a [CookieService](./grails-app/services/com/dalew/CookieService.groovy) that can be used anywhere in your Grails application.

Example of setting a new cookie:
```groovy
// This sets a cookie with the name `username` to the value `cookieUser123` with a expiration set to a week, defined in seconds
response.setCookie('username', 'cookieUser123', 604800)
// will use default age from Config (or 30 days if not defined)
response.setCookie('username', 'cookieUser123')

// using service
def cookieService // define field for DI
...
cookieService.setCookie('username', 'cookieUser123', 604800)
```

To get the cookie value:
```groovy
request.getCookie('username') // returns 'cookieUser123'

// using service
def cookieService // define field for DI
...
cookieService.getCookie('username') // returns 'cookieUser123'
```

To delete the cookie (actually it set new expired cookie with same name):
```groovy
response.deleteCookie('username') // deletes the 'username' cookie
// using service
def cookieService // define field for DI
...
cookieService.deleteCookie('username')
```

## Configuration
Default expiration age for cookie in seconds. `Max-Age` attribute, integer
If it has value `-1` cookie will not stored and removed after browser close.
If it has null value or unset, will be used 30 days, i.e. 2592000 seconds
Can't has value `0`, because it means that cookie should be removed
```groovy
grails.plugins.cookie.cookieage.default = 360 * 24 * 60 * 60
```

Default path for cookie selection strategy.
* 'context' - web app context path, i.e. `grails.app.context` option in Config.groovy
* 'root' - root of server, i.e. '/'
* 'current' - current directory, i.e. controller name
If default path is null or unset, it will be used 'context' strategy
```groovy
grails.plugins.cookie.path.defaultStrategy = 'context'
```

Default secure cookie param. Secure cookie available only for HTTPS connections. `Secure` attribute, boolean.
If default secure is null or unset, it will set all new cookies as secure if current connection is secure
```groovy
grails.plugins.cookie.secure.default = null
```

Default HTTP only param that denies accessing to JavaScript's `document.cookie`.
If null or unset will be `true
```groovy
grails.plugins.cookie.httpOnly.default = true
```

You can check out [Demo project](https://github.com/stokito/grails-cookie-demo)

## Changelog

### v1.0 Production ready version
[Source](https://github.com/stokito/grails-cookie/releases/tag/v1.0)

- #17 Since v0.1 all deprecated things was removed:
       * Tag `<cookie:get/>`. Use standard `<g:cookie/>` tag instead.
       * Methods `get()`, `set()`, `delete()` from `CookieService`. They are replaced with corresponding `getCookie()`,  `setCookie()`, `deleteCookie()`.
- #19 All cookies should be Version 1 (by RFC 2109) cookie specifications
- #22 Support of cookie attributes
- #10 Improved delete cookie method.  Method delete cookie now can takes a domain name

### v0.60 Last release with deprecated taglib and methods in service
[Source](https://github.com/stokito/grails-cookie/releases/tag/v0.6)

- #3 Fixed `deleteCookie` not works in `response`
- #16 added tests
- #17 Since v0.5 few things was deprecated and will be removed in version v1.0:
       * Tag `<cookie:get/>`. Use standard `<g:cookie/>` tag instead.
       * Methods `get()`, `set()`, `delete()` from `CookieService`. They are replaced with corresponding `getCookie()`,  `setCookie()`, `deleteCookie()`.

### v0.3
[Source](https://github.com/stokito/grails-cookie/releases/tag/v0.3)

In the v0.3 release a big issue was fixed that now sets the cookie's path to the root `/` context.
Otherwise it was setting the path to the same as the controller/service that triggered it.
Most users I believe will want this behavior. If setting the path is desired, that can be accommodated.
Please contact me or do a pull request if you'd like that.
