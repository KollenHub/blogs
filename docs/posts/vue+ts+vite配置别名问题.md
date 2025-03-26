---
title: vue+ts+vite环境中,无法解析@路径别名 - 哔哩哔哩
url: https://www.bilibili.com/opus/971721580395626504
publishedTime: null
---

一百度全是过时的答案，问题出在ts配置上

————————————————————————————

先一句话说明问题，路径别名要分别配置在vite和ts中，vite照着网上的教程配置就好。

ts部分，网上都说在tsconfig.json文件中配置，但是想要正常使用路径别名，配置必须放在tsconfig.app.json文件里面

—————————————————————————————

使用vite创建基于vue+ts模板生成的项目文件，根目录下会存在tsconfig.json、tsconfig.app.json、tsconfig.node.json三个ts配置文件。

根据网上说法，据说在vite3以后，引进了references属性，可以根据环境使用不同的配置，app文件是angular专用的ts配置文件，node是node环境使用，最后tsconfig.json会引用这两份配置。

但是我自己实测，把路径别名配置放tsconfig.json里面，ts无法正常解析@路径，只能放在tsconfig.app.json里面。

猜测tsconfig.app.json是用于所有web环境的配置文件，为什么tsconfig.json配置却不能生效，实在很难理解，按理说这是一个层级配置关系，tsconfig.json里面的配置应该作为最终确定才对。

————————————————————————————————

浪费了好几天研究这个问题，本来vscode只是提示红线，我就忍了，但是东西写好连build也不行

开始还以为是tsc解析问题，我甚至还换了webpack，使用babel的ts插件来解析ts，没想到单纯是ts配置问题，真的吐血了
