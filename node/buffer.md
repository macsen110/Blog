# Buffer
Prior to the introduction of TypedArray in ECMAScript 2015 (ES6), the JavaScript language had no mechanism for reading or manipulating streams of binary data. The Buffer class was introduced as part of the Node.js API to make it possible to interact with octet streams in the context of things like TCP streams and file system operations.

Now that TypedArray has been added in ES6, the Buffer class implements the Uint8Array API in a manner that is more optimized and suitable for Node.js' use cases.

Instances of the Buffer class are similar to arrays of integers but correspond to fixed-sized, raw memory allocations outside the V8 heap. The size of the Buffer is established when it is created and cannot be resized.

The Buffer class is a global within Node.js, making it unlikely that one would need to ever use require('buffer').Buffer.

buffer
为数据缓冲对象，是一个类似数组结构的对象，可以通过指定开始写入的位置及写入的数据长度，往其中写入二进制数据
stream
是对buffer对象的高级封装，其操作的底层还是buffer对象，stream可以设置为可读、可写，或者即可读也可写，在nodejs中继承了EventEmitter接口，可以监听读入、写入的过程。具体实现有文件流，httpresponse等