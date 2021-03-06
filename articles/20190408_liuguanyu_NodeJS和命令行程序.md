![](https://p2.ssl.qhimg.com/t012a23c1067ba6c75b.png)

> 造物无言却有情，每于寒尽觉春生。千红万紫安排著，只待新雷第一声。 —— 清.张维屏 《新雷》

### 源起

植根于Unix系统环境下的程序，很多都把贯彻Unix系统设计的哲学作为一种追求。Unix系统管道机制的发明者Douglas McIlroy把Unix哲学总结为三点：

1. 专注做一件事，并做到极致。
2. 程序协同工作。
3. 面向通用接口，如文本数据流。

随着Unix/Linux系统在服务器上影响力越发强大，以及各种跨平台解决方案的发展，这种哲学也被带到了各种平台上。若干年前，笔者第一次接触NodeJS和其包管理解决方案NPM时候，就感觉到其官方倡导的风格，和Unix系统哲学非常契合。近年来，随着NodeJS在服务端以及前端构建领域上的不断开拓，NodeJS的这种思想也正快速的渗透到这些领域。

其实，NodeJS的本身，也是开发命令行程序的一个重要利器。本文就将介绍几个常用的NodeJS相关命令行程序，之后介绍几个开发命令行中常用的组件，最后介绍发布npm包以及带scope的包的发布方法。


<!--more-->

### 命令行是如何工作的

命令行，可以简单定义为是一种基于文本流的用户交互接口和交互方式。命令行程序常常通过命令行参数的传递来得到不同的运行方式。而由于一切命令的下达，都是基于文本的，所以也为元编程，提供了便利。

命令行程序可以是编译执行的，也可以是解释执行的。对于编译后的命令行程序，将直接以机器码执行。而对于大多数的解释型的命令行程序，运行往往需要借助命令行解释程序。

这篇文章中提到的命令行程序特指需要解释程序的命令行程序。

可以充当命令行解释程序的，其实包含了大家听说过的常见的解释器，比如bash、zsh、perl、python、ruby、tcl等等，当然还有NodeJS。

打开一个命令行程序，比较标准的写法是在第一行写明解释程序的路径，如：

```bash
#!/usr/local/opt/python/bin/python3.6
```

这里 `#!` 成为shebang，一般位于文件的最开头。在Unix系统中，`#!`所在行后面的部分将被视为解释器指令。同时会把文件所在路径作为参数附在解释器后面。上例中，如果文件是`/usr/local/bin/pip`，则直接运行`/usr/local/bin/pip`的效果，等同于`/usr/local/opt/python/bin/python3.6 /usr/local/bin/pip`。 

这样做，使得用户无需关心解释程序，无需关心代码编写的语言，直接运行对应的命令行程序本身就好了。这也是shebang存在的意义。不过，由于系统设定的原因，使用windows的同学可能无法享受这种便利，一般还需手动指定解释程序的路径。但是，他们可以双击运行:-)。

可以试着用文本编辑工具打开一个NodeJS写成的脚本如：`webpack`，会发现其第一行是`#!/usr/bin/env node`。这句话并不是直接的NodeJS的解析程序。这里， `/usr/bin/env`是一个程序，目的是从系统的PATH中寻找对应名字的解释程序的地址。此时，解释程序可以被安装在各种路径，只要在系统PATH中注册过，就可以找到了。

可能大家遇到过这种问题，在运行某些NodeJS程序会出现报错：

`/usr/bin/env: node: No such file or directory`

此时可以从系统PATH中是否有node这个文件路径、某些版本的NodeJS是否名为node等方向来排查问题。

### NodeJS相关：好用的命令行工具

在NodeJS目前已经成为前端工作流的主力语言的情况下， `babel`和`webpack`基本已经成为前端开发、测试、发布上的重要工具。同时围绕`babel`和`webpack`有一系列周边的工具包和插件协助开发者完成日常开发的方方面面。

