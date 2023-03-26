### Apollo

> Apollo（阿波罗）是携程2016年5月开源一款可靠的分布式配置管理中心，能够集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性，适用于微服务配置管理场景

#### 架构图

<img src="https://img2020.cnblogs.com/blog/1090617/202007/1090617-20200728215539079-1535962969.jpg"/>

#### 核心模块及其主要功能

1. **ConfigService**

   - 提供配置获取接口
   - 提供配置推送接口
   - 服务于Apollo客户端

2. **AdminService**

   - 提供配置管理接口
   - 提供配置修改发布接口
   - 服务于管理界面Portal

3. **Client**

   - 为应用获取配置，支持实时更新

   - 通过MetaServer获取ConfigService的服务列表
   - 使用客户端软负载SLB方式调用ConfigService

4. **Portal**

   - 配置管理界面

   - 通过MetaServer获取AdminService的服务列表
   - 使用客户端软负载SLB方式调用AdminService

   

### Nacos

> 阿里2018年开源的一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台，Nacos 致力于发现、配置和管理微服务。并提供了一组简单易用的特性集，快速实现**动态服务发现、服务配置、服务元数据及流量管理**。

#### 架构图

<img src='https://cdn.nlark.com/yuque/0/2019/jpeg/338441/1561217892717-1418fb9b-7faa-4324-87b9-f1740329f564.jpeg'/>



#### 组件

<img src='https://cdn.nlark.com/yuque/0/2022/png/25601973/1646715315872-7ee3679a-e66e-49e9-ba9f-d24168a86c14.png'/>

- 服务管理：实现服务CRUD，域名CRUD，服务健康状态检查，服务权重管理等功能
- **配置管理：实现配置管CRUD，版本管理，灰度管理，监听管理，推送轨迹，聚合数据等功能**
- 元数据管理：提供元数据CURD 和打标能力
- 插件机制：实现三个模块可分可合能力，实现扩展点SPI机制
- 事件机制：实现异步化事件通知，sdk数据变化异步通知等逻辑
- 日志模块：管理日志分类，日志级别，日志可移植性（尤其避免冲突），日志格式，异常码+帮助文档
- 回调机制：sdk通知数据，通过统一的模式回调用户处理。接口和数据结构需要具备可扩展性
- 寻址模式：解决ip，域名，nameserver、广播等多种寻址模式，需要可扩展
- 推送通道：解决server与存储、server间、server与sdk间推送性能问题
- 容量管理：管理每个租户，分组下的容量，防止存储被写爆，影响服务可用性
- 流量管理：按照租户，分组等多个维度对请求频率，长链接个数，报文大小，请求流控进行控制
- 缓存机制：容灾目录，本地缓存，server缓存机制。容灾目录使用需要工具
- 启动模式：按照单机模式，配置模式，服务模式，dns模式，或者all模式，启动不同的程序+UI
- 一致性协议：解决不同数据，不同一致性要求情况下，不同一致性机制
- 存储模块：解决数据持久化、非持久化存储，解决数据分片问题
- Nameserver：解决namespace到clusterid的路由问题，解决用户环境与nacos物理环境映射问题
- CMDB：解决元数据存储，与三方cmdb系统对接问题，解决应用，人，资源关系
- Metrics：暴露标准metrics数据，方便与三方监控系统打通
- Trace：暴露标准trace，方便与SLA系统打通，日志白平化，推送轨迹等能力，并且可以和计量计费系统打通
- 接入管理：相当于阿里云开通服务，分配身份、容量、权限过程
- 用户管理：解决用户管理，登录，sso等问题
- 权限管理：解决身份识别，访问控制，角色管理等问题
- 审计系统：扩展接口方便与不同公司审计系统打通
- 通知系统：核心数据变更，或者操作，方便通过SMS系统打通，通知到对应人数据变更
- OpenAPI：暴露标准Rest风格HTTP接口，简单易用，方便多语言集成
- Console：易用控制台，做服务管理、配置管理等操作
- SDK：多语言sdk
- Agent：dns-f类似模式，或者与mesh等方案集成
- CLI：命令行对产品进行轻量化管理，像git一样好用

### Disconf

> Disconf是一套完整的基于zookeeper的分布式配置统一解决方案，它通过disconf-web管理配置信息，然后将配置的key在Zookeeper上建立节点，disconf-client启动后拉取自身需要的配置信息并监听Zookeeper的节点。在web上更新配置信息会触发zk节点状态的变动，client可以实时感知到变化，然后从web上拉取最新配置信息

#### 架构图

<img src='http://img.jbzj.com/file_images/article/202203/2022030811404002.jpg'/>

#### 各模块功能

- **Disconf-core**
  - 分布式通知模块：支持配置更新的实时化通知
  - 路径管理模块：统一管理内部配置路径URL
