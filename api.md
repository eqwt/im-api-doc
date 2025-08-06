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

一般约定：
is_ 开头的字段，表示：是否xxx，取值：0=否 1=是
```

# IM自定义字段-----待定

```
字段值使用json字符串存储
```

## 用户字段

```
字段名：

参数：
[
	....
]
```

## 群组字段

```


[

]
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
	user_sig：IMSDK UserSig
	im_appid：IMSDK appid
	im_id：IM用户id
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
	app_id：用户APPid
	mobile：手机
	email：邮箱
	pretty_id：用户靓号id
	pretty_id_level：用户靓号id等级(1=非靓号 2=普通靓号 3=黑金靓号)
	level：用户VIP等级(1=非会员 2=VIP 3=SVIP)
	is_open_mobile_search：是否开启手机搜索
	is_open_app_id_search：是否开启ID搜索
	is_open_group_add：是否开启群聊添加
	is_open_name_card_add：是否开启名片添加
	is_bind_device：是否绑定当前设备
	is_only_code_login：是否仅验证码登录
}
```

## 更新用户信息  user  put

```
[
	field：字段名(is_open_mobile_search、is_open_app_id_search、is_open_group_add、is_open_name_card_add、is_bind_device、is_only_code_login)
	value：修改后的值，一般约定：is_ 开头的字段，表示是否xxx，取值：0=否 1=是
]
```

## 添加好友-搜索  friend/search  get

```
[
	account：搜索账号
]

{
	im_id：IM用户id
	app_id：用户APPid
	pretty_id：用户靓号id
	pretty_id_level：用户靓号id等级(1=非靓号 2=普通靓号 3=黑金靓号)
	level：用户VIP等级(1=非会员 2=VIP 3=SVIP)
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
	im_id：对方腾讯用户id
	
	// type=2时
	group_number：群号码
	im_id：对方腾讯用户id
	
	// type=3时，待定------------
	
]
```

## 好友详情  friend/info  get

```
[
	im_id：对方腾讯用户id
]

{
	app_id：用户APPid
	pretty_id：用户靓号id
	pretty_id_level：用户靓号id等级(1=非靓号 2=普通靓号 3=黑金靓号)
	level：用户VIP等级(1=非会员 2=VIP 3=SVIP)
}
```

## 创建群  group  post

```
[
	name：群名称
	avatar：群头像，可空
]

{
	number：群号码
}
```

