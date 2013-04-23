# Web server agnostic configuration for PHP web applications

Version 0.1

## Todo:

* Find a better name for the configuration file than the not PHP specific `web.json`.

## Motivation

* Configuring a web server for a 3rd party web application sucks.
* Enable tooling so I don't need to bother with writing web server specific configuration files by myself.
* Make it easy to run web applications provided by third parties (like content management systems, blogging platforms etc.)
* Create a configuration file format which enables Platform-as-a-service (PAAS) providers to offer a "It just works"
  experience for PHP web applications, just like it's possible for Ruby and Python web applications.
* Provide framework agnostic means to document the requirements for running PHP web applications.

## Configuration File

* The configuration file must be valid JSON.
* The configuration file should be named `web.json` and reside in the root of the project. 
* Multiple configuration files are allowed to represent configuration for different environments, for example, `web.dev.json` or `web.staging.json`.
* Tools must let the user specify the name of the desired configuration file.

## Schema

### Properties

All properties are required, except when marked with the text "Optional.".

#### document-root

Directory within the application package which should be publicly accessible on the web.

Examples:

* `.` (project root, not recommended, but necessary for some legacy applications)
* `web`
* `public`
* `htdocs`

#### main

File which should handle all incoming web requests. Typically invokes the application's front controller. The file
path must be treated relative to the `document-root`. 

If `main` is not set, all PHP scripts in the document root must
be served by the web server.

Examples:

* `index.php`
* `app.php`

Optional.

#### deny

Deny access to these files or directories in the document root. Paths must be relative to the `document-root`.

Example:

```json
{
    "deny": [
        "config", ".htaccess"
    ]
}
```

Optional.

#### env

Additional environment variables which should be passed to the application.

Example:

```json
{
    "env": {
        "APP_ENV": "prod"
    }
}
```

Optional.

#### extra

Object which can be used to store custom configuration data, for e.g. a PAAS or a specific web server.

Vendors must not place their properties directly in this object, instead they must use a vendor named property.

Example:

```json
{
    "extra": {
        "heroku": {
            "php-config": ["short_open_tags=on"]
        },
        "nginx": {
            "include": ["etc/nginx.conf"]
        }
    }
}
```

Optional.

## Example

Use the `public` directory as document root, and send all incoming requests to the `index.php` file in the document root:

```json
{
    "document-root": "public",
    "main": "index.php"
}
```
