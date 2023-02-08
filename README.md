实现一个脚手架，通过命令可以自动创建项目。

```shell
v create demo
```

### 插件介绍

#### commander

文档地址：https://www.npmjs.com/package/figlet

说明：命令行插件

使用方法：

```js
let program = require("commander")
program.version("1.1.0")
program
    .option('-n --name <type>', 'output name')
    .command("create <app-name>")
    .description("创建项目")
    .action(name => {
        // do something
  			console.log("输入命令后回车，会执行此处代码")
    })

program.parse(process.argv);
```

#### figlet

文档地址：https://www.npmjs.com/package/figlet

说明：大型字符 - 终端打印大型文字

使用方法：

```js
var figlet = require('figlet');
 
figlet('Hello World!!', function(err, data) {
    if (err) {
        console.log('Something went wrong...');
        console.dir(err);
        return;
    }
    console.log(data)
});

// 打印结果
 _   _      _ _        __        __         _     _ _ _
 | | | | ___| | | ___   \ \      / /__  _ __| | __| | | |
 | |_| |/ _ \ | |/ _ \   \ \ /\ / / _ \| '__| |/ _` | | |
 |  _  |  __/ | | (_) |   \ V  V / (_) | |  | | (_| |_|_|
 |_| |_|\___|_|_|\___/     \_/\_/ \___/|_|  |_|\__,_(_|_)
```

#### chalk

文档地址：https://www.npmjs.com/package/chalk

说明：彩色文字 - 美化终端字符显示

使用方法：

```js
const chalk = require('chalk');
const log = console.log;

// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));
```

#### inquirer

文档地址：https://www.npmjs.com/package/inquirer

说明：命令行参数输入交互

使用方法：

```js
var inquirer = require('inquirer');
inquirer
  .prompt([
    /* Pass your questions in here */
  ])
  .then((answers) => {
    // Use user feedback for... whatever!!
  })
  .catch((error) => {
    if (error.isTtyError) {
      // Prompt couldn't be rendered in the current environment
    } else {
      // Something else went wrong
    }
  });
```

#### shelljs

文档地址：https://www.npmjs.com/package/shelljs

说明：命令行操作

使用方法：

```js
var shell = require('shelljs');
// Copy files to release dir
shell.rm('-rf', 'out/Release');
shell.cp('-R', 'stuff/', 'out/Release');
```

#### ora

文档地址：https://www.npmjs.com/package/ora

说明：loading效果

使用方法：

```js
const ora = require('ora');

const spinner = ora('Loading unicorns').start();

setTimeout(() => {
	spinner.color = 'yellow';
	spinner.text = 'Loading rainbows';
}, 1000);
```

#### download-git-repo

文档地址：https://www.npmjs.com/package/download-git-repo

说明：仓库下载

使用方法：

```js
download('https://mygitlab.com:flippidippi/download-git-repo-fixture#my-branch', 'test/tmp', { clone: true }, function (err) {
  console.log(err ? 'Error' : 'Success')
})
```

### 脚手架开发流程

1. 创建空项目/文件夹

2. 通过npm/yarn 初始化package文件

   ```shell
   npm init 或者 yarn init 
   ```

3. 安装插件

   ```shell
   yarn add figlet chalk inquirer shelljs ora download-git-repo commander -S
   // OR 
   npm install figlet chalk inquirer shelljs ora download-git-repo commander -S
   ```

4. 创建bin目录

5. 开发命令行

   ```shell
   /**
    * 开发后台脚手架，快速生成标准架构
    */
   let program = require("commander")
   program.version("1.1.0")
   
   program
       .option('-n --name <type>', 'output username')
   
   program
       .command("create <app-name>")
       .description("创建项目")
       .action(name => {
           // do something
       })
   
   program.parse(process.argv);
   ```

6. 开发下载功能

   ```shell
   /**
    * 模板克隆
    */
   let { promisify } = require("util")
   // 彩色文字
   let chalk = require("chalk")
   // loading效果
   const ora = require('ora')
   // 仓库下载
   const download = promisify(require('download-git-repo'))
   const shell = require("shelljs")
   
   const log = content => console.log(chalk.yellow(content))
   module.exports = async (name) => {
       log(`🚀 创建项目 ${name}`)
       shell.rm("-rf", name);
       try {
           const spinner = ora(`下载中.....`).start()
           await download("direct:https://git.imooc.com/coding-502/manager-fe.git", name, { clone: true })
           spinner.succeed("下载完成")
       } catch (error) {
           console.log("下载失败", error)
       }
   }
   ```

备注：如果要下载分支，则直接再git地址后用#好拼接，比如：

```shell
download("direct:https://git.imooc.com/coding-502/manager-fe.git#2-5request封装", name, { clone: true })
```