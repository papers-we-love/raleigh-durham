# The Little Manual of API Design

[Jasmin Blanchette](https://people.mpi-inf.mpg.de/~jblanche/)

# Table of Contents
  1. [Introduction](#introduction)
  2. [Characteristics of Good APIs](#characteristics-of-good-apis)
    1. [Easy to learn and memorize](#easy-to-learn-and-memorize)
    2. [Leads to readable code](#leads-to-readable-code)

## Introduction

> An application programming interface, or API, is the set of symbols that are exported and available to the users of a library to write their applications.

[Daniel Jackson](http://people.csail.mit.edu/dnj/)

> Software is built on abstractions. Pick the right ones, and pro-gramming will flow naturally from design; 
modules will havesmall and simple interfaces; and new functionality will more likely fit in without extensive reorganization.
Pick the wrongones, and programming will be a series of nasty surprises.

## Characteristics of Good APIs

1. Easy to learn and memorize
2. Leads to readable code
3. Hard to misuse
4. Easy to extend
5. Complete

API authors should strive for "minimal" and "consistent" APIs only to the extent that it helps the list above.

APIs should be consistent in the sense that similar concepts should be named the same while different concepts should have different names

### Easy to learn and memorize

Implementation of `md5` function
```javascript
function md5({ str, encoding = 'hex' }) {
  return crypto
    .createHash('md5')
    .update(str)
    .digest(encoding);
} 
```

Unit test for `md5` function
```javascript
test('mdf5 should compute challenge when given ha2, nonce, cnonce, and qop', assert => {
  const md5 = require('../../utils/md5').md5;
  const ha2 = md5({
    str: 'GET:/api/v1/digestScheme'
  });
  const actual = md5({
    str: `${ha2}:${process.env.NONCE}:${process.env.NONCE}:auth`
  });
  const expected = 'adc91a91ffaa68815d5a5d8e4ed8d9e9';
  assert.is(actual, expected, `should return ${expected}`);
});
```

Notice that the `md5` function takes an object with a property of `str` and a string value.

I named this function in my api `md5` because it computes a hash value when given a string value.

* A minimal API is easy to memorize because there is little to remember.
* A consistent API is easy to memorize because you can reapply what you learned in one part of the API when using a different part.
* An API is not only the names of the classes and methods that compose it, but also their intended semantics.
* An easy-to-learn API makes it possible to write the “hello world” example in just a few easy lines of code and to expand it incrementally to obtain more complex programs.

> An easy-to-learn API features consistent naming conventions and patterns,economy of concepts, and predictability. It uses the same name for the sameconcept, and different names for different concepts.

[MD5](https://en.wikipedia.org/wiki/MD5)
> The MD5 algorithm is a widely used hash function producing a 128-bit hash value. Although MD5 was initially designed to be used as a cryptographic hash function, it has been found to suffer from extensive vulnerabilities. It can still be used as a checksum to verify data integrity, but only against unintentional corruption.

## Leads to readable code

* Readable code is easier to document and maintain.
* Readable code is always at the right level of abstraction.
    * it neither hides important things nor forces the programmer to specify irrelevant information


Implement decode unsigned JSON web token function
```javascript
function decodeUnsignedJWT(jwt) {
  const [
    headerB64,
    payloadB64
   ] = jwt.split('.');
  const headerStr = new Buffer(headerB64, 'base64').toString();
  const payloadStr = new Buffer(payloadB64, 'base64').toString();
  return {
    header: JSON.parse(headerStr),
    payload: JSON.parse(payloadStr)
  };
}
```

Unit test for decodeUnsignedJWT function
```javascript
test('decode should return base64 decoded string', assert => {
  const header = {
    alg: 'HS256'
  };

  const payload = {
    name: 'John Rambo',
    rank: 'Sergeant',
    branch: 'Army'
  };

  const encodeUnsignedJWT = require('../../utils/encode').encodeUnsignedJWT;
  const encoded = encodeUnsignedJWT({
    header,
    payload
  });

  const decodeUnsignedJWT = require('../../utils/decode').decodeUnsignedJWT;
  const actual = decodeUnsignedJWT(encoded);
  const expected = {
    header: '{"alg":"HS256"}',
    payload: '{"name":"John Rambo","rank":"Sergeant","branch":"Army"}'
  };
  assert.deepEqual(actual, expected, `should return ${expected}`);
});
```