- **Disconf-client**
  - 配置仓库容器模块：统一管理用户实例中本地配置文件和配置项的内存数据存储
  - 配置reload模块：监控本地配置文件的变动，并自动reload到指定bean
  - 扫描模块：支持扫描所有disconf注解的类和域
  - 下载模块：restful风格的下载配置文件和配置项
  - watch模块：监控远程配置文件和配置项的变化
  - 主备分配模块：主备竞争结束后，统一管理主备分配与主备监控控制
  - 主备竞争模块：支持分布式环境下的主备竞争
- **Disconf-web**
  - 配置存储模块：管理所有配置的存储和读取
  - 配置管理模块：支持配置的上传、下载、更新
  - 通知模块：当配置更新后，实时通知使用这些配置的所有实例
  - 配置自检监控模块：自动定时校验实例本地配置与中心配置是否一致
  - 权限控制：web的简单权限控制
- **Disconf-tools**
  - context共享模块：提供多实例间context的共享

### 核心概念的对比

> Apollo、Nacos、Disconf在配置管理领域的概念基本相同，但是也存在一些不同的点，使用配置的过程中会涉及到一些比较重要的概念

#### 应用

应用是客户端系统的基本单位，Apollo的配置都是在某个应用下面的（除了公共配置），也起到了多个应用配置相互隔离的作用。Nacos的应用概念比较弱，只有一个用于区分配置的额外属性，不过可以使用 Group 来做应用字段，可以起到隔离作用。Disconf的配置项都是基于某个应用的某个环境，应用与环境相互隔离。

#### 集群

不同的环境可以搭建不同的集群，这样可以起到物理隔离的作用，Apollo、Nacos、Disconf都支持多个集群。

#### 环境 & 命名空间

Nacos的命名空间和Apollo与Disconf的环境一样，是一个逻辑概念，可以作为环境逻辑隔离。Apollo中的命名空间指配置的名称，具体的配置项指配置文件中的一个Property。

### 配置管理功能对比

#### 灰度发布

> 灰度发布是当配置的变更影响比较大的时候，需要先在部分应用实例中验证配置的变更是否符合预期，然后再推送到所有应用实例

Apollo可以直接在控制台上点灰度发布指定发布机器的IP，接着再全量发布，做得比较体系化。 Nacos修改配置时可勾选Beta发布指定IP进行灰度发布。Disconf不支持灰度发布

#### 权限管理

> 配置的变更和代码变更都是对应用运行逻辑的改变，对于配置变更的权限管控和审计能力是配置中心重要的功能

Apollo通过项目的维度来对配置进行权限管理，一个项目的owner可以授权给其他用户配置的修改发布权限。

Nacos通过命名空间对配置读写操作进行权限管理，可定义不同角色对命名空间的资源进行操作。

Disconf不支持权限管理。

#### 版本管理&回滚

> 当配置变更不符合预期的时候，需要根据配置的发布版本进行回滚

Apollo和Nacos都具备配置的版本管理和回滚能力，可以在控制台上查看配置的变更情况或进行回滚操作。Disconf配置有版本概念但不支持回滚。

#### 配置格式校验

> 应用的配置数据存储在配置中心一般都会以一种配置格式存储，比如Properties、Json、Yaml等，如果配置格式错误，会导致客户端解析配置失败引起生产故障，配置中心对配置的格式校验能够有效防止人为错误操作的发生。 

Apollo和Nacos都会对配置格式的正确性进行检验，可以有效防止人为错误。Disconfig无文件格式校验，支持任意类型配置文件(.properties文件可支持自动注入,非.properties文件则只是简单托管)

#### 监听查询

> 当排查问题或者进行统计的时候，需要知道一个配置被哪些应用实例使用到，以及一个实例使用到了哪些配置。


Apollo可以通过灰度实例列表查看监听配置的实例列表，但实例监听的配置(Apollo称为命名空间)目前还没有展示出来。

Nacos可以查看监听配置的实例，也可以查看实例监听的配置情况。Nacos使用起来相对简单，易用性相对更好些。

Disconf可以查看成功监听配置的实例

#### 多环境

> 在实际生产中，配置中心常常需要涉及多环境或者多集群，业务在开发的时候可以将开发环境和生产环境分开，或者根据不同的业务线存在多个生产环境

Apollo可在控制台创建配置的时候就要指定配置所在的环境，客户端在启动的时候指定JVM参数ENV来访问对应环境的配置文件。

Nacos通过命名空间来支持多环境，每个命名空间的配置相互隔离，客户端指定想要访问的命名空间就可以达到逻辑隔离的作用。

Disconf支持多环境配置切换，应用与环境相互隔离，互不影响。

#### 多集群

> 当对稳定性要求比较高，不允许各个环境相互影响的时候，需要将多个环境通过多集群的方式进行物理隔离。

Apollo可以搭建多套集群，Apollo的控制台和数据更新推送服务分开部署，控制台部署一套就可以管控多个集群。

Nacos控制台和后端配置服务是部署在一起的，可以通过不同的域名切换来支持多集群。

Disconf可将disconf-web集群部署，disconf-client内嵌在各个项目中

### 配置实时推送的对比

