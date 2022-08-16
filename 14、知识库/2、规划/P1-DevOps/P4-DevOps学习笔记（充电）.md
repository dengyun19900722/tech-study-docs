# atlassian关于DevOps的实践

https://www.atlassian.com/zh/devops/frameworks/devops-metrics

**推荐书本：**

<Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations>



## 1 DevOps原则



## 2 DevOps框架

### 2.1 DevOps指标 -- 在 DevOps 中衡量成功的原因、内容和方法

​		为了实现 DevOps 的承诺（更快地交付更高质量的产品），团队需要收集、分析和衡量大量指标。这些 DevOps 指标提供了 DevOps 团队查看和控制其开发管道所需的基本数据。

​		DevOps 指标是指直接揭示 DevOps 软件开发管道性能的数据点，它们有助于快速识别和消除流程中的所有瓶颈。这些指标可用于跟踪技术能力和团队流程。

​		DevOps 的核心是模糊开发团队和运维团队之间的界限，从而加强开发人员和系统管理员之间的协作。借助指标，DevOps 团队可以衡量和评估协作工作流程，并跟踪实现高级目标的进度，包括提高质量、缩短发布周期和提高应用性能。

**四大 DevOps 指标**

- **变更的提前期**

需要跟踪的关键 DevOps 指标之一是[变更](https://www.atlassian.com/zh/itsm/change-management)的提前期。请勿与周期时间（下文会讨论）混淆，变更的提前期是指代码变更提交到主干分支与进入可部署状态之间的时长。例如，当代码通过所有必要的预发布测试时。

- **变更失败率**

变更失败率是指生产后需要热修复或其他补救措施的代码变更百分比。它不衡量在部署代码之前通过测试所捕获和修复的故障。

- **部署频率**

了解将新代码部署到生产环节的频率对于了解 DevOps 的成功至关重要。很多从业人员使用“交付”一词来表示发布到预生产阶段环境中的代码变更，并保留“部署”一词来表示发布到生产环节的代码变更。

- **平均修复时间（MTTR）!!!**

  高效能团队可以快速从系统故障中恢复（通常不到一个小时），而较低效能团队可能需要长达一周的时间才能从故障中恢复。       

​       平均恢复时间 (MTTR) 衡量**从部分服务中断或完全故障中恢复**所需的时间。这是一个重要的跟踪指标，无论中断是最近部署的结果还是孤立的系统故障。

​		从传统实践专注于故障之间的平均时间 (MTBF) 转变为专注于 MTTR。它反映了现代应用更高的复杂性，因此故障的预计发生率也会变高。MTTR 还加强了**持续学习和改进**的实践。团队不会等到部署“完美”来避免各种故障（从而重置旧的 MTBF 记分板），而是持续进行部署。MTTR 不会因为破坏了“完美”的 MTBF 记录而大加指责，而是**鼓励无指责的追溯，从而帮助团队改进上游流程和工具。**

**扩展：**

​		另一个相关指标是**周期时间**，即团队在项目准备交付之前工作花费的时间。在开发领域，周期时间是指从开发人员进行提交到将其部署到生产环节的时间。此关键 DevOps 指标有助于项目主管和工程经理更好地了解哪些方面在开发管道中效果良好。因此，他们可以更好地将自己的工作与利益相关者和客户的期望保持一致，确保团队可更快地完成交付。

​		项目主管可使用[周期时间报告](https://community.atlassian.com/t5/DevOps-Articles/Cycle-Time-amp-Deployment-Frequency-Reports/ba-p/1780375)为开发管道建立基准，用于评估未来的流程。当团队针对周期时间进行优化时，开发人员通常会减少正在进行的工作和低效的工作流程。

​		在精益产品管理中，重点关注[价值流映射](https://www.atlassian.com/zh/continuous-delivery/principles/value-stream-mapping)，即呈现从产品或功能概念到交付的这一流程。DevOps 指标为有效的价值流映射和管理提供了许多必不可少的数据点，但还应使用其他业务和产品指标进行增强，以便进行真正的端到端评估。例如，冲刺燃尽图可以洞察估算和规划流程的效率，而净推荐值则表明最终交付成果是否满足客户的需求。

**总结**

​		**持续改进是实践 DevOps 的团队的核心原则**。衡量和跟踪变更的提前期、变更失败率、部署频率和 MTTR 性能的能力，让团队能够加快速度并提高质量。

## 3 DevOps工具



# 集成 GitLab && JIRA 实现自动化 workflow

https://cloud.tencent.com/developer/article/1681335

1. GitLab 如何开启 JIRA 的入口？
2. GitLab 如何打通 JIRA 的信息流？
3. GitLab 如何自动化 JIRA 的工作流（workflow）？
4. GitLab 如何批量触发 JIRA 的工作量 ？



### **GitLab 如何自动化 JIRA 的工作流（workflow）？**

![img](https://ask.qcloudimg.com/http-save/yehe-4046479/puyn1b3ckt.png)

### **GitLab 如何批量触发 JIRA 的工作量 ？**

​		以上仅仅是对单个 Feature 的提交合并触发工作流，但是日常开发中这种场景比较少，很多 Feature 通常都是批量发布和上线，以我们目前的项目为例，我们目前最大的痛点是 Feature 上线后可以自动触发 Jira 的 On Line workflow，如果可以通过在 Release 合并进 Master 批量触发工作流将可以大大的简化我们目前的工作，提高效率。

![img](https://ask.qcloudimg.com/http-save/yehe-4046479/fibwrgqxsh.png)

​		在 GitLab 中有两种方式可以实现批量触发工作流，两种实现方式不同，但各有利弊：

- Release 分支通过 Merge Request 的 Description 批量添加 Closes issue id 实现
- Feature 分支通过本地 commit -m 'Closes issue id' 然后合并到默认分支实现（master）

##### **Release 分支通过 Merge Request 的 Description 批量添加 Closes issue id 实现**

​		这种操作实现起来对**项目经理和负责人要求会高一些**，需要事先整理和汇总所有要上线的分支和对应的 issue ，然后 GitLab 会在 Release -> Master 节点的时候通过 Merge 的时候自动触发 Jira 的工作流，那我们就需要在 Release 进行 Merge Request 的时候在合并描述 Description 添加触发关键字 **Closes Issue** 即可，具体如图所示：

![img](https://ask.qcloudimg.com/http-save/yehe-4046479/fvpmyw8p01.png)

**批量关闭 issue**

在负责人点击 Merge 对应的 issue 就会自动触发 Jira 工作流，流转对应的状态。

##### **Feature 分支通过本地 commit -m 'Closes issue id' 然后合并到默认分支实现（master）**

​		这种操作实现起来对**开发人员要求会高一些**，要求开发人员遵循规范，在完成 Feature 分支功能开发后，按照规范提交 commit 关键字来触发工作流，具体如下：

```javascript
git checkout phoenix/feature/TEST-223
git commit -m 'Closes TEST-223'
```

**这种方式的好处是项目负责人不需要提前收集和整理 issue，**也不需要在 Release 进行 Merge Request 的时候在合并描述 Description 添加触发关键字，直接提交 Release 分支合并即可，具体如下：

![img](https://ask.qcloudimg.com/http-save/yehe-4046479/haxu5flvkz.png)

它的工作原理是 GitLab 会自动在 Feature 分支的 commit log 找到触发关键字然后执行自动化工作流，点击 Merge 后通过 issue 链接跳转过去就会发现 Jira 的状态已经更新，非常方便，虽然两种方式最终实现的效果都是一样的，但是我个人比较推荐使用第二种方式，比较方便不说，而且可以培养开发人员的规范意识也是比较好的





# 全链路测试

​		**全链路测试**是指通过一次测试尽可能将系统的所有业务链路全部验证完成。

> **什么是全链路？**

​		**链路主要分为调用链路与业务链路**两种，**调用链路**主要是指从请求发出到结果返回所途径的各层应用/服务所产生的路径，这种形式主要是应用于单个系统或业务场景，比如登录服务、购物车查询等主要验证的是某一业务场景的应用、缓存、数据库等各层表现；而**业务链路**是指多个有业务关联的场景组合所产生的**调用链路集合**，比如商品详情+购物车+提交订单这样的业务链路组合。

- **调用链路**：主要是适合服务工场接口测试；
- **业务链路**：特别适合历程 > 服务工场的场景；





# xray

一款完善的安全评估工具，支持常见 web 安全问题扫描和自定义 poc | 使用之前务必先阅读文档

https://github.com/chaitin/xray

## 检测模块

新的检测模块将不断添加

- **XSS漏洞检测 (key: xss)**

  利用语义分析的方式检测XSS漏洞

- **SQL 注入检测 (key: sqldet)**

  支持报错注入、布尔注入和时间盲注等

- **命令/代码注入检测 (key: cmd-injection)**

  支持 shell 命令注入、PHP 代码执行、模板注入等

- **目录枚举 (key: dirscan)**

  检测备份文件、临时文件、debug 页面、配置文件等10余类敏感路径和文件

- **路径穿越检测 (key: path-traversal)**

  支持常见平台和编码

- **XML 实体注入检测 (key: xxe)**

  支持有回显和反连平台检测

- **poc 管理 (key: phantasm)**

  默认内置部分常用的 poc，用户可以根据需要自行构建 poc 并运行。文档：https://docs.xray.cool/#/guide/poc

- **文件上传检测 (key: upload)**

  支持常见的后端语言

- **弱口令检测 (key: brute-force)**

  社区版支持检测 HTTP 基础认证和简易表单弱口令，内置常见用户名和密码字典

- **jsonp 检测 (key: jsonp)**

  检测包含敏感信息可以被跨域读取的 jsonp 接口

- **ssrf 检测 (key: ssrf)**

  ssrf 检测模块，支持常见的绕过技术和反连平台检测

- **基线检查 (key: baseline)**

  检测低 SSL 版本、缺失的或错误添加的 http 头等

- **任意跳转检测 (key: redirect)**

  支持 HTML meta 跳转、30x 跳转等

- **CRLF 注入 (key: crlf-injection)**

  检测 HTTP 头注入，支持 query、body 等位置的参数

- **Struts2 系列漏洞检测 (高级版，key: struts)**

  检测目标网站是否存在Struts2系列漏洞，包括s2-016、s2-032、s2-045等常见漏洞

- **Thinkphp系列漏洞检测 (高级版，key: thinkphp)**

  检测ThinkPHP开发的网站的相关漏洞

- ..