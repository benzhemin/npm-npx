# npm-npx

#### [Introducing npx: an npm package runner](https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner)
#### [Difference between npx and npm](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)

### `NPM` - *Manages* packages *but* doesn't make life easy *executing* any.`NPX` - A tool for *executing* Node packages.

> NPX comes bundled with NPM version 5.2+
> 

`NPM` by itself does not simply run any package. it doesn't run any package in a matter of fact. If you want to run a package using NPM, you must specify that package in your `package.json` file.

When executables are installed via NPM packages, NPM links to them:

1. *local* installs have "links" created at `./node_modules/.bin/` directory.
2. *global* installs have "links" created from the global `bin/` directory (e.g. `/usr/local/bin`) on Linux or at `%AppData%/npm` on Windows.

It doesn't matter whether you installed that package globally or locally. **NPX will temporarily install it and run it. NPM also can run packages if you configure a package.json file and include it in the script section.**

`npx` runs a command of a package without installing it explicitly.

Use cases:

- **You don't want to install packages neither globally nor locally.**
- You don't have permission to install it globally.
- Just want to test some commands.
- Sometime, you want to have a script command (generate, convert something, ...) in `package.json` to execute something without installing these packages as project's dependencies.


package.json 中定义的bin, 安装package后会在./node_modules/.bin 生成动态链接. 本地安装会在当前文件夹, 全局安装会在NODE_PATH中.

如果要执行安装的script, 有两种方式:

1. `$ ./node_modules/.bin/your-package`
2. 将命令定义在package.json的scripts中比如, 执行script的时候, npm会自动将./node_modules/bin加入到环境变量中.

```json
{
  "name": "your-application",
  "version": "1.0.0",
  "scripts": {
    "your-package": "your-package"
  }
}
```

for example:

```json
{
  "name": "hello",
  "bin": {
    "hello1": "hello"
  },
  "scripts": {
    "start": "/bin/bash -c 'echo $PATH'"
  }
}
```

npm run start, 打印出来的环境变量

```
~/Desktop/temp/link/node_modules/.bin:
~/Desktop/temp/node_modules/.bin:
~/Desktop/node_modules/.bin:
~/Desktop/node_modules/.bin:
/Users/node_modules/.bin:
/node_modules/.bin:
~/.nvm/versions/node/v16.13.0/lib/node_modules/npm/node_modules/@npmcli/run-script/lib/node-gyp-bin:
~/.nvm/versions/node/v16.13.0/bin:
```

可以看到一级一级的将/node_modules/.bin加入到环境变量中

`**NPX` - A tool for *executing* Node packages.**

npx run locally installed package

```
npx installed-package
```

npx 会去查看 <command>是否在$PATH中存在, npx运行添加的环境变量, 同npm run相似

```
~/Desktop/temp/npx/node_modules/.bin:
~/Desktop/temp/node_modules/.bin:
~/Desktop/node_modules/.bin:
~/node_modules/.bin:
/Users/node_modules/.bin:
/node_modules/.bin:
~/.nvm/versions/node/v16.13.0/lib/node_modules/npm/node_modules/@npmcli/run-script/lib/node-gyp-bin:
~/Desktop/temp/npx/node_modules/.bin:
~/.nvm/versions/node/v16.13.0/bin:
```

mkdir npx

npm link ../link

查看/node_modules/.bin下对应的命令
