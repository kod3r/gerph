# Gerph [![standard-readme compliant](https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)

> A simple key-value networked configuration store written in Go.

Based on the [bolt](https://github.com/boltdb/bolt) key/value database and the [goat](https://github.com/bahlo/goat) API framework.

## Table of Contents

- [Install](#install)
	- [Packaged releases](#packaged-releases)
	- [From source](#from-source)
- [Usage](#usage)
    - [CLI](#cli)
	- [API](#api)
- [Examples](#examples)
- [Known issues](#known-issues)
- [Contribute](#contribute)
- [License](#license)

## Install

### Packaged releases

Check out [the releases section](https://github.com/nmaggioni/gerph/releases) for ready-to-run binaries,. [Here's the latest one!](https://github.com/nmaggioni/gerph/releases/latest)

### From source

Given that your `$PATH` already has `$GOPATH/bin` in it, get the package and install it:

```
$ go install github.com/nmaggioni/gerph
```

## Usage

### CLI

Gerph has some useful command line options, you can check them by using the help flag:

```
$ gerph -h
```
### API

Being based on [bolt](https://github.com/boltdb/bolt), Gerph has the concept of _buckets_: they are containers of key/value pairs.

**All endpoints answer in JSON format.** The following table summarizes the available routes and their parameters, expressed in the form of _`:parameter`_ :

| HTTP Method | Path | Explanation |
|-------------|------|-------------|
| OPTIONS | / | Will show a list of all the endpoints grouped by method. |
| GET | / | Will reply with a listing of all the buckets and their keys. |
| GET | /:bucket | Will show all the keys inside the specified bucket. |
| DELETE | /:bucket | Will delete the specified bucket and its keys. |
| GET | /:bucket/:key | Will reply with the content of the specified key of the specified bucket. |
| PUT or POST | /:bucket/:key | Will set the content of the specified key of the specified bucket to the given content. Content must be sent in `x-www-form-urlencoded` encoding (`Content-Type` header set to `application/x-www-form-urlencoded`), and the value of the key must be set to the `value` field.|
| DELETE | /:bucket/:key | Will delete the specified key of the specified bucket. |

Each call can answer with one of these status codes:

+ `200` - OK
+ `204` - No Content
+ `400` - Bad Request
+ `500` - Internal Server Error

## Examples

### cURL
Setting a key:

```bash
curl -X PUT -d 'value=myValue' "http://localhost:3000/myBucket/myKey"
```

Getting a key:

```bash
curl "http://localhost:3000/myBucket/myKey"
```

Getting all keys of a bucket:

```bash
curl "http://localhost:3000/myBucket"
```

Getting all keys of all buckets:

```bash
curl "http://localhost:3000"
```

## Known issues

+ **At the moment Gerph is only capable of storing strings as values.** If other types of data are in need of being stored, the effort of casting back and forth from strings to the actual data type falls on the developer. _Resolution of this problem will be part of the next iteration._

## Contribute

PRs gladly accepted! Basing them on a new feature/fix branch would help in reviewing.

## License

[MIT © Niccolò Maggioni](https://github.com/nmaggioni/gerph/blob/master/LICENSE)
