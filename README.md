# 孙兴松

**资深 Rust 工程师｜实时通信 / 分布式系统**

电话：18698600513｜邮箱：[sunxs@outlook.com](mailto:sunxs@outlook.com)｜所在地：辽宁大连

16 年以上软件研发经验，近 4.5 年专注 Rust，擅长实时通信、异步并发、分布式系统和云原生微服务。主导云联络中心 CTI 网关、ACD 路由引擎、坐席统一网关及授权服务建设，具备从架构设计、核心开发到生产交付的完整经验，并拥有 11 年以上 Java 生态开发经验。

------

### **技术亮点**

- **Rust**：近 4.5 年生产级 Rust 开发经验（2022.02 - 2026.08），主导云联络中心实时通信中后台体系建设。熟悉 Tokio 异步编程、Actor 并发模型（Actix / Kameo）、Tonic 流式 gRPC、WebSocket / SSE 实时推送及自定义二进制 TCP 协议；自研业务代码未使用 `unsafe`，利用 Rust 类型系统降低内存安全与数据竞争风险。
- **全栈技术融合**：11 年+ Java 生态经验（Spring Boot、Spring Cloud），熟悉 Vue / Angular 前端开发，具备跨技术栈系统集成能力。
- **架构设计**：具备 Kubernetes 云原生架构经验，熟悉微服务、多协议网关及分布式系统设计与优化。

------

### **核心项目经历**

#### **OneCX 全渠道呼叫中心系统**（2022.02 - 2026.08）

**技术负责人** | 亚美亚 ( 大连 )

基于 Rust + Kubernetes 构建云联络中心实时通信中后台体系，覆盖坐席接入、CTI 信令翻译、全渠道智能路由与授权管控。整体采用 **Tokio 异步 + Actor 并发模型**，以消息传递隔离并发状态，自研业务代码未使用 `unsafe`；单进程内通过 `tokio::try_join!` 统一编排多协议入口并实现信号优雅停机。负责以下核心服务：

- **CTI 实时通信网关（Rust）**：将每条坐席 WebSocket 建模为独立有状态 Actor，构建 **WebSocket + gRPC + 自定义二进制 TCP 三端异构协议网关**，支持监听、强插、密语、会议、咨询、转接等呼叫控制能力，打通语音、聊天、邮件和 WebRTC 全渠道呼叫控制。消费 gRPC server-streaming 电话事件流并实现退避式自愈重连；通过 `tokio::sync::Notify` 协调事件处理流程，应对坐席登录时的瞬时事件洪峰。
- **全渠道路由引擎 ACD（Rust）**：以 Actor 模型隔离坐席、任务与路由状态，实现实时坐席分配；设计**优先级队列 + 技能路由 + 并发负载权重**匹配算法，实现坐席软负载均衡与「末位坐席亲和」，并基于 `oneshot` channel 和异步超时机制实现 RONA 振铃无应答自动改派。
- **坐席统一网关（9-crate Cargo Workspace）**：单 Actor 聚合双路上游 WebSocket，经 **SSE** 单向推送浏览器；自研「单调序列号 + 有界环形缓冲 + Last-Event-ID 重放 + Epoch 代际计数」机制，在缓冲窗口内实现断线重连后的**至少一次（at-least-once）事件投递**，并消解陈旧下线任务竞态；针对 HTTP/2 反向代理缓冲 SSE 的问题设计哨兵 flush 补丁。
- **授权管理微服务（REST + gRPC 双协议）**：实现 **Node-Locking 防盗版**（SHA3-256 内容完整性校验 + 物理机 MAC / Kubernetes 节点 machine-id 机器指纹绑定 + 启动期集合运算对账清理失效授权）；基于 Tokio 定时任务构建**浮动席位心跳续约与自动回收引擎**（按时间戳判定超时席位、sled 事务原子回收），实现客户端异常退出后席位自动释放；采用 sled 嵌入式 KV + bincode 构建无需外部数据库服务的存储层。
- **共性基础设施**：用枚举统一抽象 **Redis 单机 / Cluster / Sentinel 三种拓扑**（含 Sentinel 主从读写分离），并以 **Redis Streams** 实现跨实例坐席状态广播与强制登出协调；自研声明式宏（命令路由表 / 跨数据库方言分页）与 `build.rs` 编译期代码生成消除样板代码。

#### **号码盾牌系统**（2021.07 - 2022.01）

**技术负责人** | 亚美亚( 大连 )

- **核心功能**：
  - **动态号码修改**：拦截通话后实时修改主叫/被叫号码，支持自定义号码规则（前缀替换、随机生成）；基于 Spring Boot + Spring Cloud 构建后端服务，使用 **MyBatis + PostgreSQL** 存储策略规则与通话日志，保障规则配置的灵活性与数据可靠性。
  - **智能黑白名单拦截**：基于策略引擎实现号码黑名单自动拒接与白名单优先放行。
  - **外呼与实时报表**：集成外呼功能，系统日均处理 **10 万+**次请求；前端使用 **Vue + ElementUI** 实现动态策略配置与数据可视化大屏，实时展示拦截统计与通话记录。
