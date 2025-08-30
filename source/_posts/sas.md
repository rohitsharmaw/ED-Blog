---
title: 如何使用学生分析系统
date: 2025-08-30 13:05:30
tags: ['工具','文档']
categories: ['文档', '工具']
poster:
    headline: 如何使用学生分析系统
    topic: 了解学生的利器
katex: true
author: ED_Builder
---

一个由 Github Copilot Pro & Gemini 2.5 Pro & GPT-4.1 联合构建的结果

<!-- more -->

不得不说 AI 真的太好用了，以前用的 Microsoft Copilot Think Deeper 和 Github Copilot Free 写的和史一样
### 基本信息
- 作者：ED_Builder、Github Copilot Pro、Gemini 2.5 Pro、OpenAI GPT-4.1
- 仓库：[rohitsharmaw/SAS](https://github.com/rohitsharmaw/SAS)（暂时无法访问）
- 构建方法：静态构建，使用 [Cloudflare Pages](https://pages.cloudflare.com) 或 [GitHub Pages](https://pages.github.com) 都可以
- 开源协议：GNU Affero General Public License v3 (AGPLv3)

等级图标使用 Arcaea &copy; Lowiro 的潜力值背景，站点图标使用 [Cloudflare](https://cloudflare.com) 的图标集。  
图表构建使用 [ChartJS](https://chartjs.org)（MIT）
{%folding 限制条件 color:red%}
根据 AGPLv3 许可证，你可以：
- **自由使用和修改**：你可以随意使用、修改这个软件的源代码，甚至可以把它集成到自己的项目中。
- **自由分发**：你可以把原版或修改后的软件分发给别人，甚至可以收费。
- **用于网络服务**：你可以把它部署成网站、API 服务、SaaS 平台等供用户使用。
- **商业使用**：你可以在商业项目中使用它，只要你遵守它的规则。

你必须：
- **公开源代码**： 如果你修改了 AGPLv3 授权的软件，并通过网络提供服务（比如网站或 API ），你必须把修改后的源代码也公开给用户。
- **保留原始版权声明**：你不能删掉原作者的版权信息和许可证说明。
- **标明修改内容**：如果你改了代码，必须清楚地标注你改了什么、什么时候改的。
- **附带许可证文件**：每个源代码文件都要包含 AGPLv3 的许可证说明。

禁止你：
- **闭源部署网络服务**：如果你用它做了一个网站或服务，不能只部署而不公开代码。
- **附加额外限制**：你不能在分发时加上额外的限制，比如禁止别人再修改或再分发。
- **用于绕过开源义务**：比如你不能把它包在一个闭源系统里，试图“隐藏”它的存在。

也就是说：  
如果你使用了本项目，并且只是自己私有使用，你可以在修改源代码之后不公开；但是如果他人使用，你必须公开源代码并保留原作者的版权信息和许可证说明。

如果您违反了 AGPLv3 协议，我们有权终止您的使用许可，直到您整改完毕  
```LICENSE
  However, if you cease all violation of this License, then your
license from a particular copyright holder is reinstated (a)
provisionally, unless and until the copyright holder explicitly and
finally terminates your license, and (b) permanently, if the copyright
holder fails to notify you of the violation by some reasonable means
prior to 60 days after the cessation.
```
{%endfolding%}
### 自行部署
1. 在 Github 上 fork 本仓库
2. 新建一个 Cloudflare Pages，并连接到你 fork 的那个仓库
3. 保持默认配置，部署即可使用
### JSON 文件格式
**请注意**：{%mark 文件的正确配置直接决定了分析系统的运行，请一定按照给定格式进行编辑！%}  
本文档为了解释每个部分的内容，选择了 JSONC 格式（标准 JSON 不允许注释），各位在投入使用时请务必删除注释！
```jsonc
{
    "id": "ED_Builder",//学生 ID
    "name": "ED_Builder",//学生姓名
    "class": "A",//班级
    "matrix"://能力雷达图
    {
        "代码能力": 100,//对应的领域和能力值（也可以是浮点数）
        "算法能力": 100,//可以配置不同数量
        "构造能力": 100,
        "数据结构": 100,
        "知识储备": 100,
        "文化课": 0
    },
    "knowledge"://学习内容知识树
    {
        "id": "root",//知识点 ID
        "name": "OI",//知识点名称
        "difficulty": 10,//难度（可以是浮点数）
        "learned": false,//是否学习完成（必须是 bool 值）
        "progressing": true,//是否正在学习
        "children"://这个知识点下面的分支
        [
            {
                "id": "graph",
                "name": "图论",
                "difficulty": 5,
                "learned": true,
                "progressing": false,
                "children":
                [
                    {
                        "id": "save",
                        "name": "图的存储",
                        "difficulty": 5.1,
                        "learned": true,
                        "progressing": false,
                        "children": []
                    },
                    {
                        "id": "traverse",
                        "name": "图的遍历",
                        "difficulty": 5.2,
                        "learned": true,
                        "progressing": false,
                        "children": []
                    }
                ]
            },
            {
                "id": "dp",
                "name": "动态规划 DP",
                "difficulty": 7,
                "learned": false,
                "progressing": true,
                "children":
                [
                    {
                        "id": "backpack",
                        "name": "背包DP",
                        "difficulty": 7.1,
                        "learned": true,
                        "progressing": false,
                        "children": []
                    },
                    {
                        "id": "tree",
                        "name": "树形DP",
                        "difficulty": 7.6,
                        "learned": false,
                        "progressing": true,
                        "children": []
                    }
                ]
            }
        ]
    }
}
```
### 上传分析
将你的 JSON 文件保存好以后，打开分析系统，选择你的 JSON 文件（可以选择多个），下面就会生成图表了  
并且我们会根据雷达图各项指标的平均值进行等级判定哦（就是这里用了 Arcaea 的图标）
### 花絮
如果你希望个性化你的站点或更改等级计算方式等，可以修改我们提供的样式表和脚本（请注意阅读限制条件）

其实这个东西是我的教练 d*****4 让我开发的，当时教练还给我看了 ta 让 DeekSeep（？）生成的方案，而且还用到了 Django。  
因为当时教练的期望是把所有学生的数据都存放到服务器，还要通过用户名和密码进行鉴权然后看到所有学生的分析。  
奈何我太蒻了用了一晚上的 AI 都没做出来。后续还会更新支持 KV 命名空间和环境变量的版本，不过那就是时间问题 ~~还有记性问题~~ 了。