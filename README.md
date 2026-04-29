# 黑马点评 - Spring Boot 后端项目

> 基于 Spring Boot + Redis + MySQL 构建的企业级点评平台后端服务

## 🏗️ 技术栈

| 技术 | 版本 | 说明 |
|------|------|------|
| Java | 8+ | 编程语言 |
| Spring Boot | 2.7.x | 后端框架 |
| Spring Data Redis | 2.7.x | Redis 数据访问 |
| MyBatis Plus | 3.5.x | ORM 框架 |
| MySQL | 8.0+ | 关系型数据库 |
| Redis | 6.0+ | 缓存数据库 |
| Redisson | 3.19.x | 分布式锁 |
| Hutool | 5.8.x | 工具类库 |

## 📋 功能特性

### 用户模块
- ✅ 邮箱验证码登录
- ✅ 退出登录
- ✅ 用户信息管理
- ✅ 每日签到
- ✅ 签到统计

### 商户模块
- ✅ 商户信息管理
- ✅ 商户类型管理
- ✅ 商户缓存优化

### 优惠券模块
- ✅ 普通优惠券管理
- ✅ 秒杀优惠券
- ✅ 订单管理

### 社交模块
- ✅ 关注功能
- ✅ 博客管理
- ✅ 评论管理

## 🚀 快速开始

### 环境要求
- JDK 8+
- MySQL 8.0+
- Redis 6.0+
- Maven 3.6+

### 数据库配置

1. 创建数据库：
```sql
CREATE DATABASE IF NOT EXISTS hmdp DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

2. 导入数据：
```bash
mysql -u root -p hmdp < hmdp.sql
```

### 配置文件

修改 `src/main/resources/application.yaml`：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/hmdp?useSSL=false&serverTimezone=UTC
    username: your_username
    password: your_password
  redis:
    host: localhost
    port: 6379
    password: your_redis_password
  mail:
    host: smtp.qq.com
    port: 465
    username: your_email@qq.com
    password: your_authorization_code
```

### 启动项目

```bash
# 开发模式
mvn spring-boot:run

# 打包部署
mvn clean package
java -jar target/hm-dianping-1.0.0.jar
```

## 🔌 API 接口文档

### 用户接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/user/code` | POST | 发送邮箱验证码 |
| `/user/login` | POST | 用户登录 |
| `/user/logout` | POST | 用户退出 |
| `/user/me` | GET | 获取当前用户 |
| `/user/sign` | POST | 签到 |
| `/user/sign/count` | GET | 获取签到次数 |

### 商户接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/shop` | GET | 获取商户列表 |
| `/shop/{id}` | GET | 获取商户详情 |
| `/shop` | POST | 创建商户 |
| `/shop/{id}` | PUT | 更新商户 |
| `/shop/{id}` | DELETE | 删除商户 |

### 优惠券接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/voucher` | POST | 添加优惠券 |
| `/voucher/list/{shopId}` | GET | 获取商户优惠券 |
| `/voucher/seckill/{id}` | POST | 秒杀优惠券 |

### 关注接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/follow/{id}` | POST | 关注用户 |
| `/follow/{id}` | DELETE | 取消关注 |
| `/follow/isFollow/{id}` | GET | 检查是否关注 |

### 博客接口

| 接口 | 方法 | 描述 |
|------|------|------|
| `/blog` | GET | 获取博客列表 |
| `/blog/{id}` | GET | 获取博客详情 |
| `/blog` | POST | 发布博客 |
| `/blog/like/{id}` | POST | 点赞/取消点赞 |

## 📁 项目结构

```
hm-dianping/
├── src/
│   └── main/
│       ├── java/com/hmdp/
│       │   ├── controller/     # 控制层
│       │   ├── service/        # 服务层
│       │   │   └── impl/       # 服务实现
│       │   ├── mapper/         # 数据访问层
│       │   ├── entity/         # 实体类
│       │   ├── dto/            # 数据传输对象
│       │   ├── config/         # 配置类
│       │   ├── utils/          # 工具类
│       │   └── HmDianPingApplication.java
│       └── resources/
│           ├── mapper/         # MyBatis XML
│           ├── db/             # SQL 文件
│           ├── application.yaml
│           └── application.properties
├── pom.xml
└── hmdp.sql
```

## 🧩 核心功能实现

### 1. 邮箱验证码登录

```
前端请求发送验证码 → 生成6位随机码 → 存储Redis(有效期2分钟) → 发送邮箱
前端携带邮箱+验证码登录 → 验证Redis中的验证码 → 查询/创建用户 → 生成Token → 返回Token
```

### 2. 分布式秒杀

- 使用 Redis 预减库存
- 使用 Redisson 分布式锁防止超卖
- Lua 脚本保证原子性

### 3. 缓存策略

- 商户信息缓存（30分钟过期）
- 用户登录状态缓存（10小时过期）
- 空值缓存（防止缓存穿透）

## 🔧 配置说明

### Redis 常量配置

| 常量 | 值 | 说明 |
|------|-----|------|
| LOGIN_CODE_KEY | login:code: | 验证码前缀 |
| LOGIN_CODE_TTL | 2 | 验证码有效期(分钟) |
| LOGIN_USER_KEY | login:token: | 用户登录前缀 |
| LOGIN_USER_TTL | 36000 | 用户有效期(秒) |

## 🤝 贡献指南

1. Fork 本仓库
2. 创建功能分支
3. 提交代码
4. 创建 Pull Request

## 📄 许可证

MIT License

## 📞 联系我们

如有问题或建议，欢迎提交 Issue 或联系开发者。

---

**注意**: 本项目仅用于学习和演示目的，生产环境使用请谨慎。
