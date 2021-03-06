<!-- YAML
added: v0.7.10
-->

* {Array}

一个到子进程的管道的稀疏数组，对应着传给 [`child_process.spawn()`] 的选项中值被设为 `'pipe'` 的 [`stdio`]。
注意，`child.stdio[0]`、`child.stdio[1]` 和 `child.stdio[2]` 分别可用作 `child.stdin`、 `child.stdout` 和 `child.stderr`。

在下面的例子中，只有子进程的 fd `1`（stdout）被配置为一个管道，所以只有父进程的 `child.stdio[1]` 是一个流，数组中的其他值都是 `null`。

```js
const assert = require('assert');
const fs = require('fs');
const child_process = require('child_process');

const child = child_process.spawn('ls', {
  stdio: [
    0, // 使用父进程的 stdin 用于子进程
    'pipe', // 把子进程的 stdout 通过管道传到父进程 
    fs.openSync('err.out', 'w') // 把子进程的 stderr 指向一个文件
  ]
});

assert.strictEqual(child.stdio[0], null);
assert.strictEqual(child.stdio[0], child.stdin);

assert(child.stdout);
assert.strictEqual(child.stdio[1], child.stdout);

assert.strictEqual(child.stdio[2], null);
assert.strictEqual(child.stdio[2], child.stderr);
```