> 当配置变更的时候，配置中心需要将配置实时推送到应用客户端。

Nacos和Apollo配置推送都是基于HTTP长轮询，客户端和配置中心建立HTTP长联接，当配置变更的的时候，配置中心把配置推送到客户端。Disconf配置是基于zookeeper监听机制实现配置变更通知，接受到通知后客户端主动向服务端获取最新配置。

### 部署结构 & 高可用的对比

#### Apollo

Apollo分为MySQL，Config Service，Admin Service，Portal四个模块：

- MySQL存储Apollo元数据和用户配置数据;
- Config Service提供配置的读取、推送等功能，客户端请求都是落到Config Service上;
- Admin Service提供配置的修改、发布等功能，Portal操作的服务就是Admin Service;
- Portal提供给用户配置管理界面;

在生产环境使用Apollo，Portal可以两个节点单独部署，稳定性要求没那么高的话，Config Service和Admin Service可以部署在一起，数据库支持主备容灾。

#### Nacos

Nacos部署需要Nacos Service和MySQL：

- Nacos对外提供服务，支持配置管理和服务发现;
- MySQL提供Nacos的数据持久化存储;

单机模式下，Nacos可以使用嵌入式数据库部署一个节点，就能启动。生产环境使用Nacos，Nacos服务需要至少部署三个节点，再加上MySQL主备容灾。

#### Disconf

Disconf部署为前后端分离项目，后端基于MySQL、Redis、Tomcat、Zookeeper部署，前端基于Nginx部署静态资源

#### 整体情况

- Nacos与Disconf的部署结构比较简单，运维成本较低。Apollo部署组件较多，运维成本比Nacos高。

- Apollo容器化较困难，Nacos有官网的镜像可以直接部署。

- Apollo相对于Nacos在配置管理做的更加全面，不过配置较麻烦。
- Nacos使用相对比较简洁，在对性能要求比较高的大规模场景更适合。
- Disconf功能相对简洁，可基于此项目进行二次开发
- Nacos除了提供配置中心的功能，还提供了动态服务发现、服务共享与管理的功能，降低了服务化改造过程中的难度。

### 功能比对矩阵

> Apollo使用1.2.0 release版本，Nacos使用0.5版本

<table>
  <thead>
    <tr>
      <th>功能点</th>
      <th>Apollo</th>
      <th>Nacos</th>
      <th>Disconf</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>配置实时推送</td>
      <td>支持(HTTP长轮询1s内)</td>
      <td>支持(HTTP长轮询1s内)</td>
      <td>支持</td>
    </tr>
    <tr>
      <td>版本管理</td>
      <td>支持</td>
      <td>支持</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>配置回滚</td>
      <td>支持</td>
      <td>支持</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>灰度发布</td>
      <td>支持</td>
      <td>支持&#xff08;不太好用&#xff09;</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>权限管理</td>
      <td>支持</td>
      <td>支持</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>审计</td>
      <td>支持</td>
      <td>不支持</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>多集群</td>
      <td>支持</td>
      <td>支持</td>
      <td>支持</td>
    </tr>
    <tr>
      <td>多环境</td>
      <td>支持</td>
      <td>支持</td>
      <td>支持</td>
    </tr>
    <tr>
      <td>client本地缓存</td>
      <td>支持</td>
      <td>支持</td>
      <td>支持</td>
    </tr>
    <tr>
      <td>监听查询</td>
      <td>支持</td>
      <td>支持</td>
      <td>支持</td>
    </tr>
    <tr>
      <td>运维成本</td>
      <td>中等</td>
      <td>较低</td>
      <td>较低</td>
    </tr>
    <tr>
      <td>通信协议</td>
      <td>HTTP</td>
      <td>HTTP</td>
      <td>Zookeeper、HTTP</td>
    </tr>
    <tr>
      <td>配置格式校验</td>
      <td>支持</td>
      <td>支持</td>
      <td>不支持</td>
    </tr>
    <tr>
      <td>单机读(QPS)</td>
      <td>9000</td>
      <td>15000</td>
      <td></td>
    </tr>
    <tr>
      <td>单击写(QPS)</td>
      <td>1100</td>
      <td>1800</td>
      <td></td>
    </tr>
    <tr>
      <td>3节点读(QPS)</td>
      <td>27000</td>
      <td>45000</td>
      <td></td>
    </tr>
    <tr>
      <td>3节点写(QPS)</td>
      <td>3300</td>
      <td>5600</td>
      <td></td>
    </tr>
  </tbody>
</table>



### 系统示例

- [Nacos](http://console.nacos.io/nacos/index.html#/login)
  - 用户：nacos
  - 密码：nacos

- [Apollo](http://81.68.181.139/signin)
  - 用户：apollo
  - 密码：admin

### 参考文档

- [Nacos](https://nacos.io/zh-cn/docs/v2/quickstart/quick-start.html)
- [Apollo](https://www.apolloconfig.com/#/zh/README)
- [Disconf](https://disconf.readthedocs.io/zh_CN/latest/index.html)