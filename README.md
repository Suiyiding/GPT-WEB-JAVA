<div align="center">
    <p style="font-size:25px;font-weight: 800;">GPT-WEB-CLIENT</p>
</div>
<div align="center" style="text-align:center;margin-top:30px;margin-bottom:20px">
   <a style="padding-left:10px"><img src="https://img.shields.io/github/stars/a616567126/GPT-WEB-JAVA"/></a>
   <a style="padding-left:10px"><img src="https://img.shields.io/github/forks/a616567126/GPT-WEB-JAVA?color=red&logo=red"/></a>  
   
   
</div>

# **Project Title**  

**[1.0体验地址](https://bot.v-wim.xyz/)**   
 
**基于Spring Boot  Mybatis-plus的GPTweb后台**  

# **2.0全新版本，全新ui，全新体验**  
# **可切换2.0分支拉取后台代码，完整代码添加作者咨询**
# **体验地址:https://ai.v-wim.xyz**
 
## Getting Started  

* [**JDK>=8**](golang_install_guide)
* [**MySql>=8.0**](golang_install_guide)
* [**Redis>=4.0**](golang_install_guide)
  
## Major Function
--客户端  

* **登录**
* **注册赠送10次对话（短信注册，公众号注册，邮箱注册，账号密码注册）**
* **对话记录**
* **画图**
* **流式对话**
* **公告查看**
* **个人信息展示（剩余次数，身份，昵称）**
* **产品查询购买（支持支付宝，微信，QQ钱包）**
* **订单查询**
* **支付 易支付，支付宝支付，微信支付**  
* **stable-diffusion画图**  
* **flagstudio画图**
* **Midjourney画图**


--管理端  

* **首页（数据统计）**
* **支付配置**
* **对KEY配置**
* **用户管理**
* **订单管理**
* **公告管理**
* **产品管理**
* **系统配置**


 
## Installing
 
**1.本地运行配置maven，jdk并检查版本是否兼容**  

**2.安装redis**  

**3.安装mysql8.0 并创建数据库`gpt`**  

**4.导入sql**  

**5.修改yml种的mysql与redis连接地址与账号密码**  

    dev-开发环境  
    
    prod-生产环境  
## yml
```yml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://127.0.0.1/gpt?characterEncoding=utf8&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true&autoReconnect=true&failOverReadOnly=false
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
    filters: stat
    maxActive: 20
    initialSize: 1
    maxWait: 60000
    minIdle: 1
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: select 'x'
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    maxOpenPreparedStatements: 20
redis:
  host: 127.0.0.1
  port: 6380
  password: password
  database: 0
  jedis:
    pool:
      max-idle: 100
      min-idle: 1
      max-active: 1000
      max-wait: -1  
```          
## SQL
 ```sql
 -- Table structure for announcement  
 
  DROP TABLE IF EXISTS `announcement`;  
  CREATE TABLE `announcement` (
  `id` bigint NOT NULL,
  `title` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '标题',
  `content` text CHARACTER SET utf8 COLLATE utf8_general_ci COMMENT '公告内容',
  `sort` int DEFAULT '0' COMMENT '排序',
  `type` int DEFAULT NULL COMMENT '公告类型 1-公告、2-指南',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='公告';  

-- Table structure for gpt_key  

DROP TABLE IF EXISTS `gpt_key`;
CREATE TABLE `gpt_key` (
  `id` bigint NOT NULL,
  `key` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT 'key',
  `use_number` int DEFAULT '0' COMMENT '使用次数',
  `sort` int DEFAULT '0' COMMENT '排序',
  `state` int DEFAULT '0' COMMENT '状态 0 启用 1禁用',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='gptkey\n';  

-- Table structure for pay_config  

DROP TABLE IF EXISTS `pay_config`;
CREATE TABLE `pay_config` (
  `id` bigint NOT NULL,
  `pid` int DEFAULT NULL COMMENT '易支付商户id',
  `secret_key` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '易支付商户密钥',
  `notify_url` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '易支付回调域名',
  `return_url` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '易支付跳转通知地址',
  `submit_url` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '易支付支付请求域名',
  `api_url` varchar(255) DEFAULT NULL COMMENT '易支付订单查询api',
  `ali_app_id` varchar(50) DEFAULT NULL COMMENT '支付宝appid',
  `ali_private_key` varchar(3000) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '支付宝应用私钥',
  `ali_public_key` varchar(500) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '支付宝应用公钥',
  `ali_gateway_url` varchar(100) DEFAULT NULL COMMENT '支付宝接口地址',
  `ali_notify_url` varchar(100) DEFAULT NULL COMMENT '支付宝回调地址',
  `ali_return_url` varchar(100) DEFAULT NULL COMMENT '支付宝页面跳转地址',
  `pay_type` tinyint DEFAULT '0' COMMENT '支付类型 0 易支付 1微信 2支付宝 3支付宝、微信',
  `wx_appid` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '微信支付的appid',
  `wx_mchid` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '微信支付直连商户号',
  `wx_native_url` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '微信nativepostapi地址',
  `wx_native_return_url` varchar(100) DEFAULT NULL COMMENT '微信native回调地址',
  `wx_v3_secret` varchar(40) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '微信apiv3秘钥',
  `wx_serial_no` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '商户api序列号',
  `wx_private_key` text CHARACTER SET utf8 COLLATE utf8_general_ci COMMENT '商户证书内容apiclient_key.pem',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='支付配置';

-- Table structure for product  

DROP TABLE IF EXISTS `product`;
CREATE TABLE `product` (
  `id` bigint NOT NULL,
  `name` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '产品名',
  `price` decimal(10,2) DEFAULT NULL COMMENT '价格',
  `type` tinyint DEFAULT '0' COMMENT '类型 0 次数 1 月卡 2 加油包',
  `number_times` int DEFAULT NULL COMMENT '次数',
  `monthly_number` int DEFAULT NULL COMMENT '月卡每日可使用次数',
  `sort` int DEFAULT '0' COMMENT '排序',
  `stock` int DEFAULT '1' COMMENT '库存数',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='产品表';  

-- Table structure for refueling_kit  

DROP TABLE IF EXISTS `refueling_kit`;
CREATE TABLE `refueling_kit` (
  `id` bigint NOT NULL,
  `product_id` bigint DEFAULT NULL COMMENT '产品id',
  `user_id` bigint DEFAULT NULL COMMENT '用户id',
  `number_times` int DEFAULT NULL COMMENT '可使用次数',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='加油包';  

-- Table structure for t_order  

DROP TABLE IF EXISTS `t_order`;
CREATE TABLE `t_order` (
  `id` bigint NOT NULL,
  `product_id` bigint DEFAULT NULL COMMENT '产品id',
  `user_id` bigint DEFAULT NULL COMMENT '用户id',
  `price` decimal(10,2) DEFAULT NULL COMMENT '价格',
  `state` int DEFAULT '0' COMMENT '状态 0待支付 1支付完成 2 支付失败',
  `pay_type` varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '支付方式 wxpay、alipay、qqpay',
  `pay_number` int DEFAULT NULL COMMENT '购买数量',
  `trade_no` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '易支付订单号',
  `msg` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '支付结果消息\n',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT NULL COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='订单表';  

-- Table structure for use_log  

DROP TABLE IF EXISTS `use_log`;
CREATE TABLE `use_log` (
  `id` bigint NOT NULL,
  `user_id` bigint DEFAULT NULL COMMENT '用户id',
  `use_number` int DEFAULT '1' COMMENT '使用次数',
  `use_type` tinyint DEFAULT '1' COMMENT '使用类型 1 次数 2 月卡 3加油包',
  `use_value` text CHARACTER SET utf8 COLLATE utf8_general_ci COMMENT '聊天内容',
  `kit_id` bigint DEFAULT NULL COMMENT '加油包id',
  `gpt_key` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '使用gptkey',
  `state` tinyint DEFAULT '0' COMMENT '是否成功 0成功 1失败',
  `question` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci COMMENT '问题',
  `answer` mediumtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci COMMENT '答案',
  `send_type` tinyint DEFAULT '0' COMMENT '消息类型 0-正常 1-流式',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='使用记录表';

-- Table structure for user  

DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` bigint NOT NULL,
  `name` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '姓名',
  `mobile` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '手机号',
  `password` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT '123456' COMMENT '密码',
  `last_login_time` datetime DEFAULT NULL COMMENT '上次登录时间',
  `type` tinyint DEFAULT '0' COMMENT '类型 0 次数用户 1 月卡用户 -1 管理员',
  `expiration_time` datetime DEFAULT NULL COMMENT '月卡到期日期',
  `remaining_times` int DEFAULT '5' COMMENT '剩余次数',
  `card_day_max_number` int DEFAULT '0' COMMENT '月卡当日使用最大次数',
  `from_user_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '微信用户账号',
  `is_event` tinyint DEFAULT '0' COMMENT '是否关注公众号 0未关注 1关注',
  `email` varchar(80) DEFAULT NULL COMMENT 'email地址',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='用户表';

-- Table structure for sys_config  

DROP TABLE IF EXISTS `sys_config`;
CREATE TABLE `sys_config` (
 `id` bigint NOT NULL,
 `registration_method` tinyint DEFAULT '1' COMMENT '注册模式 1账号密码  2 短信注册 3 关闭注册 4邮件注册',
 `key_switch` tinyint DEFAULT '0' COMMENT '是否禁用自动禁用key 0关闭 1开启',
 `default_times` int DEFAULT '10' COMMENT '默认注册次数',
 `ali_access_key_id` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '阿里云accessKeyId',
 `ali_secret` varchar(100) DEFAULT NULL COMMENT '阿里云secret',
 `ali_sign_name` varchar(50) DEFAULT NULL COMMENT '阿里云短信签名',
 `ali_template_code` varchar(50) DEFAULT NULL COMMENT '阿里云短信模版id',
 `img_upload_url` varchar(50) DEFAULT NULL COMMENT '图片上传地址',
 `img_return_url` varchar(50) DEFAULT NULL COMMENT '图片域名前缀',
 `sd_url` varchar(50) DEFAULT NULL COMMENT 'Sd接口地址',
 `is_open_sd` tinyint DEFAULT '0' COMMENT '是否开启sd 0未开启 1开启',
 `is_open_proxy` tinyint DEFAULT '0' COMMENT '是否开启代理 0关闭 1开启',
 `proxy_ip` varchar(20) DEFAULT NULL COMMENT '代理ip',
 `proxy_port` int DEFAULT NULL COMMENT '代理端口',
 `bing_cookie` varchar(300) DEFAULT NULL COMMENT '微软bing cookie',
 `is_open_bing` tinyint DEFAULT '0' COMMENT '是否开启bing 0-未开启 1开启',
 `is_open_flag_studio` tinyint DEFAULT '0' COMMENT '是否开启FlagStudio 0-未开启 1开启',
 `flag_studio_key` varchar(100) DEFAULT NULL COMMENT 'FlagStudio key',
 `flag_studio_url` varchar(100) DEFAULT NULL COMMENT 'FlagStudio 接口地址',
 `baidu_appid` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '百度appid',
 `baidu_secret` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '百度Secret\n',
 `mj_guild_id` bigint DEFAULT NULL COMMENT 'Mj服务器id',
 `mj_channel_id` bigint DEFAULT NULL COMMENT 'Mj频道id',
 `mj_user_token` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT 'mj用户token',
 `mj_bot_token` varchar(255) DEFAULT NULL COMMENT '频道机器人token',
 `mj_bot_name` varchar(50) DEFAULT NULL COMMENT '频道机器人名称',
 `mj_notify_hook` varchar(255) DEFAULT NULL COMMENT '任务状态变更回调地址',
 `is_open_mj` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT '0' COMMENT '是否开启mj 0-未开启 1开启',
 `baidu_key` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '百度应用key',
 `baidu_secret_key` varchar(50) DEFAULT NULL COMMENT '百度应用Secret',
 `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
 `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
 `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
 `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
 `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
 `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
 PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='系统配置';


-- Table structure for email_config  

DROP TABLE IF EXISTS `email_config`;
CREATE TABLE `email_config` (
  `id` bigint NOT NULL,
  `host` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT '邮件提供商地址',
  `port` int DEFAULT NULL COMMENT '端口号',
  `username` varchar(50) DEFAULT NULL COMMENT '邮件账号',
  `password` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL COMMENT 'SMTP授权密码',
  `protocol` varchar(20) DEFAULT NULL COMMENT '邮件协议',
  `data_version` int DEFAULT '0' COMMENT '数据版本（默认为0，每次编辑+1）',
  `deleted` int DEFAULT '0' COMMENT '是否删除：0-否、1-是',
  `creator` bigint DEFAULT '0' COMMENT '创建人编号（默认为0）',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间（默认为创建时服务器时间）',
  `operator` bigint DEFAULT '0' COMMENT '操作人编号（默认为0）',
  `operate_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '操作时间（每次更新时自动更新）',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 COMMENT='邮件配置表';
          
```         
## Running the tests
 
**启动idea出现如下信息说明运行成功**  

o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8000 (http) with context path ''  

com.cn.app.chatgptbot.Application        : Started Application in 5.138 seconds (process running for 5.521)

 ## Precautions For Using Nginx  
 
 **若使用nginx反向代理到后端需要增加socket支持，与socket长连接时间**  
 
 **proxy_set_header Upgrade $http_upgrade;**  
 
 **proxy_set_header Connection "upgrade";**  
 
 **proxy_read_timeout   3600s; #超时设置**  
 
 **proxy_send_timeout 12s;**  
 
 ![image](https://user-images.githubusercontent.com/43660702/230708043-911ea192-dbcd-4c1b-929a-d888be5bd237.png)
 
## Precautions For Using Proxy  

**项目中默认没有使用代理，如果需要可以自行修改ProxyUtil中的代码**
```java
     public ReactorClientHttpConnector getProxy() {
        HttpClient httpClient = HttpClient.create().tcpConfiguration((tcpClient) -> tcpClient.proxy(proxy -> proxy
                .type(ProxyProvider.Proxy.HTTP)
                .host("代理ip")
                .port(代理端口)));
        return new ReactorClientHttpConnector(httpClient);
    }
```  



### And coding style tests
 
 **Vue2.0前端地址[GPT-WEB-CLIENT](https://github.com/a616567126/GPT-WEB-CLIENT)**  
 

 
## Contributors

这个项目的存在要感谢所有做出贡献的人.

<a href="https://github.com/a616567126/GPT-WEB-JAVA/graphs/contributors">
<img src="https://contrib.rocks/image?repo=a616567126/GPT-WEB-JAVA" />
</a>  


 
## Put It Last  
**易支付网站地址：[白晨易支付](https://my.mmywl.cn/)**  
**作者使用服务器地址：[浅夏云](https://www.qxqxo.com/aff/ZGWPEDLQ)**  
**作者使用机场地址：[新华云](https://newhua99.com/#/register?code=fMYmE5Ri)**  
**默认启动时需配置gpt_key,pay_config,sys_config,因为项目启动时会加载对应参数到redis中，如果手动修改数据库，需要在redis中修改对应参数，防止不生效**
**FlagStudio地址：http://flagstudio.baai.ac.cn/


 **支付配置(pay_config)**
 字段|描述|注意
-|:-:|-:
pid|易支付商户id|无
secret_key|易支付商户秘钥|无
notify_url|易支付回调域名|后台接口地址：例如https://baidu.com 记得带上'http/https://'
return_url|易支付跳转通知地址|支付成功后返回的前端页面：例如https://baidu.com/#/user/product !!!此域名是客户端地址，#/user/product为客户端路由地址
submit_url|易支付支付请求域名|易支付发起支付的api地址，例如：https://pay888.mfysc.shop/submit.php
api_url|易支付订单查询api|后端核对订单时，易支付使用订单查询的api地址例如：https://pay888.mfysc.shop/api.php
ali_app_id|支付宝appid|无
ali_private_key|支付宝应用私钥|无
ali_public_key|支付宝应用公钥|无
ali_gateway_url|支付宝接口地址|写死：openapi.alipay.com！！！千万不要带https/http://
ali_notify_url|支付宝回调地址|支付宝支付成功后回调地址，例如：https://baidu.com/order/ali/callBack baidu.com为后台接口地址，记得带上http/https://
ali_return_url|支付宝页面跳转地址|支付宝支付成功后页面跳转地址，例如：https://baidu.com/#/user/product，!!!此域名是客户端地址，#/user/product为客户端路由地址
pay_type|支付类型 0 易支付 1微信 2支付宝 3支付宝、微信|开启对应类型之后记得配置相关支付参数
wx_appid|微信支付的appid|无
wx_mchid|微信支付商户号|无
wx_native_url|微信nativepostapi地址|写死：https://api.mch.weixin.qq.com/v3/pay/transactions/native 记得带https://
wx_native_return_url|微信native回调地址|微信支付成功后回调地址，例如：https://baidu.com/order/wx/callBack baidu.com为后台接口地址，记得带上http/https://
wx_v3_secret|微信apiv3秘钥|注意区分v2 v3区别，本系统采用v3方式
wx_serial_no|商户api序列号|无
wx_private_key|商户证书内容apiclient_key.pem|无  

 **系统配置(sys_config)**
 字段|描述|注意
-|:-:|-:
registration_method|注册模式 1账号密码  2 短信注册 3 关闭注册 4邮件注册|开启短信注册需配置阿里短信相关参数，具体参数在下面，开启邮件注册后需要在emil_config中配置邮件相关参数
key_switch|是否禁用自动禁用key 0关闭 1开启|开启后请求异常时会自动禁用key
default_times|默认注册次数|用户注册时默认赠送请求次数
ali_access_key_id|阿里云accessKeyId|无
ali_secret|阿里云secret|无
ali_sign_name|阿里云短信签名|无
ali_template_code|阿里云短信模版id|无
img_upload_url|图片上传地址|例如：/usr/local 配置图片上传路径
img_return_url|图片域名前缀|上传图片后与图片名组合成可访问的url 例如：https://baidu.com 图片上传成功后 则返回 https://baidu.com /2023/04/26/2222.jpg 
sd_url|Sd接口地址|开启sd时需配置这个地址
is_open_sd|是否开启sd 0未开启 1开启|无
is_open_proxy|是否开启代理 0关闭 1开启|无
proxy_ip|代理ip|无
proxy_port|代理端口|无
bing_cookie|微软bing cookie|无
is_open_bing|是否开启bing 0-未开启 1开启|无
is_open_flag_studio|是否开启FlagStudio 0-未开启 1开启|无
flag_studio_key|FlagStudio key|登录之后api获得每天500次请求
flag_studio_url|FlagStudio 接口地址|暂时写死https://flagopen.baai.ac.cn/flagStudio
baidu_appid|百度appid|用于百度翻译
baidu_secret|百度Secret|用于百度翻译
baidu_key|百度应用key|用于敏感词检查
baidu_secret_key|百度应用Secret|用于敏感词检查
is_open_mj|是否开启mj 0-未开启 1开启|无
mj_guild_id|Mj服务器id|url地址中获得
mj_channel_id|Mj频道id|url地址中获得
mj_user_token|mj用户token|F12查看network中的Authorization参数
mj_bot_token|机器人token|https://discord.com/developers/applications中获取
mj_bot_name|机器人名称|默认Midjourney Bot若其他自行修改
mj_notify_hook|m机器人回调通知地址|无
 
 
 
 
 
## 条件允许的情况下可以请作者喝一杯冰阔落
 * **支付宝**  
 * <img src="https://user-images.githubusercontent.com/43660702/228105535-144d09cd-6326-4c22-b9b9-8c69c299caac.png" width="100px" height="100px">
 * **微信**
 * <img src="https://user-images.githubusercontent.com/43660702/228105188-09c49078-9156-40bc-8327-f2b05c5bc5fa.png" width="100px" height="100px"> 
 
 
## 记得点一个Star哦!!!!  

## 扫码添加好友
![IMG_60D5DE670485-1](https://user-images.githubusercontent.com/43660702/232187172-9d971a97-b7a3-407f-9ba1-a35516505733.jpeg)



## 关注公众号
![关注公众号](https://user-images.githubusercontent.com/43660702/229270101-4f11a841-51fc-4625-b498-833629fe7934.png)



[![Star History Chart](https://api.star-history.com/svg?repos=a616567126/GPT-WEB-JAVA&type=Timeline)](https://star-history.com/#a616567126/GPT-WEB-JAVA&Timeline)  


## SPONSOR
本项目由[JetBranins](https://www.jetbrains.com/?from=Unity3DTraining)赞助相关开发工具  
<a href="https://www.jetbrains.com/?from=Unity3DTraining"><img src="https://github.com/XINCGer/Unity3DTraining/blob/master/Doc/images/jetbrains.png" width = "150" height = "150" div align=center /></a>

## License

Apache License 2.0
