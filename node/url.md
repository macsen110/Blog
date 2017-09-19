# URL
> The url module provides utilities for URL resolution and parsing.

1. Constructor: new URL(input[, base])#
input `<string>` The input URL to parse
base `<string>` | `<URL>` The base URL to resolve against if the input is not absolute.

```
Creates a new URL object by parsing the input relative to the base. If base is passed as a string, it will be parsed equivalent to new URL(base).

const { URL } = require('url');
const myURL = new URL('/foo', 'https://example.org/');
// https://example.org/foo
```

2. url.hash
Gets and sets the fragment portion of the URL.

```
const { URL } = require('url');
const myURL = new URL('https://example.org/foo#bar');
console.log(myURL.hash);
// Prints #bar

myURL.hash = 'baz';
console.log(myURL.href);
// Prints https://example.org/foo#baz
```
2. url.host (`string`)
Gets and sets the host portion of the URL.
```
const { URL } = require('url');
const myURL = new URL('https://example.org:81/foo');
console.log(myURL.host);
// Prints example.org:81

myURL.host = 'example.com:82';
console.log(myURL.href);
// Prints https://example.com:82/foo
```
Invalid host values assigned to the host property are ignored.

3. url.hostname (`<string>`)
Gets and sets the hostname portion of the URL. The key difference between url.host and url.hostname is that url.hostname does not include the port.

```
const { URL } = require('url');
const myURL = new URL('https://example.org:81/foo');
console.log(myURL.hostname);
// Prints example.org

myURL.hostname = 'example.com:82';
console.log(myURL.href);
// Prints https://example.com:81/foo
Invalid hostname values assigned to the hostname property are ignored.
```
4. url.href(`<string>`)

Gets and sets the serialized URL.
```
const { URL } = require('url');
const myURL = new URL('https://example.org/foo');
console.log(myURL.href);
// Prints https://example.org/foo

myURL.href = 'https://example.com/bar';
console.log(myURL.href);
// Prints https://example.com/bar
Getting the value of the href property is equivalent to calling url.toString().
```
Setting the value of this property to a new value is equivalent to creating a new URL object using new URL(value). Each of the URL object's properties will be modified.

If the value assigned to the href property is not a valid URL, a TypeError will be thrown.

5. url.origin(`<string>`)
Gets the read-only serialization of the URL's origin.
```
const { URL } = require('url');
const myURL = new URL('https://example.org/foo/bar?baz');
console.log(myURL.origin);
// Prints https://example.org
const { URL } = require('url');
const idnURL = new URL('https://你好你好');
console.log(idnURL.origin);
// Prints https://xn--6qqa088eba

console.log(idnURL.hostname);
// Prints xn--6qqa088eba 
```
6. url.password (`<string>`)

* Gets and sets the password portion of the URL.

```
const { URL } = require('url');
const myURL = new URL('https://abc:xyz@example.com');
console.log(myURL.password);
// Prints xyz

myURL.password = '123';
console.log(myURL.href);

```
7. url.pathname (`<string>`)
Gets and sets the path portion of the URL.
```
const { URL } = require('url');
const myURL = new URL('https://example.org/abc/xyz?123');
console.log(myURL.pathname);
// Prints /abc/xyz

myURL.pathname = '/abcdef';
console.log(myURL.href);
// Prints https://example.org/abcdef?123
```
8. url.port(`<string>`)
Gets and sets the port portion of the URL.
```
const { URL } = require('url');
const myURL = new URL('https://example.org:8888');
console.log(myURL.port);
// Prints 8888

// Default ports are automatically transformed to the empty string
// (HTTPS protocol's default port is 443)
myURL.port = '443';
console.log(myURL.port);
// Prints the empty string
console.log(myURL.href);
// Prints https://example.org/

myURL.port = 1234;
console.log(myURL.port);
// Prints 1234
console.log(myURL.href);
// Prints https://example.org:1234/

// Completely invalid port strings are ignored
myURL.port = 'abcd';
console.log(myURL.port);
// Prints 1234

// Leading numbers are treated as a port number
myURL.port = '5678abcd';
console.log(myURL.port);
// Prints 5678

// Non-integers are truncated
myURL.port = 1234.5678;
console.log(myURL.port);
// Prints 1234

// Out-of-range numbers are ignored
myURL.port = 1e10;
console.log(myURL.port);
// Prints 1234

```
The port value may be set as either a number or as a String containing a number in the range 0 to 65535 (inclusive). Setting the value to the default port of the URL objects given protocol will result in the port value becoming the empty string ('').

If an invalid string is assigned to the port property, but it begins with a number, the leading number is assigned to port. Otherwise, or if the number lies outside the range denoted above, it is ignored.

9. url.protocol(`<string>`)
Gets and sets the protocol portion of the URL.
```
const { URL } = require('url');
const myURL = new URL('https://example.org');
console.log(myURL.protocol);
// Prints https:

myURL.protocol = 'ftp';
console.log(myURL.href);
// Prints ftp://example.org/
```
Invalid URL protocol values assigned to the protocol property are ignored.

10. url.searchParams (`<URLSearchParams>`)
Gets the URLSearchParams object representing the query parameters of the URL. This property is read-only; to replace the entirety of query parameters of the URL, use the url.search setter. See URLSearchParams documentation for details.
