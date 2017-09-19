# path

tip:


>To achieve consistent results when working with Windows file paths on any operating system, use `path.win32`

>To achieve consistent results when working with POSIX file paths on any operating system, use `path.posix`


1. join
>The path.join() method joins all given path segments together using the platform specific separator as a delimiter, then normalizes the resulting path.
```

> path.join('/a', '.')
'/a'
> path.join('/a', 'b','.')
'/a/b'
> path.join('/a', '','b','.')
'/a/b'
> path.join('/a', '','b','.', 'cc')
'/a/b/cc'
> path.join('/a', '','b','.', 'cc', '..')
'/a/b'
> path.join('/a', '','b','.', 'cc', '..', 'd')
'/a/b/d'
> path.join('/a', '','b','.', 'cc', '../', 'd')
'/a/b/d'
> path.join('/a', '','b','.', 'cc', '../es', 'd')
'/a/b/es/d'
```

2. path.basename(path[, ext])
3. path.delimiter
  Provides the platform-specific path delimiter:
```
  ; for Windows
  : for POSIX
```
4. path.dirname(path)
The path.dirname() method returns the directory name of a path, similar to the Unix dirname command. 
5. path.extname(path)
6. path.format(pathObject)
> The path.format() method returns a path string from an object. This is the opposite of `path.parse()`.
>
* pathObject.root is ignored if pathObject.dir is provided
* pathObject.ext and pathObject.name are ignored if pathObject.base exists
>

7. path.isAbsolute(path)
8. path.join([...paths])

* ...paths `<string>` A sequence of path segments
* Returns: `<string>`
> The path.join() method joins all given path segments together using the platform specific separator as a delimiter, then normalizes the resulting path.

9. path.normalize(path)

* path `<string>`
* returns `<string>`

10. path.parse(path)

* path`<string>`
* returns `<object>`
```
The returned object will have the following properties:

dir <string>
root <string>
base <string>
name <string>
ext <string>
```


11. path.posix
The path.posix property provides access to POSIX specific implementations of the path methods.

12. path.relative(from, to)

* from `<string>`
* to `<string>`
* Returns: `<string>`

13. path.resolve([...paths])

* ...paths `<string>` A sequence of paths or path segments
* Returns: `<string>`

> The path.resolve() method resolves a sequence of paths or path segments into an absolute path.

> The given sequence of paths is processed from right to left, with each subsequent path prepended until an absolute path is constructed. For instance, given the sequence of path segments: /foo, /bar, baz, calling path.resolve('/foo', '/bar', 'baz') would return /bar/baz.

> If after processing all given path segments an absolute path has not yet been generated, the current working directory is used.

> The resulting path is normalized and trailing slashes are removed unless the path is resolved to the root directory.

> Zero-length path segments are ignored.

> If no path segments are passed, path.resolve() will return the absolute path of the current working directory.

For example:
```
path.resolve('/foo/bar', './baz');
// Returns: '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/');
// Returns: '/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
// if the current working directory is /home/myself/node,
// this returns '/home/myself/node/wwwroot/static_files/gif/image.gif'

```

14. path.sep
`<string>`
Provides the platform-specific path segment separator:

* \ on Windows
* / on POSIX
```
For example on POSIX:
'foo/bar/baz'.split(path.sep);
// Returns: ['foo', 'bar', 'baz']
On Windows:
'foo\\bar\\baz'.split(path.sep);
// Returns: ['foo', 'bar', 'baz']

```
path.win32
The path.win32 property provides access to Windows-specific implementations of the path methods.