- **技术栈**：Spring Boot、Spring Cloud、MyBatis、PostgreSQL、Vue、ElementUI

#### **电子保函交易平台**（2020.11 - 2021.06）

**技术负责人** | 中保函( 大连 )

- **核心功能**：
  - **保函申请全流程管理**：客户注册后根据项目选择担保公司在线申请项目保函，系统自动将申请请求分发至对应担保公司，担保公司在规定时限内反馈申请结果并生成电子保函，实现保函业务**全流程线上化**。
  - **多担保公司对接**：设计统一的申请分发机制，支持客户自主选择担保公司，平台与多家担保公司系统对接，提升业务撮合效率。
  - **系统架构与部署**：基于**Spring Boot + Spring Cloud** 微服务架构构建系统，**MyBatis + MySQL** 实现数据持久化，使用 **Redis** 缓存热点数据与会话管理，基于**阿里云**（ECS、RDS、OSS、SLB）完成应用发布与部署。
  - **前端与权限管理**：前端采用 **Vue + iView** 实现客户注册、保函申请、进度查询等页面；利用 RuoYi 自带的 **RBAC 权限体系**实现客户、担保公司、平台管理员多角色权限隔离。
- **技术栈**：Java、Spring Boot、Spring Cloud、MyBatis、RuoYi、Redis、MySQL、Vue、iView、Axios、ECharts

#### **联络中心数据展示平台**（2018.11 - 2020.10）

**技术负责人** | 亚美亚( 大连 )

- **核心功能**：
  - **实时数据采集**：通过 JTAPI / TSAPI、Socket 接入 Avaya AES、CMS、RTA / RTS，并将事件发送至 Kafka。
  - **流式指标计算**：基于 Kafka Streams 的窗口聚合与状态存储，计算坐席状态、技能组指标、通话记录及半小时统计数据。
  - **多维报表分析**：采用元数据驱动方式动态生成查询，支持字段选择、条件过滤、分组汇总、公式计算和数据脱敏。
  - **周期统计汇总**：消费 Kafka 结果并持久化，通过定时任务生成日、周、月、季度和年度报表。
  - **实时监控与告警**：定时检测坐席及技能组指标，支持两级阈值和公式告警，并通过 WebSocket / SSE 实时推送。
  - **看板与报表导出**：提供可配置监控布局和可视化图表，使用 Apache POI 生成多语言 Excel 报表。
  - **容器化部署**：各服务通过 Docker 独立部署，并使用 Kubernetes、Helm 及 ZooKeeper / Curator 支持集群管理和高可用。
- **技术栈**：Java、Spring Boot、JHipster、Spring Security、Spring Data JPA、Hibernate、Kafka、Kafka Streams、Spring Cloud Stream、Redis、PostgreSQL、Liquibase、WebSocket、SSE、Angular、TypeScript、Chart.js、D3、Apache POI、Go、Docker、Kubernetes、Helm、ZooKeeper。

#### **多媒体网关系统**（2016.12 - 2018.10）

**全栈工程师** | 亚美亚( 大连 )

- **核心功能**：
  - **多平台消息集成**：支持**微信、WhatsApp、Facebook、Twitter、LINE**等渠道的消息收发，日均处理 **50 万+**条消息；抽象统一消息模型，兼容各平台 API 在数据格式和 OAuth2 认证方式上的差异，通过适配层降低渠道接入与维护复杂度。
  - **高性能消息架构**：基于 **AWS SQS + RabbitMQ** 优化消息分发，采用 **Spring Cloud Consul** 实现服务治理，使用 **Redis** 管理会话状态，保障高并发场景下的消息可靠投递。
  - **全栈功能开发**：后端基于 **Spring Boot** 构建微服务，前端使用 **Vue + ElementUI** 实现多平台配置管理与实时监控大屏，支撑业务方可视化运维。
  - **安全体系构建**：集成 **JWT 鉴权**与 **TLS 加密**，实现请求合法性校验与传输链路加密。
- **技术栈**：Spring Boot、Spring Cloud Consul、RabbitMQ、AWS SQS、Redis、JWT、Vue、ElementUI、OAuth2

#### **招商银行信用卡中心 IVR 系统**（2016.01 - 2016.11）

**全栈工程师** | 亚美亚( 大连 )

- **核心功能**：
  - **动态 IVR 流程**：菜单、语音播放、按键收集、语言切换、条件跳转及异常处理
  - **智能语音交互**：支持 ASR、TTS、DTMF、智能对话及动态变量播报
  - **呼叫路由与转接**：支持动态路由、排队信息查询，以及 AES / SIP 坐席与 IVR 转接
  - **客户上下文管理**：集成 Oceana、Context Store、Customer Journey，同步客户、坐席和菜单轨迹
  - **多语言服务**：支持中文、英文、粤语及印度区域语言等约 50 种语言资源
  - **呼入与外呼管理**：支持客户分组、时间计划、外呼任务和呼叫结果记录
  - **数据追踪与容灾**：记录通话、节点轨迹、交易及录音；数据库异常时降级写文件并支持数据恢复
  - **配置化运维**：数据库动态加载流程、路由、语音和系统参数，并支持缓存更新
