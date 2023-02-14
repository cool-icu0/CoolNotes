！！！编程中的同步与异步与生活中的感觉是相反的
编程同步是一件事情做完再做另一件事情，异步是同时开始做几件事情（结合下面的例子理解）

readFile()的基础用法

readFile() // 异步读取文件

可传入3个参数

第一个参数：文件的路径（不可省略）

第二个参数：编码方式（可省略，省略后读出的文件就是buffer数据格式）

第三个参数：（不可省略）
一个函数，表示读取文件之后要做的事情；
这个函数可传入两个参数，第一个代表读取失败时的错误，第二个代表读取成功时读到的数据；

```
const fs = require('fs');

fs.readFile('../../static/txt/a.txt', 'utf-8', (err, data) => {
    console.log(data);
});

```

**readFileSync()的基础用法**

readFileSync() // 同步读取文件

可传入两个参数

第一个参数代表文件路径（不可省略）

第二个参数代表读出文件的编码方式（可省略，省略后读出的数据是buffer数据格式）

```
const fs = require('fs');

let res = fs.readFileSync('../../static/txt/a.txt', 'utf-8');

console.log(res);
```

**readFile()异步**

```
const fs = require('fs');

fs.readFile('../../static/txt/a.txt', 'utf-8', (err, data) => {
    console.log(data);
});

fs.readFile('../../static/txt/b.txt', 'utf-8', (err, data) => {
    console.log(data);
});

fs.readFile('../../static/txt/c.txt', 'utf-8', (err, data) => {
    console.log(data);
});
```

由于readFile是异步读取文件，所以就相当于读取三个文件并打印的操作在同时进行，受延迟等影响，每次执行打印的顺序不固定。
如：
111111111111 （a.txt的内容）
333333333333333 （c.txt的内容）
22222222222222 （b.txt的内容）
或：
111111111111
22222222222222
333333333333333

**readFileSync()同步**

```
const fs = require('fs');

let resA = fs.readFileSync('../../static/txt/a.txt', 'utf-8');
console.log(resA);

let resB = fs.readFileSync('../../static/txt/b.txt', 'utf-8');
console.log(resB);

let resC = fs.readFileSync('../../static/txt/c.txt', 'utf-8');
console.log(resC);
```

由于readFileSync()是同步读取，所以按照顺序读取并打印a.txt、b.txt、c.txt，上一个文件未读取时不会读取下一个文件（做完一件事再做另一件事）。
所以无论执行多少次结果都是：
111111111111
22222222222222
333333333333333