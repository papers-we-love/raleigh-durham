# The Little Manual of API Design

[Jasmin Blanchette](https://people.mpi-inf.mpg.de/~jblanche/)

# Table of Contents
  1. [Introduction](#introduction)
  2. [Characteristics of Good APIs](#characteristics-of-good-apis)
    1. [Easy to learn and memorize](#easy-to-learn-and-memorize)
    2. [Leads to readable code](#leads-to-readable-code)
    3. [Hard to misuse](#hard-to-misuse)
    4. [Easy to extend](#easy-to-extend)
    5. [Complete](#complete)
  3. [The Design Process](#the-design-process)
    1. [Know the requirements](#know-the-requirements)
    2. [Write use cases before you write any other code](#write-use-cases-before-you-write-any-other-code)
    3. [Look for similar APIs in the same library](#look-for-similar-apis-in-the-same-library)
    4. [Define the API before you implement it](#define-the-api-before-you-implement-it)
    5. [Have your peers review your API](#have-your-peers-review-your-api)
    6. [Write several examples against the API](#write-several-examples-against-the-api)
    7. [Prepare for extensions](#prepare-for-extensions)
    8. [Don’t publish internal APIs without review](#don\'-t-publish-internal-apis-without-review)
    9. [When in doubt, leave it out](#when-in-doubt-\,-leave-it-out)

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

### Leads to readable code

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

### Hard to Misuse

> A well-designed API makes it easier to write correct code than incorrect code, and encourages good programming practices.
It does not needlessly force the user to call methods in a strict order or to be aware of implicit side effects or semantic oddities.

Make an API hard to misuse by eliminating redundancy. For example, an addItem(Item) method that lets the user write.
```javascript
const mySet = new Set();
mySet.add(1);
mySet.add(5);
mySet.add('some text');
mySet.keys() // SetIterator {1, 5, "some text"}
```

Above is some new ES2015 syntax in JavaScript for creating Sets.
The Set object lets you store unique values of any type, whether primitive values or object references.

As the paper states you want to design an api that is hard to use and in this case a set is good in order to have unique values and avoid duplicate values which can be problematic at times. 

### Easy to extend

APIs should be easy to extend as new classes/function get added over time. Parameters get added/removed from function signatures.

```javascript
'use strict';

const {join} = require('path');
const {readFileSync} = require('fs');

const winston = require('winston');
const jwt = require('jsonwebtoken');

const {responseCodes} = require('../constants');

const PATH = '/api/v1/';

const createToken = (req, res, next) => {
    // sign with RSA SHA256
  const cert = readFileSync(join(__dirname, '../ca/ca.key'));

  const {name} = req.body;

    // sign asynchronously
  jwt.sign({ name }, cert, { algorithm: 'RS256' }, (err, token) => {
    if (err) {
      winston.log('error', 'Error Creating json web token', {err});
      res.send(err);
    }
    res.send(responseCodes['created'], {
      adminToken: token
    });
    return next();
  });
};


module.exports = (app) => {
  app.post(`${PATH}/createToken`, createToken);
};
```


Here I have an endpoint to sign a jwt with an expiration date which returns the following payload.

```json
{
  "adminToken": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.
                 eyJuYW1lIjoiYWRtaW5Ub2tlbiIsImlhdCI6MTQ4NjYwNDU3OX0.
                 Pg7HOKIwn6HietMotYbwIvSGqvOcoUSNegQXW_BN-C5rQu9ZXyJnz6iK-L2JzLqlzAApuB1ria5TcN0HkrZQ3aBrIflvjv96W5M_51GEwXxpQ1wTSA-T6ZRBzanq7b_RRhB_TEsezf7hf87U-N6nWnV9EKo1LAf_fAo53-7mXTVAJyG39n2NxJfkJ9btvcq8BLZN68AF-WFpsPTtujQV2kA9sI9ApeTdh_7qgLTkm8Iup7rwDrtnA2TNidgqjnMSgKsHYdtvc1_guxp6LJ2lVeF0XgmMVqw6-EFfa4w15azEF-bJYHn23f_GobpRZsR7_YvWLN5dt1NU56JT2Nip3Q"
}
```

I can easily extend the api to do more things with jwt's by just adding HTTP methods to module.exports

### Complete

* Ideally an api would be complete and your users can do everything that they want to do.
* Realistically this does not always happen but if you can provide a way for your users to extend the api or customize it then it can help an API be complete.
* API completeness is something that can occur incrementally over time through extension as mentioned above.

## The Design Process

### Know the requirements
* Before setting out to design an API, you should have a clear understanding of the requirements.
* Usually you will have to do some requirements analysis
  1. Ask your collegues, users, your boss etc. in order to get a better picture

### Write use cases before you write any other code

* Avoid diving into implementation from the very start of api design.
  1. APIs designed in this way don't take users of your API into account
  2. The API Implementation should adapt to the users oof you API.
* Write some code snippets based on your requirements analysis.
* I would suggest writing some unit tests though the author doesn't explicitly mention this.
* Using the principles of TDD your API will take shape
  1. Add a test
  2. Run all tests and see if the new test fails
  3. Write the code
  4. Run tests
  5. Refactor code

### Look for similar APIs in the same library

* Author argues that similar APIs have the added benefit that users familiar with one method/class can easily learn another method/class if they are similar.
* Author adds the caveat that you shouldn't blindly follow an already existing API but instead look for improvements and/or fix bad APIs and then mimic then elsewhere.
* If you are writing a newer version of the API, then you should the API you are replacing very well, otherwise you run the risk of introducing new design flaws for old flaws

### Define the API before you implement it

* For a library with thousands of users, it is much better if the implementation is tricky and the API is straightforward than the other way around.
* APIs will typically outlast their implementations (e.g., UNIX/POSIX, OpenGL).
* As you implement the API or write unit tests for your implementation, you will most probably find flaws or undefined corner cases in your original design.
  1. I would argue here that if you are following Test-Driven Development that you can find corner cases possibly earlier and avoid abstractions from leaking out into your api.

### Have your peers review your API

1. Look for feedback.
2. Ask for feedback.
3. Beg for feedback.
4. Show your APIs to your peers, and collect all the feedback you get. 
5. Try to momentarily forget how much work it would be to implement the requested changes.
6. The more facts you possess, the better thechances that you will design a good API.

Some thoughts about this that I have:

* Open a pull request in whatever version control system that you like (eg. Github, Gitlab, Bitbucket, etc)
  1. Add reviewers if you can and ask for people's thoughts on what you worked.
  2. Depending on what you are working can really save you from costly design decisions in the future.
  3. One thing I liked about how Jose Valim (Creator of Elixir) is how collaborative he has been with the Elixir Programming Language by seeking out advice from others.
  4. I believe more feedback can help avoid mistakes in the future.

### Write several examples against the API

* After you designed an API, you should write a few examples that use the API.
* Often, you can obtain examples simply by fleshing out the use cases defined earlier
  1. Once again I typically use the unit tests that I write for my classes/functions as later examples.

  **Ask others to write unit tests of your examples and an added bonus is they can describe any roadblocks they ran into.**

### Prepare for extensions

Expect your API to be extended in two ways:

1. By the maintainers of the API, who will add to it (and occasionally deprecate parts of the API).
2. By users, who will write subclasses to customize the behavior of its components
    1. In a language like you JavaScript you can somewhat achieve this by making `BaseClass.prototype` is copied into `ChildClass.prototype`
    2. Although Kyle Simpson has an interesting take on this for JavaScript coining the term Object linked with other Objects (OLOO).
    3. Essentially you delegate methods back to the original objects
    4. Example delegating a method in an array back to the `Array.prototype`, it walks up the prototype chain to find the used method.

### Don't publish internal APIs without review

Author points out that you should carefully review internal APIs before releasing them out into the public 
because once your APIs are public it becomes more difficult to update bad method names once people are using them.

*I would also add here that if you have a solid code review process you can get feedback like this early on, 
especially if you make any code changes via a pull request and add multiple reviewers.*

### When in doubt, leave it out

* If you are in doubt about adding any functionality in your API, then leave it out, mark it as internal and reconsider at a later time.
* Wait for feedback from users.
  1. Author makes good point that you can't always add every feature that users want.
  2. Author suggest a rule of thumb to wait for 3 independent request for the same feature before implementing it.