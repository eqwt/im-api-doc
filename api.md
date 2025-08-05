# 接口说明

```
接口前缀：域名+/api/

响应数据格式：
{
	code：状态码(0=失败 1=成功 401=登录失效)
	msg：提示消息
	data：{...} 数据
}

登录之后的其它接口需要在请求头中增加token，如：Authorization：Bearer xxxx
token刷新：如响应头中有Authorization，则刷新token

后续具体接口描述，大致格式如下，看下下面API就懂了：
{接口文字描述}  {路由}  {请求方法}
[] 请求参数
{} 响应参数，只会描述data部分
```

# API

## 注册  register  post

```
[
	way：注册方式(mobile=手机 email=邮箱)
	mobile：手机，way=mobile时必须
	email：邮箱，way=email时必须
	code：验证码，测试暂时用123456
	password：密码
	password_confirmation：确认密码
]
```

## 登录  login  post

```
[
	way：登录方式(pwd=密码登录 code=验证码登录)
	device_id：设备码
	username：用户名，手机或邮箱
	password：密码，way=pwd时必须
	code：验证码，way=code时必须，测试暂时用123456
]

{
	token：登录token
	user_sig：腾讯SDK UserSig
	tx_appid：腾讯SDK appid
	tx_id：腾讯用户id
}
```

## 获取验证码  send_code  post

```
[
	type：类型(register=注册 login=登录)
	account：账号(手机/邮箱)
]
```

## 登录用户信息  user  get

```
{
	number：用户app号
	custom_id：用户appid
	pretty_id：用户靓号id
	mobile：手机
	email：邮箱
}
```

## 添加好友-搜索  friend/search  get

```
[
	account：搜索账号
]

{
	tx_id：腾讯用户id
	number：用户app号
	custom_id：用户appid
	pretty_id：用户靓号id
}
```

## 添加好友-提交  friend/add  post

```
[
	type：渠道(1=搜索 2=群成员 3=好友名片)
	
	// 公共参数
	wording：附言，可空
	remark：备注，可空
	group_name：分组，可空
	
	// type=1时
	account：账号(搜索时填写的参数带过来)
	tx_id：对方腾讯用户id
	
	// type=2时
	group_number：群号码
	tx_id：对方腾讯用户id
	
	// type=3时，待定------------
	
]
```

## 好友详情  friend/info  get

```
[
	tx_id：对方腾讯用户id
]

{
	number：用户app号
	custom_id：用户appid
	pretty_id：用户靓号id
}
```

