# Brief

最早是在豆瓣时期做 Douban app engine (Aka DAE) 的时候，我们遇到了一系列的问题，其中最重要也是最棘手的就是如何保证 runtime 的隔离。经过两年的开发，DAE 作为一个 Pure Python PaaS 已经很好的驱动了所有的豆瓣业务，在语言层上也做了一定颗粒度的隔离，但是在底层调度和编排上受限于 CPython 自己的 runtime，自始至终也没很好的解决应用间的隔离和资源配额。

即便是强如豆瓣这样的团队，完全把精力放在 lxc 自行研发一套容器引擎也是一件非常不现实的事情，这时候我们把目光放在了新生事物 Docker 上面。但是在13年年末的时候，Docker 毕竟还很年轻。我们也只是看了看，跑了跑，并未基于此开发。然后我就离职了……

等我回国的时候已经是14年年初，Docker 也发布了 1.0 里程碑式的版本。从生产来看，已经有部分公司开始使用 Docker 来解决运维自动化等一系列问题。在加入芒果TV之后，我组建起新的团队开始在这个方面寻求解决隔离问题的终极版「DAE」。

一开始我们实现了一个基于 Python 开发的 PaaS 平台，Aka NBE。小范围测试后发现效果还不错，资源利用和部署效率上均有不错的提升。在那个年代 K8S 还只是刚出茅庐，Mesos 的 Marthon 框架口碑不错，但公认太重，而且也是刚发布不就。至于原生的 Swarm 还不知道在哪里，于是乎我们决定自研调度编排框架，这就是 [Project Eru](https://github.com/projecteru) 的开始。

经过1年多左右的开发，Eru 一代目很好的完成了自己的目标。截止15年底，至少有 1/5 的芒果TV 业务基于此驱动，在效率和资源成本上达到了局部最优。并且我们的平台上托管了所有的 Redis，很长一段时间内芒果TV的所有 Redis 都是以 Cluster 形式在容器中由 Eru 驱动的。

然后，我们又离职了。

15年的时候，之前豆瓣平台组的 leader 洪教授 (Aka [hongqn](https://github.com/hongqn)) 去了宜信大数据，他们基于 docker 的 swarm 开发了 [lain](https://github.com/laincloud)。作为嫡系我们也和教授他们聊了很多，互通了各自设计的有无，比如他们的自举，比如我们的资源分配算法，比如他们偏 PaaS，比如我们偏 IaaS。16年我们离职后加入 [ENJOY](https://enjoy.ricebook.com/)，在 Eru1 的基础上开始进行 v2 的开发，在之前的基础上吸收了不少 lain 优秀的设计，并于16年底完成了驱动 ENJOY 所有业务的目标。

既然博采众长，那 lain 完善的文档也是一大亮点之一，这就是这本白皮书的由来，感谢友军的工作。在这本白皮书里面我们也会跟 lain 的[白皮书](https://laincloud.gitbooks.io/white-paper)一样介绍 Eru2 的设计方面的各种细节，希望读者能从更高的角度了解这个项目。