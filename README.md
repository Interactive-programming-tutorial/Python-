<!--
author:   icbugcoder

email:    icbugcoder@88.com

version:  1.0.0

language: zh

narrator: icbugcoder

logo: https://aiycblog.oss-cn-zhangjiakou.aliyuncs.com/cover/2021-05-14-1.jpg

comment:  本教程适合想学习Python的爱好者，以及正在学习Python基础知识的同学学习！Python大佬可移步去查看Python爬虫，神经视觉教程，文档正在不断更新，欢迎浏览！

script:  https://cdn.code.run.aiyc.top/pyodide-statics/pyodide.js

@onload
window.languagePluginUrl = 'https://cdn.code.run.aiyc.top/pyodide-statics/'

window.pyodide_ready = true;

window.pyodide_modules = new Set()

window.py_packages = ["matplotlib", "numpy"]

window.loadModules = function() {
  languagePluginLoader.then(() => {
    console.log("pyodide is ready")
    if (window.py_packages) {

      for( let i = 0; i < window.py_packages.length; i++ ) {
        window.pyodide_modules.add(window.py_packages[i])
      }

      pyodide.loadPackage(window.py_packages).then(() => {
        console.log("all packages loaded")
        window.pyodide_ready = true;
      });
    }
    else {
      window.pyodide_ready = true;
    }
  })
}

window.loadModules()

@end


@Pyodide.eval: @Pyodide.eval_(@uid)

@Pyodide.eval_
<script>

function initPlot() {
try {

pyodide.runPython(`
import io, base64

try:
  img_str_
except NameError:
  img_str_ = {}

def plot(fig, id="plot-@0"):
  buf = io.BytesIO()
  fig.savefig(buf, format='png')
  buf.seek(0)
  img_str_[id] = "data:image/png;base64," + base64.b64encode(buf.read()).decode('UTF-8')
`)
} catch (e) {}
}

function copyPlot() {
  if ( pyodide.globals.img_str_["plot-@0"] ) {
    //document.getElementById("plot-@0").src = pyodide.globals.img_str_["plot-@0"]
    //document.getElementById("plot-@0").parentElement.style = ""

    console.html("<hr/>")
    console.html("<img src='" + pyodide.globals.img_str_["plot-@0"] + "' onclick='window.img_Click(\"" + pyodide.globals.img_str_["plot-@0"] + "\")'>")
  }
}

////////////////////////////////////////////////////

function runPython() {
  if (window.pyodide_ready) {
    pyodide.globals.print = (...e) => { e = e.slice(0,-1); console.log(...e) };

    setTimeout(() => {

      try {
        initPlot()

        let fin = pyodide.runPython(`@input`)
        if (fin) {
          console.log(fin)
        }

        copyPlot()

        send.lia("LIA: stop")
      } catch(e) {
        //window.py_packages = ["matplotlib"]

        let module = e.message.match(/ModuleNotFoundError: No module named '([^']+)/g)

        if (! module) {
          console.error(e)
          //let msg = e.message.match(/File "<unknown>", line (\d+)\n.*\n.*\n.*/g)

          //window.console.log(msg[0])

          send.lia("LIA: stop")
        }
        else if (module.length != 0) {
          module = module[0].split("'")[1]

          if (window.pyodide_modules.has(module)) {
            console.error(e)

            send.lia("LIA: stop")
          } else {
            console.debug("downloading module =>", module)
            window.py_packages = [ module ]
            window.pyodide_ready = false
            window.loadModules()
            runPython()
          }
        }
        else {
          console.error(e)

          send.lia("LIA: stop")
        }
      }
    }, 100)
  } else {
    setTimeout(runPython, 234)
  }
}

runPython()

"LIA: wait";
</script>

@end

-->

# Python入门教程

本教程适合想学习Python的爱好者，以及正在学习Python基础知识的同学学习！Python大佬可移步去查看Python爬虫，神经视觉教程，文档正在不断更新，欢迎浏览！

文档友情开源：AI悦创 && icbugcoder

