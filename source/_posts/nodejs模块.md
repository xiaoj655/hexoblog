---
title: Nodejs模块
tags:
  - nodejs
categories:
  - twice week
---

# nodejs模块

---

### url:

```js
url.parse('https://www.baidu.com:443/path/index.html?id=2#tag=3')
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~返回值
{
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com:443',
  port: '443',
  hostname: 'www.baidu.com',
  hash: '#tag=3',
  search: '?id=2',
  query: 'id=2',
  pathname: '/path/index.html',
  path: '/path/index.html?id=2',
  href: 'https://www.baidu.com:443/path/index.html?id=2#tag=3'
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
url.format('obj')  //与上一个函数是互逆的关系
url.resolve('[url]',path)  //返回url根路径下的path地址
URLSearchParams(url.parse('[url]').attribute)
```

### querystring

```js
const qs = require('querystring')
qs.parse(obj,[sep[,eq[,options]]])  //sep和eq分别用来指定键值对之前和键值之前的连接符
qs.stringfy(obj,[sep[,eq[,options]]])  //序列化为一串字符,与parse互逆
qs.escape(str)   //对str进行URL编码
qs.unescape(str)  // 与上互逆
//在大多数情况下，我们不需要手动使用 querystring.escape() 方法进行 URL 编码，因为 Node.js 在进行 HTTP 请求时会自动对 URL 进行编码。但在某些特殊情况下，例如需要手动构造 URL 参数时，可能需要使用 querystring.escape() 方法进行编码。
```

### Git安装网络上的项目

```js
npm install git+[https]
或
npm install git+ssh://[ssh]
```

### HTTP HTTPS

