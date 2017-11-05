# 06 JSON Addons

In this sample we are going to play with `Node.js` addons.

Summary steps:

- Create `c++` file.
- Create a `gyp` file for build configuration.
- Install `node-gyp` to build it.

# Steps to build it

## Prerequisites

- In order to follow this step guides you will also need to take sample _05 Buffer_ as starting point.

# Steps

- Delete all files `index.js`, `bufferSlice.js` and `stringDecoder.js`:

### ./index.js

```diff
- const buffer = Buffer.alloc(8);
- console.log(buffer);

- const buffer2 = Buffer.allocUnsafe(8);
- console.log(buffer2);

- buffer2.fill();
- console.log(buffer2);

- const string = 'touché';
- const buffer3 = Buffer.from(string, 'ascii');

- console.log(string, string.toString(), string.length);
- console.log(buffer3, buffer3.toString(), string.length);

```

- If `Node.js` could not find a `.js` or `.json`file, it will look for a `.node` file and it would interpret hte file as a compiled addon module. We are going to implement the sample in [`Addons section`](https://nodejs.org/docs/latest-v6.x/api/addons.html).

- Create `c++` file:

### ./hello.cc
```c++
#include <node.h>

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void Method(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  args.GetReturnValue().Set(String::NewFromUtf8(isolate, "world"));
}

void init(Local<Object> exports) {
  NODE_SET_METHOD(exports, "hello", Method);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, init)

}

```

- Now, we need to create a `gyp` file for build configuration.

### ./binding.gyp

```gyp
{
  "targets": [
    {
      "target_name": "addon",
      "sources": [ "hello.cc" ]
    }
  ]
}

```

- This file is used by [`node-gyp`](https://github.com/nodejs/node-gyp) library to compile `Node.js` addons. So we need to install this lib:

```bash
npm install node-gyp -g
```

- Once installed, we could build the file using `node-gyp configure`:

```bash
node-gyp configure
```

# About Lemoncode

We are a team of long-term experienced freelance developers, established as a group in 2010.
We specialize in Front End technologies and .NET. [Click here](http://lemoncode.net/services/en/#en-home) to get more info about us.