- **技术栈**：Java、Avaya Orchestration Designer、VXML、Servlet / JSP、Tomcat、Maven、JDBC / JNDI、PostgreSQL、SQL Server、RESTful API、Apache HttpClient、CTI / AES、SIP、ASR / TTS、Log4j

#### **SingTel 呼叫中心统计报表**（2015.08 - 2015.12）

**全栈工程师** | 亚美亚( 大连 )

- **核心功能**：实现用户管理、角色管理、报表展示和 CSV 文件导出。
- **技术栈**：Struts2、Spring、Hibernate、JSP、jQuery、Microsoft SQL Server。

#### **太古可口可乐呼叫中心统计报表**（2015.02 - 2015.07）

**报表开发工程师** | 亚美亚( 大连 )

- **核心工作**：基于 SQL Server Reporting Services 定制呼叫中心历史报表。
- **报表范围**：电话流量汇总、分时段流量、无效通话明细、话后调研统计与明细、跨厂支持及紧急备份话务流量汇总。
- **技术栈**：T-SQL（复杂查询、窗口函数、存储过程、查询优化）、SSRS 报表开发（Tablix、参数化报表、钻取/下钻、子报表、表达式）

#### **星展银行呼叫中心项目**（2014.07 - 2015.01）

**实施开发工程师** | 亚美亚( 大连 )

- 负责亚美亚呼叫中心产品的维护、升级与实施。

#### **SP Faults Web 故障提示公告程序**（2014.02 - 2014.06）

**全栈工程师** | 亚美亚( 大连 )

- **核心功能**：实现 XML 文件导入、用户管理、角色管理、短信故障通告和大屏故障展示。
- **技术栈**：Spring MVC、JDBC、JSP、jQuery、MySQL，并通过 Linux Crontab 定时运行 Shell 脚本生成 CSV 统计文件。

#### **交通银行呼叫中心 IVR 系统**（2013.09 - 2014.01）

**实施工程师** | 亚美亚

- 负责呼叫中心 IVR 系统实施，包括 Red Hat Linux 安装、语音流程系统、语音路由系统、数据缓存系统、WebSphere 运行环境配置及系统测试。

#### **业务支撑系统**（2011.07 - 2013.08）

**Java 开发工程师** | 东软集团（大连）

- **项目经历**：参与湖南广电 BOSS 系统订单模块、东软服务开通平台工作流引擎压力测试、联通网格营销系统 WorkFlow 二次封装，以及北京航天传媒 BOSS 系统资源和集成订单模块开发。
- **项目交付**：负责四川联通、陕西联通、江西联通、湖南广电、营口广电和省网广电等项目的资源模块、集成订单模块维护与需求开发。
- **技术栈**：Struts2、Java、JSP、HTML、CSS、JavaScript、JDBC、PL/SQL、SOAP WebService、Shell。

#### **飞行学院管理系统**（2010.06 - 2011.06）

**Java 开发工程师** | 大连海航科技

- 根据设计文档完成前后端功能开发，包括页面展示、后端业务逻辑和数据库增删改查。
- **技术栈**：Struts2、Spring、iBatis、ExtJS、Oracle。

------

### **技能专长**

- **编程语言**：Rust（近 4.5 年，异步生态）、Java（11 年+）、TypeScript / JavaScript、SQL / PL/SQL、Shell
- **核心框架**：Actix / Kameo（Actor 模型）、Tokio（异步运行时）、Tonic（gRPC）、Poem / Actix Web、Spring Boot / Spring MVC、Struts2、Hibernate / MyBatis / iBatis、Vue / Angular / ExtJS
- **关键 Rust 库**：Serde、Prost / tonic-build、thiserror / anyhow、tracing、rbatis、tokio-tungstenite、bincode
- **协议与通信**：gRPC（含 server-streaming）、WebSocket、SSE、REST、SOAP WebService、自定义二进制 TCP 帧协议（粘包 / 半包处理）
- **数据库与报表**：PostgreSQL、Redis（单机 / Cluster / Sentinel）、sled、Oracle、SQL Server / SSRS、MySQL
- **云原生与平台**：Kubernetes、Docker、AWS SQS、WebSphere、Linux / Windows / macOS
- **工程与协作**：敏捷开发（Scrum）、Git / GitHub、CI/CD（GitHub Actions / Bamboo）、JIRA、代码审计（Sonar / Coverity / Black Duck）

------

### **教育背景**

**大连东软信息学院** | 软件技术（专科） | 2007.09 - 2010.06

[下载 / 预览 PDF 版简历](./README.pdf)