[http具体方法](https://www.notion.so/http-3417be76195e49a8aa54ed8919430132)

node的浏览器端调试

> node --inspect --inspect-brk a.js
> 

node进程管理工具

> supervisor
nodemon
forever
pm2
> 

```js
const http = require('http')

const server = http.createServer((request, response) => {
  if (request.method === 'POST') {
    let body = ''
    request.on('data', (chunk) => {
      body += chunk
    });
    request.on('end', () => {
      console.log(`Request body: ${body}`)
      response.writeHead(200, { 'Content-Type': 'text/plain' })  //撰写返回头
      response.end('Received request body')   //发送响应数据
    })
  } else {
    response.writeHead(200, { 'Content-Type': 'text/plain' })
    response.end('Hello, World!')  //用浏览器打开url地址就属于get请求
  }
})
server.listen(8080, () => {
  console.log('Server running on port 3000');
})
```

ps:为什么要 body += chunk

> 当客户端发送一个 POST 请求时，请求数据会被分成多个数据块（chunk）发送到服务器。`request.on('data', callback)`
 方法会在每个数据块到达服务器时触发回调函数，并将数据块作为参数传递给回调函数。这个回调函数（上述代码中的箭头函数）会将数据块拼接到 `body`
 变量中。
> 

```js
//http.get()

const http = require('http');

// 发送 GET 请求
http.get('http://example.com', (response) => {
  // 处理服务器的响应数据
  let data = ''
  response.on('data', (chunk) => {
    data += chunk
  })
  response.on('end', () => {
    console.log(data)
  })
}).on('error', (error) => {
  console.error(error)
})
```

```js
//http.post()

const http = require('http')

const option ={
  hostname: 'www.example.com',
  port: 80,
  path: '/submit',
  method: 'POST',
  headers: {
	   'Content-Type': 'application/json'
  }
const res = http.request(option,(res)=>{
		/* res 是一个 http.IncomingMessage 对象，表示从服务器接收到的响应。这个对象实现了一个可读流，可以通过它读取来自服务器的响应数据。res.statusCode: 响应状态码。
res.headers: 响应头对象。
res.setEncoding(encoding): 设置响应数据的编码格式。
res.on('data', callback): 注册一个回调函数，用于处理响应数据。*/
})

res.write(data) // 此data为向服务器发送的数据
res.end()       //用以告诉服务器数据发完了

```

### events

> `events`模块是一个非常重要的模块，它提供了一种简单而有效的方式来实现事件驱动的编程。事件驱动编程（Event-driven programming）是一种编程范式，它的核心思想是将程序的执行流程与事件的发生和处理分离开来，程序会在事件发生时进行相应的处理，而不是在固定的时间点执行某些操作。
> 

```js
const EventEmitter = require('events');

// 创建一个事件触发器实例
const emitter = new EventEmitter();

// 绑定事件处理函数
emitter.on('myEvent', (data) => {
  console.log('Received data:', data);
});

// 触发事件
emitter.emit('myEvent', { message: 'Hello, world!' });
```

### File System

> 在 Node.js 中，文件系统（File System）是一个核心模块，它提供了一组 API，用于操作文件和目录。通过文件系统模块，我们可以读取和写入文件，创建和删除目录，以及执行一些其他的文件系统操作。
> 

```jsx
const fs = require('fs')
const fsPrimises = require('fs').promises

fs.mkdir('logs',(err)=>{ 
    if(err) throw err
    console.log('mkdir success!')
})

fs.rename('./logs','./log',()=>{
    console.log('rename dir success!')
})

fs.rmdir('./logs',()=>{
    console.log('done')
})

fs.writeFile('./logs/log1.log','hello\nworld!', (err)=>{
    console.log('done')
})

fs.appendFile('./logs/log1.log','\n!!!!',(err)=>{
    console.log('done')
})

fs.unlink('./logs/log1.log',(err)=>{
    console.log('done')
})

fs.readFile('./logs/log1.log',(err,content)=>{
    console.log(content.toString())
})

const content = fs.readFileSync('./logs/log1.log')

console.log(content.toString())
console.log('continue...')

;(async() =>{      //使用fsPromises模块的异步读取文件内容写法,优点就是返回promise类型
    let result = await fsPrimises.readFile('./logs/log1.log')
    console.log(result.toString())
})()

function readDir(dir){
fs.readdir(dir,(err,content)=>{
    console.log(content)
    content.forEach((value,index) => {
        
        fs.stat(`${dir}/${value}`,(err,status)=> {
            // console.log(content.isDirectory())
            if(status.isDirectory()){
                readDir(value)
            }else{
                fs.readFile(`${dir}/${value}`,'utf-8',(err,content)=>{
                    console.log('~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~')
                    console.log(content)
                    
                })
            }
        })
    })
})
}

fs.watch('./logs/log0.log',(err,content)=>{
    console.log('file has changed.')
})
```

### 流与zlib

> 
> 

```jsx
const fs = require('fs')
const zlib = require('zlib')

const gzip = zlib.createGzip()  //用于创建一个gzip压缩算法的可写流。可以通过这个可写流将数据压缩成gzip格式。

const readStream = fs.createReadStream('./logs.txt')
const writeStream = fs.createWriteStream('./logs.gzip')

readStream
    .pipe(gzip)
    .pipe(writeStream)
```

### readline

```js
const readline = require('readline');

const rl = readline.createInterface({   //固定写法
  input: process.stdin,
  output: process.stdout
});

rl.on('line', (input) => {  //听line事件来逐行读取输入流中的数据
  console.log(`Received: ${input}`);
});

rl.on('close', () => {   //当输入流结束时，会触发close事件
  console.log('Input stream closed.');
});
```

### Crypto

> nodejs提供的加密模块
> 

```js
//哈希加密
const crypto = require('crypto')

const password = 'abc12'

const hash = crypto
    .createHash('sha1')
    .update(password,'utf-8')
    .digest('hex')            //以16进制表示

console.log(hash)            //8fe670fef2b8c74ef8987cdfccdb32e96ad4f9a2
```

| 对称加密 | 使用相同的密钥进行加密和解密，常用的算法有AES、DES、3DES等。 |
| --- | --- |
| 非对称加密 | 使用公钥进行加密，使用私钥进行解密，常用的算法有RSA。 |
| 数字签名 | 使用私钥对数据进行签名，使用公钥进行验证，常用的算法有RSA、DSA等。 |
| 哈希函数 | 将任意长度的数据映射为固定长度的数据，常用的算法有MD5、SHA-1、SHA-256等。 |