# Ring-web

Ring-web uses the Ring daemon ([dring](https://gerrit-ring.savoirfairelinux.com/#/q/status:open)) which is written in C++.

## State of things

1. ~~Our mission is to create support for web using the browser directly.~~
2. We are oriented into creating a RESTful API closely bound to the daemon to allow simple communication.
3. Selection of the micro-framework.

## 1. ~~Control delegated to the browser~~

This approach is user-friendly because it allows anyone to install it directly as an Add-on that can be used to control the daemon. For this there are a few options: 

### Executing the C++ binary

This part is under [Add-ons/Extensions/](Add-ons/Extensions/). Firefox dropped the support for its engine XPCOM. For more information, see its [Readme](Add-ons/Extensions/XPCOM/README.md). Nowadays, Firefox focuses on the WebExtensions which are essentialy compatible with Chrome and Opera. They are focusing into providing a native Web Extension instead of an Extension. Chrome does allow Extensions using NaCL: see an [example](Add-ons/Extensions/Chrome-NaCl/) but they are focusing more on the Chrome Apps.

### Compiling the daemon into Javascript

See [visualization](Resources/Images/c_to_js.png).

#### Why not Emscripten?

For those who don't know [Emscripten](https://github.com/kripken/emscripten), it's a really nice [LLVM](https://en.wikipedia.org/wiki/LLVM) to Javascript compiler. So basically, it will be able to generate LLVM bytecode from C/C++ and compiles it into Javascript. 

Useful to compile the daemon into JS, isn't it? Unfortunately, the compiler still has trouble with the [long long type](http://stackoverflow.com/questions/18971732/what-is-the-difference-between-long-long-long-long-int-and-long-long-i) in C++, so we were unable to compile all the ring-daemon dependancies (specially [GMP Library](https://gmplib.org)). But give it time, and maybe some day we'll be able to compile it.

See [Readme](Add-ons/WebExtensions/Emscripten/README.md).

## 2. RESTful Server

At the moment, we are going with the second option of delegating the control to the daemon.

### ~~Wrapping the Daemon~~

#### Go

See Go [Readme](Server/Go/README.md) and [example](Server/Go/wrapper).

#### NodeJS

The base of the server has been written but the binding has not been implemented.

See [example](Server/Nodejs/ring-demo/).

### Delegating control to the Daemon using shared library

#### Python

The pro is the maturity and the variety of the possible methods. In particular, the Python / C API makes it possible to embed a Python interpreter inside C++ using a standard and well documented interface.

The con *may* be the speed of the interlanguage framework.

For more information, consult the [README.md](Server/Python/README.md).

#### Go

The pros of this option are that it is fast to implement (has a build-in http server), easy to maintain, scalable and supports concurrency.

The main part of the work would be to create mapping from C++ uncommon Objects that are not supported by built-in SWIG support. Although, there is an [excellent example](https://github.com/savoirfairelinux/ring-client-android/blob/master/ring-android/app/src/main/jni/jni_interface.i) of how this can be done in ring-client-android. 

See [Readme](Server/Go/README.md) and [example](Server/Go/dynamic-lib/ring-demo/).

#### C / C++

The pro is that it is directly compatible with C++.

@TODO con(s)

##### Writing from scratch using Bootstrap

Follow the [Work in Progress](Server/Cpp/Bootstrap/).

##### Using existing libraries

Library | C / C++ | GPLv3 Compatible | Cross-platform | Complexity | Size | Latest Release
---|---|---|---|---|---|---
[Restbed](https://github.com/Corvusoft/restbed) | C++ | Yes | Yes | Average | Large | 4.0 : 28-05-2016 
~~[Crow](https://github.com/ipkn/crow)~~ | C++ | Yes | Yes | Easy | Short | Last commit in march
~~[cpprestsdk](https://github.com/Microsoft/cpprestsdk)~~ | C++ | Yes | Yes | Hard | Huge | 2.8.0 : 25-02-2016
[cpp-netlib](https://github.com/cpp-netlib/cpp-netlib) | C++ | Yes | Yes | Medium | Medium | 0.12.0 : 30-03-2016
~~[dyad](https://github.com/rxi/dyad)~~ | C | Yes | Yes | Easy | Light | Last commit 9 month ago

###### Restbed

The release 3.5 and the most recent one 4.0 have a dependency *asio* that needs a [patch](https://bugs.archlinux.org/task/48620#comment145230) to counter the restbed build fail caused by:

    error: ‘::SSLv3_server_method’ has not been declared

###### Crow

It's a C++ [Header-only](https://en.wikipedia.org/wiki/Header-only) Microframework for Web, inspired by Python Flask. See the [debate](https://news.ycombinator.com/item?id=8002604) on its usability / performance.

## 3. Selection of the micro-framework

### Criteria

1. Code readability
2. Size of the framework
3. Documentation and community
4. Scalability and modularity
5. Performance Benchmark (Optional)

### Overview

@TODO

#### Micro-framework Name

1. ...
2. ...
3. ...
4. ...

A paragraph of impression.

## License

The code is licensed under a GNU General Public License [GPLv3](http://www.gnu.org/licenses/gpl.html).

## Authors

Simon Zeni simon.zeni@savoirfairelinux.com

Seva Ivanov mail@sevaivanov.com

