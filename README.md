# 积木式编程
从用户的角度来看，Blockly是一种直观，可使的构建代码的方式。 从开发人员的角度来看，Blockly是一个现成的用户界面，用于创建可视语言，从而为用户生成正确的代码。 Blockly可以将块导出为许多编程语言，包括这些流行的选项：JavaScript、Python、PHP、Lua、Dart。

Blockly一种在网页上运行的图形化编程语言。使用者以拖拽拼图的方式开发出应用程序。不需要任何的代码编写。
![image](https://user-images.githubusercontent.com/101512741/186431143-ba8de972-92bd-4793-808e-1eef63fcbede.png)



# Netless App Template

[Netless App 文档](https://github.com/netless-io/window-manager/blob/master/docs/develop-app.md)

## 快速开始

前置条件：至少需要安装了 `git`、`node 16`、`npm 8`。

1.  点击右上角 `Use this template` 生成一个属于你自己的仓库

    如果需要选用原生以外的模版，请勾上 `Include all branches`

    > 如果你安装了 [GitHub CLI](https://cli.github.com)，也可以用这个命令一键生成并 clone 本仓库
    >
    >     gh repo create your-app-name --template netless-io/community-apps --include-all-branches

2.  在 .env 文件里配置白板房间 UUID 和 Token

    请将本目录下的 .env.example 文件复制一份，重命名为 .env 或 .env.local 后，在里面填写必须的白板配置信息。你可以在 [Netless Workshop](https://workshop.netless.link) 申请专用的白板配置。

3.  执行 `npm install` 安装依赖 

4.  执行 `npm install blockly` 安装依赖 

5.  执行 `npm run start` 进行本地开发

## 提交到社区 App 列表

开发完 App 后，可以在 [Community Apps](https://github.com/netless-io/community-apps) 仓库提交你的项目，方便更多人发现和使用。

## 清单

- [ ] 添加 .env 或 .env.local
- [ ] 修改 package.json 里的 `name`，`private` 等字段
- [ ] (可选) 发布 npm 包
- [ ] 添加 License
- [ ] 修改 README
- [ ] (可选) 添加 Logo