如果你想加入编写文档，请参照主页 [社区贡献者中加入我们的方式](https://aiyc.top/contributors/index.html?_sw-precache=cd861b79bf60a062cb4b92eea796f2ab#%E5%8F%91%E5%B8%83%E4%B8%8E%E5%88%86%E4%BA%AB%E6%96%87%E7%AB%A0)

![](https://aiycblog.oss-cn-zhangjiakou.aliyuncs.com/cover/2021-05-14-1.jpg)

开始学习Python前，请忘掉培训机构的推销广告 <!-- class = "animated infinite bounce" style = "color: red;" -->

因为那些广告传言并不能让你静下心来认真学习Python，Python的基础虽然简单，但是当你到后期进阶的学习爬虫以及神经网络技术时你就会发现，Python的难度在于深层次的理解学习。

### 本文档阅读须知

本文档由icbugcoder && AI悦创友情开源，请勿用于商业用途

本文档所有内容会开源到github上，包括本站的文档解释器，在线运行代码包模块，如有需要请前往github组织账户页面去查看

前往链接: [点击前往](https://github.com/Interactive-programming-tutorial)

本文档严格遵守MIT 麻省理工学院许可证 

如不了解开源范围请查阅： [百度百科](https://baike.baidu.com/item/MIT%E8%AE%B8%E5%8F%AF%E8%AF%81/6671281?fr=aladdin)

虽然本文档为开源文档，但是创作者仍然要恰饭的，有兴趣一对一可以联系

> AI悦创·推出辅导班啦，包括「Python 语言辅导班、C++ 辅导班、java 辅导班、算法/数据结构辅导班、少儿编程、pygame 游戏开发」，全部都是一对一教学：一对一辅导 + 一对一答疑 + 布置作业 + 项目实践等。当然，还有线下线上摄影课程、Photoshop、Premiere 一对一教学、QQ、微信在线，随时响应！C++ 信息奥赛题解，长期更新！长期招收一对一中小学信息奥赛集训，莆田、厦门地区有机会线下上门，其他地区线上。 微信：Jiabcdefh

1v1介绍官网:[点击前往](https://www.class1v1.com/)

** 本文档内容所需材料链接，我会提供在文档页面内，请查看并自行判断是否需要下载 **

#### 学习社区

QQ频道：[点击链接跳转至QQ加入](https://qun.qq.com/qqweb/qunpro/share?_wv=3&_wwv=128&appChannel=share&inviteCode=1W7iVSG&businessType=9&from=246610&biz=ka)

扫描二维码加入请关注aiyc.top首页侧边栏


## 文档简介

>相信大家都听说过 Google旗下公司DeepMind团队研发的AlphaGo的围棋机器人战胜了人类

> 因此从2015年后，Python人工智能被宣传成了编程界的热门词汇。青少年也开始学习Python训练逻辑思维，到初中或高中学习C++语言参加奥林匹克竞赛进行考学方面的降低分数线或加分

学习之前我们要先了解一下Python可应用的领域：

1.大数据分析、计算 numpy，pandas【数据分析处理类】

2.数据采集，网页爬虫 requests 等【数据抓取类】

3.机器学习，神经网络 sklearn,tensorflow,pytorch，强化学习【神经网络视觉类】

仍然有很多领域可以应用，比如说Python脚本应用在安全领域渗透领域，django、flask等框架搭建网站等等！上三种属于Python应用较广的领域

当然 Python 还是有很多问题的，比如运行速度慢，跨平台有时候兼容性不好，不能很好的编译成二进制直接用。 但是有坏处，肯定也有好处，而且好处远远大于它的坏处，不然为什么变成最流行的语言？为什么大家都说人生苦短，我用Python？

Python是万能胶水，相比很多其他的语言，Python可以在多领域内进行变换操作不用学习更多的语言种类。
【除了在爬虫逆向需要懂一些前端知识javascript....】

爬虫，AI，数据，分析，可视化，web，服务，啥啥都能做。



### 交互式教程开发的原因

交互式教程有很多好处，比如可以在没有电脑的环境下随心的学习，在所给的代码块中输入自己的代码进行运行【虽然有些慢】

可以使编程教学做得更友善，更生动，更好学。

聊一聊本站的一些功能：

功能展示：

文本结构图像展示：

``````````````````````````````````````````````````
                             .--->  F
    A       B     C   D     /
    *-------*-----*---*----*----->  E
                         ^ v          /   '--->  G
               B --> C -'
``````````````````````````````````````````````````

Python基础语法调用

```Python
print("Hello")
```
@Pyodide.eval

Python数据分析模块第三方包调用

```Python
import numpy as np
import matplotlib.pyplot as plt

t = np.arange(0.0, 2.0, 0.01)
s = np.sin(2 * np.pi * t)

fig, ax = plt.subplots()
ax.plot(t, s)

ax.grid(True, linestyle='-.')
ax.tick_params(labelcolor='r', labelsize='medium', width=3)

plt.show()

plot(fig) # <- this is required to plot the fig also on the LiaScript canvas
```
@Pyodide.eval
