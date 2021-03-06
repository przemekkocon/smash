<img src=assets/logo.png width=500 height=202 />

[![Build Status](https://travis-ci.org/appformation/smash.svg?branch=master)](https://travis-ci.org/appformation/smash)

Smash is Volley insipired networking library that's using [OkHttp][okhttp] in its core.

Usage
-----

Add Smash library to your project `build.gradle`
```groovy
dependencies
{
    compile 'pl.appformation:smash:0.1.0'
}
```

Instantiate one shared SmashQueue object (e.g. in your Application class):
```java
mSmashQueue = Smash.buildSmashQueue(getApplicationContext());
```

To request content of server as String do following:
```java
String url = "https://github.com/appformation/smash";
SmashStringRequest request = new SmashStringRequest(SmashRequest.Method.GET, url,
        new SmashResponse.SuccessListener<String>()
        {
            public void onResponse(String response)
            {
                handleMyNiftyResponse(response);
            }
        }, new SmashResponse.FailedListener()
        {
            public void onFailedResponse(SmashError error)
            {
                showImpendingDoomError(error);
            }
        });
```

Request object looks definitely cooler with [Retrolambda] library:
```java
SmashStringRequest request = new SmashStringRequest(SmashRequest.Method.GET, url,
    this::handleNiftyResponse, this::showImpendingDoomError);
```

Constructed request this way we now need to add to our SmashQueue:
```java
mSmashQueue.add(request);
```

Why another library?
--------------------

Some of us like Volley library, it's easy to use and has superb extensibility with Request class, but recent removal of HttpClient in Marshmalow made library obsolete. We also don't like reactive programming. With this in mind came idea to rewrite Volley to use directly all of the internals of [OkIo] and [OkHttp] libraries.


What's different than in Volley
-------------------------------

* URL address can be modified, up to the point of reaching dispatcher queue
* [Source] class from OkIo is used instead of InputStream
* Ability to modify User-Agent field globally for all requests
* Both listeners are obligatory in request constructor
* To prevent class names conflicting with OkHttp all smash classes have Smash prefix


Things to do
------------

* Retry policy (for now default from OkHttp is used)
* Authentication mechanism in requests
* Ability to easily add interceptors (e.g. Stetho)
* More baked in request classes (JSONArray, JSONObject, byte[])
* [Moshi] support (thanks to OkIo we can use Source class with Moshi)
* Support for multipart/form-data upload
* ... and last but not least, unit tests


Changelog
---------

[Current version is 0.1.0](CHANGELOG.md)


License
-------

    Copyright (C) 2015 Appformation Sp. z o.o.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

 [okhttp]: http://square.github.io/okhttp/
 [okio]: http://github.com/square/okio/
 [moshi]: http://github.com/square/moshi/
 [source]: https://square.github.io/okio/okio/Source.html
 [retrolambda]: https://github.com/orfjackal/retrolambda