同时，目前最为流行的前端框架Angular、react、vue（以首字母为序），各自有自带的脚手架和开发辅助工具。如`ng-cli`、`create-react-app`和`vue-cli`等等。更有[Poi](https://poi.js.org/)这样的通吃React和Vue的脚手架工具。

上面这部分每一个都可以独立出来单独讲解。有兴趣的读者可以参考上述工具的官方网站获取更多信息。

下面来说几个其他方面的NodeJS相关的软件包。

#### 多版本共存 n/nvm

大多数情况，我们只需面对单一的NodeJS版本。等到时机成熟，再统一把NodeJS版本升级到更高版本。

不过笔者就曾经遇到过一个年久失修失修的项目需要重新维护的情况。此时需要把NodeJS版本切到老版本。同时，我们也不想舍弃大多数项目运行的新版本NodeJS环境。

这种情况可以使用n或nvm。下图展示了，用n下载并切换到一个新版本的过程。

![](https://p0.ssl.qhimg.com/t0195ca7651263de14e.gif)

除了下载之外，n还提供了列表的方式切换多个版本，以及删除某个版本的方法。读者可以在安装之后使用`n -h`查看所有可用参数。

n采用bash编写。但提供了一个npm仓库安装的入口，可以使用大家传统意义的npm安装法进行全局安装，前提是你必须有一个可以运行的NodeJS环境。

`npm install -g n`

或者在没有NodeJS的环境下，可以使用`n-install`脚本。安装只需运行：`curl -L https://git.io/n-install | bash`。

如果是windows用户，在windows10下面可以安装`wsl`来获得Linux脚本运行环境，官方仓库的一个issues，对此有一个[操作说明](https://github.com/tj/n/issues/511)。

对windows10以下的用户，可以考虑折腾下`Cygwin`。

除了n之外，还有一个管理工具为nvm，也是采用bash脚本编写。安装亦可使用安装脚本来完成。如：`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash` 或 `wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`。这里的`v0.34.0`是版本号，可能会随着版本迭代而变化。

使用windows的读者，除了上述`wsl`和`Cygwin`之外，可以考虑使用[nvm-windows](https://github.com/coreybutler/nvm-windows)这个用Golang编写的版本。

就目前的最新版本来说，n和nvm的都会尝试处理公共的依赖库，然而处理方式是不一样的。

n和nvm都会在首次使用某个版本时将此版本的NodeJS下载至本地，不同的是：n将尝试用新版本代替系统路径中，关键路径如bin、lib、include、share的包。nvm则是保留每一个版本的副本，并将NodeJS的系统路径指向.nvm维护的沙箱地址。

从处理上，nvm显得更轻量和高效，但是需要修改系统的PATH，这一步nvm脚本会自动完成。n则无需入侵系统路径，但每次修改时候均需操作系统路径，且此时最好使用`sudo n`运行，避免因权限不足，拒绝向系统路径复制。

由于nvm会修改PATH地址，所以如果同时默认安装nvm和n，n会运转不正常。一种方案是避免同时安装，另外可以手动修改PATH，使默认的NodeJS路径先于nvm的系统路径，如修改PATH片段为：

`/usr/local/bin:/Users/leon/.nvm/versions/node/v10.6.0/bin:`

#### 执行辅助 nodemon/npx

nodemon是一个执行器，意义在于，如果版本变化或者程序变化，无需重新启动。这在开发时候非常有用。

nodemon还可以指定运行的端口，如：

`nodemon ./server.js localhost 8080`

除了控制NodeJS包之外，nodemon还可以控制非NodeJS脚本，比如：`nodemon --exec "python -v" ./app.py`，将监控app.py的内容，并在最开始以及发生变化时候，调用`python -v`进行解析。当然，如果你的app.py指定了shebang，也可以不需指定解析函数。

![](https://p3.ssl.qhimg.com/t014868a7eb1105e2cf.png)

nodemon有很多灵活的配置，通过这些配置，可以实现环境变量设置、延迟启动、命令执行、监控定制扩展名、优雅重启、事件监听等功能。做法是在需要这些配置的目录下，提供相关的配置`nodemon.json`，也可以在package.json中通过`nodemonConfig`字段指明。

[这里](https://github.com/remy/nodemon/blob/master/doc/sample-nodemon.md)是官方提供的一份配置文件的样例，供读者参考。

再来说说npx。什么是npx呢？简单说，就是找到并运行一个包，并且“用完即走”。

这里有两层意思：

1. 找到。从哪里找：先是当前的依赖，然后是PATH，还找不到就到网上找来安装。
2. 用完即走。即使从网上安装的，运行完就会删掉，不会留下运行的包。 读者可以试着运行下：`npx github:piuccio/cowsay "awesome npx" `体验下。

这实在是居家旅行、开发调试的利器。比如我要在当前目录下开一个http服务，可以直接运行：`npx http-server`

![](https://p5.ssl.qhimg.com/t0159c53cabfea02e7c.png)

之后就可以直接在浏览器访问这个地址进行调试了。

另外，如果你需要临时用一个老版本的node来运行某个脚本，也可以祭出npx，这个node会被临时安装、临时使用、用完即走。

`npx -p node@6 npm init`

#### 切换NodeJS注册表 nrm/yrm

nrm/yrm维护了一个列表，包括npm主站和其他镜像。可以使用nrm/yrm use 快速切换，以达到最快的下载速度。nrm维护的是npm的注册表，yrm维护的是yarn注册表。

![](https://p5.ssl.qhimg.com/t0130c2e758a293a28a.png)

### 辅助编写NodeJS包

除了直接用大神们写好的命令之外，我们也可以按照自己的需求定制自己需要的NodeJS包。我们知道，命令行其实也是一种人机交互，因此，交互上有很多可以借鉴的效果。编写者只需将包倒入就可以使用这些交互效果。这里笔者给大家推荐几个包

#### 命令行参数读取 commander

命令行的一个特点就是根据参数的不同调整运行策略。然而处理命令行输入以及验证是一个非常繁琐的事情。为此，TJ大神曾经创立了commander包。最基础的用法如下：

```JavaScript

var program = require('commander');

program
  .version('0.1.0')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq-sauce', 'Add bbq sauce')
  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.peppers) console.log('  - peppers');
if (program.pineapple) console.log('  - pineapple');
if (program.bbqSauce) console.log('  - bbq');
console.log('  - %s cheese', program.cheese);

```

默认地，commander会自动创建-h的帮助文件，即利用每一个option的输入产生帮助文案。

![](https://p3.ssl.qhimg.com/t012e3350df9aca2696.png)

用户的每一个输入，都会放置在program对应option长名的字段的驼峰形式上，如果没有提供长名，则放在短名字段上。上例中，如使用： `testcommander -p 111 -P 222 -b 333`则依次存储在`program`的`peppers`、`pineapple`和`bbqSauce`上。

同时，commander提供多种验证方式，如正则表达式：

`program.option('-s --size <size>', 'Pizza size', /^(large|medium|small)$/i, 'medium')`

则指定只能输入特定的值。

同时，commander提供一个方案，允许用户设置子命令。commander称之为Git风格的子命令。

```JavaScript

var program = require('commander');

program
  .version('0.1.0')
  .command('install [name]', 'install one or more packages')
  .command('search [query]', 'search with optional query')
  .command('list', 'list packages installed', {isDefault: true})
  .parse(process.argv);

```

这个例子中，假设命令行名字为`pm`，则当用户输入`pm-install`、`pm-search`或`pm-list`时候，commander会尝试在入口文件的同一级目录找到`install`、`search`或`list`，并交给这个文件去执行。


#### 进度条 progress

在编写web程序时候，大家经常会展示一个进度条。用以缓解用户在等待时候的焦虑。其实在命令行程序中也会有这种交互方式。比如wget就会在下载过程中给出进度提示。

在NodeJS中也有这样的效果可以使用。这就是progress包。下面的代码，运行结果是下载CentOS安装盘。在下载之中，会实时打印进度

```JavaScript
const ProgressBar = require("progress")
const request = require("request")
const progress = require("request-progress")
const fs = require("fs")

const download = (url, headers, target, totalSize) => {
    let percent = 0

    const bar = new ProgressBar('下载中: ├:bar┤ 完成:percent 预估完成时间:eta秒 用时:elapseds', {
        total: 100,
        complete: "█",
        incomplete: "─",
        width: 60
    })

    let opt = {
        headers,
        url: url
    }

    return new Promise((resolve, reject) => {
        progress(request.get(opt))
            .on('progress', function (state) {
                let progressFix = ((state.percent) * 100).toFixed(2)
                delta = progressFix - percent
                bar.tick(delta)
                percent = progressFix
            })
            .on("error", () => {
                return reject()
            })
            .on('end',  () => {
                bar.tick(100 - percent)
                console.log('\n')
                return resolve(target)
            })
            .pipe(fs.createWriteStream(target));
    })
}

const foo = {
    getHeaders: () => {
        const headers = {
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
            'Accept-Charset': 'UTF-8,*;q=0.5',
            'Accept-Encoding': 'gzip,deflate,sdch',
            'Accept-Language': 'en-US,en;q=0.8',
            'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:13.0) Gecko/20100101 Firefox/13.0'
        }

        return Object.assign({}, headers)
    },

    download: function (url, target, totalSize){
        let headers = this.getHeaders()
        headers = Object.assign(headers)

        download(url, headers, target, totalSize)
    }
}


foo.download("http://mirrors.cmich.edu/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso",
    "CentOS-7-x86_64-DVD-1810.iso", 4508876.8
    )

```

运行的结果如图：

![](https://p3.ssl.qhimg.com/t0137ac669ec46e9bfc.png)

这个包的核心就是根据内置和自定义的token在命令行打印出相应的字符，用以完成交互。

#### 交互着色 chalk

chalk是一个命令行交互的着色工具。在命令行支持的情况下，可以支持最多16位色域（前提是命令行终端可以支持）。一般可以配合console.log使用，如：

```JavaScript
const chalk = require('chalk');
const log = console.log;

// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));
```

笔者曾经做过一个在命令行下显示图片的程序，就是利用的chalk和console.log进行的配合。

![](https://p1.ssl.qhimg.com/t017c751adaee8e942c.gif)


#### 交互式问答 inquirer

在需要不断的同用户进行交互式问答，并根据用户的输入进行验证和路径选择，这个时候inquirer是非常趁手的工具。它内置了单选、多选、问答等多种交互方式。大家可以感受下：

![](https://p2.ssl.qhimg.com/t011037120b9d788a0e.png)

![](https://p0.ssl.qhimg.com/t01b114ade03f904bea.png)

![](https://p4.ssl.qhimg.com/t018baae438c4be5245.png)

![](https://p4.ssl.qhimg.com/t0172b9fcb7cb7fef2a.png)

![](https://p1.ssl.qhimg.com/t01b6347e184fe8344c.png)

![](https://p4.ssl.qhimg.com/t016cd26d3bae6fc73d.png)

甚至可以通过插件实现suggest

![](https://p5.ssl.qhimg.com/t01f1f765b6f653541f.gif)

vue框架的脚手架vue-cli是一个使用inquire的绝佳案例，读者可以通过阅读源码，感受下大神出神入化的使用。

#### 小图标 ora

ora打印出一个优雅的文本小图标，用于在各种情况下给出用户优雅而清晰的提示。用法很简单：

```JavaScript
const ora = require('ora');

const spinner = ora('Loading unicorns').start();

setTimeout(() => {
	spinner.color = 'yellow';
	spinner.text = 'Loading rainbows';
}, 1000);
```

![](https://p5.ssl.qhimg.com/t0101472abafa3f07e7.gif)


#### 命令行玩浏览器 puppeteer

puppeteer是谷歌开发的无头浏览器，使得命令行亦可操作浏览器，并能根据浏览器的执行结果进行进一步操控。因为puppeteer源自官方，所以之前类似项目PhantomJS的开发者决定不再更新PhantomJS。

目前puppeteer已经广泛用于前端测试，端对端测试，以及爬虫。

鉴于篇幅无法展开介绍，读者可以参考其[官方文档](https://github.com/GoogleChrome/puppeteer)。同时，奇舞周刊中黄小璐老师的的[这篇文章](https://mp.weixin.qq.com/s/TZgQPKrpIskIgxX3LCXZYw)以及李光钊老师的[这篇文章](https://mp.weixin.qq.com/s/Xypg-9qZ8OqsPczEKdn6JA)都曾经介绍过puppeteer的使用。

### 发布NodeJS包 

写好的NodeJS包需要发布出去，才能给大家使用。`npm publish`就是为了这个需求而产生的。为了发布你需要在npm上注册用户，并登录，然后发布就好了。npm的详情页面以及各个镜像会在一段时间内自动更新。

如果你的NodeJS包，是使用尚未广泛支持的语法写成的。那么需要在package.json的`script`字段加入`prepublish`命令，调用babel等预编译器处理，使得程序可以有更多的兼容性。 

对于希望用户在全局使用的命令，要注意在根目录写好入口，一般是在package.json中的bin字段，指定入口文件。在安装时，如果是全局安装，npm将会使用符号链接把这些文件链接到prefix/bin，如果是本地安装，会链接到./node_modules/.bin/。

除了通常的包，还有一种是带有scope的包，vue-cli的3.0版本就是@vue开头的。这个scope是组织的名字。每一个带有scope的包有公有和私有之分，私有的需要付费给npm。

目前npm的读写权限策略如下：

![](https://p4.ssl.qhimg.com/t0136857f4d4d4d70b0.png)

如果是个人，可以考虑增加公有的命名空间。如果是企业付费用户，你在发布相关包之前，需要申请成为这个scope的member。

对公有scope，首先将包的name改为`@scope名字/包名`，同时，在发布时，使用`npm publish --access public`即可。

### 小结

本文简述了命令行的意义和优势，介绍了解释型命令行的运行机制，同时介绍了几个NodeJS相关的命令行工具，推荐了几款撰写命令行程序常用的包，最后，概述了发布包和使用scope的发布情况。希望给大家的NodeJS命令行相关开发和技术选型，提供一些有用的帮助。