**孙兴松 - 资深Rust工程师/全栈工程师**
📞 18698600513 | 📧 [sunxs@outlook.com](mailto:sunxs@outlook.com) | 🌍 辽宁大连

------

### **技术亮点**

- **Rust**：4年生产级Rust开发经验（2022.02 - 至今），主导云联络中心实时通信中后台体系（Rust）。异步编程（Tokio）、Actor 并发模型（actix / kameo，以消息传递替代共享锁）、流式 gRPC 通信（Tonic）、WebSocket/SSE 实时推送与自定义二进制 TCP 协议；全体系**零 `unsafe`**，从语言层面消除数据竞争。
- **全栈技术融合**：8年+ Java生态经验（Spring Boot)，熟悉Vue/Angular前端开发，具备跨技术栈系统集成能力。
- **架构设计**：Kubernetes云原生架构经验，熟悉微服务、多协议网关、分布式系统设计与优化。
- **工程化**：Cargo Workspace 多 crate 分层架构、`build.rs` 编译期 Protobuf 代码生成、类型化错误体系、`tracing` 日志运行时热重载、LTO + musl 静态编译精简镜像。

------

### **核心项目经历**

#### **OneCX 全渠道呼叫中心系统**（2022.02 - 至今）

**技术负责人** | OneCX Solutions (亚美亚 / Avaya)

基于 Rust + Kubernetes 构建云联络中心实时通信中后台体系，覆盖坐席接入、CTI 信令翻译、全渠道智能路由与授权管控。整体采用 **Tokio 异步 + Actor 并发模型**，以消息传递替代共享锁，全程零 `unsafe`；单进程内用 `tokio::try_join!` 统一编排多协议入口并实现信号优雅停机。负责以下核心服务：

- **CTI 实时通信网关（Rust）**：将每条坐席 WebSocket 建模为独立有状态 Actor，承载 53 类前端命令、41 项呼叫控制操作（监听/强插/密语/会议/咨询/转接等）与 140+ 消息处理器；构建 **WebSocket + gRPC + 自定义二进制 TCP 三端异构协议网关**，打通语音、聊天、邮件、WebRTC 全渠道呼叫控制。消费 gRPC server-streaming 电话事件流并实现退避式自愈重连；用 `tokio::sync::Notify` 构建无锁事件背压门控，应对坐席登录瞬时事件洪峰。
- **全渠道路由引擎 ACD（ Rust）**：以无共享锁的 Actor 架构（147 消息处理器 / 264 消息类型）实现实时坐席分配；设计**优先级队列 + 技能路由 + 并发负载权重**的匹配算法实现坐席软负载均衡与「末位坐席亲和」，并基于 `oneshot` channel + 异步超时实现 RONA 振铃无应答自动改派。
- **坐席统一网关（9-crate Cargo Workspace）**：单 Actor 聚合双路上游 WebSocket，经 **SSE** 单向推送浏览器；自研「单调序列号 + 有界环形缓冲 + Last-Event-ID 重放 + Epoch 代际计数」机制，实现断线重连下的**至少一次（at-least-once）事件投递**并消解陈旧下线任务竞态；针对 HTTP/2 反向代理缓冲 SSE 的问题设计哨兵 flush 补丁。
- **授权管理微服务（REST + gRPC 双协议）**：实现 **Node-Locking 防盗版**（SHA3-256 内容签名 + 物理机 MAC / Kubernetes 节点 machine-id 机器指纹绑定 + 启动期集合运算对账清理失效授权）；基于 Tokio 定时任务构建**浮动席位心跳续约与自动回收引擎**（按时间戳判定超时席位、sled 事务原子回收），实现客户端异常退出后席位自动释放；采用 sled 嵌入式 KV + bincode 设计零外部依赖存储层。
- **共性基础设施**：用枚举统一抽象 **Redis 单机 / Cluster / Sentinel 三种拓扑**（含 Sentinel 主从读写分离），并以 **Redis Streams** 实现跨实例坐席状态广播与强制登出协调；自研声明式宏（命令路由表 / 跨数据库方言分页）与 `build.rs` 编译期代码生成消除样板代码。

#### **号码盾牌系统**（2021.07 - 2022.01）

**全栈工程师** | 亚美亚

- **核心功能**：
  - **动态号码修改**：拦截通话后实时修改主叫/被叫号码，支持自定义号码规则（如前缀替换、随机生成）。
  - **黑白名单拦截**：基于策略引擎实现智能拦截，支持号码黑名单（自动拒接）与白名单（优先放行），拦截准确率99.8%。
  - **外呼与报表**：集成外呼功能，日均处理10万+请求；提供实时报表展示拦截统计与通话记录。
- **技术实现**：
  - **后端**：基于RuoYi框架（Spring Boot + MyBatis）开发，结合PostgreSQL存储策略规则与通话日志。
  - **前端**：使用Vue + ElementUI实现动态策略配置与数据可视化大屏。

#### **多媒体网关系统**（2018.11 - 2019.11）

**全栈工程师** | 亚美亚

- **核心功能**：
  - **多平台消息集成**：支持**微信、WhatsApp、Facebook、Twitter 、LINE**等渠道的消息收发，日均处理50万+条消息。
  - **统一协议适配**：抽象消息模型，兼容各平台API差异（如JSON/XML格式、OAuth2认证），开发适配层降低维护成本30%。
  - **高性能消息队列**：基于AWS SQS + RabbitMQ优化消息分发，吞吐量提升至20k/s，延迟<100ms。
- **技术实现**：
  - **后端**：Spring Boot + Spring Cloud Consul（服务治理） + Redis（会话状态管理）。
  - **前端**：Vue + ElementUI实现多平台配置管理与实时监控大屏。
  - **安全优化**：集成JWT鉴权与TLS加密，拦截非法请求率99.5%。

------

### **技能专长**

| **编程语言** | Rust（4年，异步生态）｜Java（8年+）｜TypeScript |
| **核心框架** | Actix / Kameo（Actor模型）｜Tokio（异步运行时）｜Tonic（gRPC）｜poem / Actix-web（Web）｜Spring Boot｜Vue |
| **关键Rust库** | Serde（序列化）｜Prost / tonic-build（Protobuf代码生成）｜thiserror / anyhow（错误处理）｜Tracing（结构化日志）｜rbatis（异步ORM）｜tokio-tungstenite（WebSocket）｜bincode |
| **协议与通信** | gRPC（含 server-streaming 流式）｜WebSocket｜SSE｜REST｜自定义二进制 TCP 帧协议（粘包/半包处理） |
| **数据库** | PostgreSQL｜Redis（单机/Cluster/Sentinel）｜Sled（嵌入式KV）｜SQL Server｜MySQL |
| **云原生** | Kubernetes｜Docker｜AWS/GCP｜Prometheus/Grafana |
| **开发方法论** | 敏捷开发（Scrum）｜CI/CD（GitHub Actions）｜代码审计（Sonar/Coverity） |

------

### **早期项目亮点**（2011 - 2020）

- **实时数据大屏**：基于JHipster（Spring Boot + Angular）实现千人级坐席实时监控，支持动态报表编辑。
- **招商银行IVR系统**：日均处理50万+语音交互，系统可用性99.99%。

------

### **教育背景**

**大连东软信息学院** | 软件技术（专科） | 2007.09 - 2010.06

👉 [📄 点击下载/预览 PDF 版简历](./孙兴松_18698600513.pdf)

