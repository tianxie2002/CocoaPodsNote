下载和安装CocoaPods？  
用淘宝的Ruby镜像来访问cocoapods。按照下面的顺序在终端中敲入依次敲入命令：  
$ gem sources --remove https://rubygems.org/
//等有反应之后再敲入以下命令  
$ gem sources -a http://ruby.taobao.org/  
为了验证你的Ruby镜像是并且仅是taobao，可以用以下命令查看：      
$ gem sources -l  
只有在终端中出现下面文字才表明你上面的命令是成功的：

http://ruby.taobao.org/

在终端中运行：  
$ sudo gem install cocoapods


使用CocoaPods？
利用vim创建Podfile，运行：

$ vim Podfile
然后在Podfile文件中输入以下文字：

platform :ios, '7.0'
pod "AFNetworking", "~> 2.0"
注意，这段文字不是小编凭空生成的，可以在AFNetworking的github页面找到。这两句文字的意思是，当前AFNetworking支持的iOS最高版本是iOS 7.0, 要下载的AFNetworking版本是2.0。

然后保存退出。vim环境下，保存退出命令是：

:wq
这时候，你会发现你的项目目录中，出现一个名字为Podfile的文件，而且文件内容就是你刚刚输入的内容。注意，Podfile文件应该和你的工程文件.xcodeproj在同一个目录下。

这时候，你就可以利用CocoPods下载AFNetworking类库了。还是在终端中的当前项目目录下，运行以下命令：

$ pod install 
因为是在你的项目中导入AFNetworking，这就是为什么这个命令需要你进入你的项目所在目录中运行。

运行上述命令之后，小编的终端出现以下信息：

EricmatoMacBook-Pro:CocoaPodsDemo ericwang$ pod install
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.0.2)
Generating Pods project
Integrating client project

[!] From now on use `CocoaPodsDemo.xcworkspace`.
注意最后一句话，意思是：以后打开项目就用 CocoaPodsDemo.xcworkspace 打开，而不是之前的.xcodeproj文件。

你也许会郁闷，为什么会出现.xcodeproj文件呢。这正是你刚刚运行$ pod install命令产生的新文件。除了这个文件，你会发现还多了另外一个文件“Podfile.lock”和一个文件夹“Pods”。 点击 CocoaPodsDemo.xcworkspace 打开之后工程之后，项目Xcode目录结构如下图：

Figure 5

你会惊喜地发现，AFNetwoking已经成功导入项目了（红框部分）！

现在，你就可以开始使用AFNetworking.h啦。可以稍微测试一下，在你的项目任意代码文件中输入：

#import <AFNetworking.h>
或者
#import "AFNetworking.h"
然后编译，看看是否出错。如果你严格按照小编上述的步骤来，是不可能出错的啦。

至此，CocoPods的第一个应用场景讲述完毕。别看小编写了这么多，其实过程是十分简单的。总结一下就是：

先在项目中创建Podfile，Podfile的内容是你想导入的类库。一般类库的原作者会告诉你导入该类库应该如何写Podfile；
运行命令：`$ pod install.
下面，小编继续讲述第二种使用场景。

场景2：如何正确编译运行一个包含CocoPods类库的项目

你也许曾经遇到过（特别是新手iOS开发者）这种情况，好不容易在GitHub上找到一份代码符合自己想需求，兴冲冲下载下来，一编译，傻眼了，发现有各种各样错误。一看，原来是缺失了各种其他第三方类库。这时候莫慌，你再仔细一看，会发现你下载的代码包含了Podfile。没错，这意味着你可以用CocoaPods很方便下载所需要的类库。

下面，小编以代码 UAAppReviewManager 为例来说明如何正确编译运行一个包含CocoPods类库的项目。

UAAppReviewManager是一个能够让你方便地将提醒用户评分的功能加入你的应用中。当你去UAAppReviewManager的GitHub地址下载这份代码之后，打开Example工程（UAAppReviewManagerExample），编译，你会发现Xcode报告一大堆错误，基本都是说你编译的这份代码找不到某某头文件，这就意味着你要成功编译UAAppReviewManager的Example代码，必须先导入一些第三方类库。同时你会发现在UAAppReviewManagerExample文件夹下面有三个跟CocosPods相关的文件（文件夹）：Podfile，Podfile.lock和Pods，如下图：

Figure 6

用

这时候，打开终端，进入UAAppReviewManagerExample所在的目录，也就是和Podfile在同一目录下，和场景1一样，输入以下命令（由于已经有Podfile，所以不需要再创建Podfile）：

$ pod update
过几秒（也许需要十几秒，取决于你的网络状况）之后，终端出现：

Analyzing dependencies
Fetching podspec for `UAAppReviewManager` from `../`
Downloading dependencies
Installing UAAppReviewManager (0.1.6)
Generating Pods project
Integrating client project

[!] From now on use `UAAppReviewManagerExample.xcworkspace`.
这时候，再回到UAAppReviewManagerExample文件夹看一看，会看到多了一个文件UAAppReviewManagerExample.xcworkspace：

Figure 7

根据终端的信息提示，你以后就需用新产生的UAAppReviewManagerExample.xcworkspace来运行这个Example代码了。

打开UAAppReviewManagerExample.xcworkspace，编译运行，成功！如下图：

Figure 8

注意，这里有个小问题，如果刚刚你不是输入$ pod update，而是输入$ pod install，会发现类库导入不成功，并且终端出现下面提示：

[!] Required version (UAAppReviewManager (from `../`)) not found for `UAAppReviewManager`.
Available versions: 0.1.6
这里的意思大概是Podfile文件过期，类库有升级，但是Podfile没有更改。$ pod install只会按照Podfile的要求来请求类库，如果类库版本号有变化，那么将获取失败。但是 $ pod update会更新所有的类库，获取最新版本的类库。而且你会发现，如果用了 $ pod update，再用 $ pod install 就成功了。

那你也许会问，什么时候用 $ pod install，什么时候用 $ pod update 呢，我又不知道类库有没有新版本。好吧，那你每次直接用 $ pod update 算了。或者先用 $ pod install，如果不行，再用 $ pod update。