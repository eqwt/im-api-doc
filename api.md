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
字段名：Tag_Profile_Custom_UserR

参数：
{
	level：会员等级
	level_expires_at：会员到期时间
	pretty_id：靓号id
	pretty_id_level：靓号id等级(1=非靓号 2=普通靓号 3=黑金靓号)
	pretty_id_expires_at：靓号id过期时间
	
	is_open_mobile_search：是否开启手机搜索
	is_open_app_id_search：是否开启ID搜索
	is_open_group_add：是否开启群聊添加
	is_open_name_card_add：是否开启名片添加
	is_bind_device：是否绑定当前设备
	is_only_code_login：是否仅验证码登录
	
	updated_at：用户更新ID的时间，用于查看好友信息时本地缓存比对
}
```

## 群组字段

```
字段名：GroupR
{
	level：VIP等级
	level_expires_at：VIP到期时间
	pretty_id：靓号id
	pretty_id_level：靓号id等级(1=非靓号 2=普通靓号 3=黑金靓号)
	pretty_id_expires_at：靓号id过期时间
}

字段名：GroupRw
{
	is_ban_revoke：是否禁止群成员撤回
	is_ban_mutual_add：是否禁止群成员互加
	is_popup_notice：是否弹窗提示公告
}
```

# API

# 登录注册

### 注册  register  post

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

### 登录  login  post

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

### 获取验证码  send_code  post

```
说明：
1. 无需登录的验证码(比如：登录、注册、找回密码)
2. 如用户登录后发送验证码，请求头需要token(比如：修改手机号、邮箱、修改登录密码)

[
	type：类型(register=注册 login=登录 update_pwd=修改登录密码 bind_mobile=绑定手机 bind_email=绑定邮箱 update_mobile=修改手机号 update_email=修改邮箱 forget_pwd=找回密码)
	account：账号(手机/邮箱)
]
```

## 用户相关

### 登录用户信息  user  get

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

### 更新用户信息  user  put

```
[
	nickname：昵称
	is_open_mobile_search：是否开启手机搜索
	is_open_app_id_search：是否开启ID搜索
	is_open_group_add：是否开启群聊添加
	is_open_name_card_add：是否开启名片添加
	is_bind_device：是否绑定当前设备
	is_only_code_login：是否仅验证码登录
]
```

### 上传头像  user/avatar  post

```
[
	file：文件二进制流
]
{
	url：上传后地址
}
```

### 修改app_id  user/app_id  put

```
说明：用户level必须是会员，才可以修改

[
	app_id：新appId
]
```

### 修改登录密码  user/password  put

```
[
	way：验证方式(pwd=原密码验证 mobile=手机验证 email=邮箱验证)
	
	// way=pwd
	old_password：旧密码
	new_password：新密码
	new_password_confirmation：确认新密码
	
	// way=mobile 或 way=email
	code：验证码（发送验证码 type=update_pwd）
]
```

### 绑定手机  user/bind_mobile  post

```
[
	mobile：手机号
	code：验证码（发送验证码 type=bind_mobile）
]
```

### 绑定邮箱  user/bind_email  post

```
[
	email：邮箱
	code：验证码（发送验证码 type=bind_email）
]
```

## 好友相关

### 添加好友-搜索  friend/search  get

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

### 添加好友-提交  friend/add  post

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

### 好友详情  friend/info  get

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

## 群相关

### 创建群  group  post

```
[
	name：群名称
	avatar：群头像，可空
]

{
	number：群号码
}
```

### 搜索群  group/search  get

```
说明：用户搜索群号时，如果群号长度<9位,请求接口；>=9位,请求腾讯

[
	pretty_id：搜索群号
]

{
	number：IM群号
}
```

