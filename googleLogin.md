# Google登录功能需求文档

## 1. 功能概述
实现移动应用的Google账号登录功能，通过Google OAuth认证获取用户信息，并与ERP系统对接完成用户登录。

## 2. 系统配置获取
### 2.1 配置信息来源
- API地址：https://dev.mob-mob.com/app/server-script/Mobile%20APP%20Api%20-%20Get%20App%20Setting
- 配置项：
  - Google OAuth Client ID
  - Google OAuth Client Secret

### 2.2 配置信息获取流程
1. 调用mob-mob API获取应用配置信息
2. 从返回结果中提取Google OAuth相关配置

## 3. 登录流程
### 3.1 技术实现
使用Capacitor.Plugins.SocialLogin插件实现Google登录功能：
```javascript
// 初始化Google登录配置
await Capacitor.Plugins.SocialLogin.initialize({
  google: {
    webClientId: '获取的Google Client ID'
  }
});

// 执行Google登录
const res = await Capacitor.Plugins.SocialLogin.login({
  provider: 'google',
  options: {}
});
```

### 3.2 具体步骤
1. 初始化Google登录配置
   - 从mob-mob API获取Google OAuth Client ID
   - 使用SocialLogin.initialize方法进行初始化
   - 确保配置信息正确加载

2. 调用Google登录接口
   - 调用SocialLogin.login方法
   - 设置provider参数为'google'
   - 处理用户取消登录的情况

3. 获取Google授权令牌
   - 从登录响应中提取accessToken
   - 验证令牌的有效性
   - 保存令牌用于后续请求

4. 将令牌提交给ERP系统
   - 构建包含令牌的请求体
   - 发送POST请求到ERP验证接口
   - 设置适当的请求头部信息

5. 获取ERP系统返回的登录结果
   - 解析ERP系统的响应数据
   - 存储用户登录状态和信息
   - 根据登录结果进行相应的界面跳转

## 4. ERP系统对接
### 4.1 对接内容
- 提交Google授权令牌
- 获取登录验证结果

### 4.2 注意事项
- ERP系统对接的具体细节需要与海华确认
- 包括但不限于：
  - 接口地址
  - 请求参数格式
  - 返回结果格式
  - 错误处理机制

## 5. 错误处理
### 5.1 需处理的错误情况
1. 配置信息获取失败
2. Google登录失败或取消
3. 令牌获取失败
4. ERP系统验证失败

### 5.2 错误处理方式
- 显示适当的错误提示
- 提供重试机制
- 记录错误日志