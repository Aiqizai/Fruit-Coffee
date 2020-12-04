# Fruit-Coffee

## 项目运行
```
# 克隆到本地
git clone https://github.com/Aiqizai/Fruit-Coffee.git

# 进入文件夹
cd Fruit-Coffee

# 安装依赖
npm install 或 yarn(推荐)

# 开启本地服务器localhost:8080
npm run serve

# 发布环境
npm run build
```

### 项目描述
```
咖啡爱好者，适合不同人群的最佳选择。查看热卖咖啡，在分类中选购自己喜欢的咖啡，足不出户就能选购自己喜爱的咖啡送到你的面前的一个咖啡商城。
```

### 实现的功能
```
1、用户注册，用户登录，通过手机号码注册

2、查看商品分类菜单，通过关键字搜索商品

3、查看商品详情

4、加入购物车，收藏商品，购物车管理

5、提交订单，订单管理

6、地址管理，修改个人资料

7、安全中心包括修改密码，注销账号，以及退出登录

```
### 主要技术栈 vue-cli + vue-router + axios + es6 + + less + Vant 

### 技术要点
```
1、使用Vue-cli搭建页面，部分组件使用Vant UI框架

2、Vue数据驱动DOM渲染

3、使用Vue-router设置路由，进行页面跳转

4、使用axios发送数据请求 

5、localStorage实现数据持久化

6、使用ES6语法和less进行代码的编写

7、使用postcss-pxtorem 和 lib-flexible 进行rem适配

8、根据后台返回的token判断是否已经有用户登录, 才能进行购买, 查看个人信息
```
