动手写代码之前，先把 Claude Code 装好、跟模型连上。

## 安装

我们先去到Claude Code官网

> https://code.claude.com/docs/en/overview



找到这里的安装命令

![](<images/环境准备：Claude Code 安装配置-image-9.png>)



我们根据自己的操作系统来选择对应的安装命令，比如我是macOS，就选择第一条

> curl -fsSL https://claude.ai/install.sh | bash



如果是windows，我们就选择第二或者第三条

> irm https://claude.ai/install.ps1 | iex
>
> curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd



## 配置模型

### 官方订阅

https://claude.ai/

使用 Claude Code 时，模型首选当然是 Anthropic 自家的 Claude，而最具性价比的方式是直接订阅套餐——对大多数用户来说，每月 $100 的 Max 计划已经足够。如果预算有限，也可以参考下面的国产模型替代方案。

![](<images/环境准备：Claude Code 安装配置-image-7.png>)



如果订阅了Claude，只需要在本地终端输入Claude，然后用下面截图第一种方式，网页授权登录即可使用

![](<images/环境准备：Claude Code 安装配置-image-8.png>)



### GLM

GLM模型算是比较有性价比的了，而且订阅方式非常方便——https://www.bigmodel.cn/glm-coding?ic=WQLTOYQV0S



正常来说订阅一个Lite就够用了

![](<images/环境准备：Claude Code 安装配置-image-4.png>)

然后可以到Coding Plan工作台，创建一个Api Key——https://bigmodel.cn/coding-plan/personal/overview

![](<images/环境准备：Claude Code 安装配置-image-5.png>)



有了Apikey，建议直接参考GLM官方文档这个方式三来进行手动配置，记得把your\_zhipu\_api\_key替换成刚才创建的

https://docs.bigmodel.cn/cn/coding-plan/quick-start

![](<images/环境准备：Claude Code 安装配置-image-3.png>)



### DeepSeek

在Claude Code接入DeepSeek也是非常好的选择，可以在这里创建一个Api Key——https://platform.deepseek.com/api\_keys，然后自行进行充值https://platform.deepseek.com/top\_up使用，DeepSeek最近价格折扣，还是非常耐用的

![](<images/环境准备：Claude Code 安装配置-image-6.png>)



最后就是修改一下你的Claude Code配置，改成使用DeepSeek的，可以参考官方文档完成

https://api-docs.deepseek.com/zh-cn/quick\_start/agent\_integrations/claude\_code

![](<images/环境准备：Claude Code 安装配置-image-1.png>)

## 测试使用

安装完成后，我们打开终端，输入

> claude



就会进入到我们的终端界面了

![](<images/环境准备：Claude Code 安装配置-image.png>)

我们可以输入一句你好试试

![](<images/环境准备：Claude Code 安装配置-image-2.png>)

可以看到，AI会对你进行回答，恭喜你正式开启了AI编程之路！

## 安装 Superpowers 插件

Superpowers 是 Claude Code 的一组技能，把这门课要求的开发纪律（需求澄清、计划评审、TDD、代码评审）焊进了流程，装好之后不需要额外配置，技能会自动触发。

终端里执行：

> claude plugin install superpowers@claude-plugins-official

或者进了 Claude Code 会话里再装也一样：

> /plugin install superpowers@claude-plugins-official

装完重启 Claude Code，插件加载生效。

## 准备 git worktree

git worktree 是 git 自带的子命令，近些年的 git 版本都带，不用额外装，确认本地 git 能正常用就行。再给课程项目准备一个干净的 git 仓库，后面每章开新功能都从它开出独立的 worktree，改砸了整间目录一扔，主仓库不受影响。
