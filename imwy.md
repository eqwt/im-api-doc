# ------API------

# 通用错误状态码

```
20090：禁用设备
20091：禁用IP
20092：禁用客户端类型
20093：非内部IP
20094：该账号类型禁止登录该客户端类型
20095：当前版本已停止使用,请更新!
20096：非内部设备
20099：服务错误
```

# 登录

## 注册

```
！使用登录接口中扩展字段，使用 register 账号提交

扩展字段：json字符串
{
	app_name：APP名称
	app_version：APP版本
	action：register
	username：账号
	nickname：昵称
	password：登录密码
	safe_pwd：安全密码
}

响应示例：
{
  "errCode": 1, // 注册固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：注册成功
  20001：账号必须6-20位
  20002：账号格式不正确
  20003：账号已存在
  20004：登录密码必须8-20位
  20005：登录密码格式不正确
  20006：安全密码必须是8位数字
  20007：安全密码不能是相同数字
  20008：安全密码不能是连续数字
  20009：昵称必须1-64个字符内
}
```

## 登录

```
请求扩展字段(在需要二次验证时)：
{
	app_name：APP名称
	app_version：APP版本
	
	verify_way：验证方式(1=原设备验证 2=安全密码验证)
	
	// verify_way=1时，
	code：验证码，该字段为空时表示发送至原设备，不空时表示验证验证
	
	// verify_way=2时
	safe_pwd：安全密码
}

响应：
{
  "errCode": 错误码(0=成功 1=失败)
  "responseCode": 状态码
  
  // 状态码定义
  20001：用户不存在
  20002：密码错误
  20003：非信任设备
  20004：验证方法非法
  20005：原设备验证码发送成功
  20006：原设备验证码不正确
  20007：原设备验证码发送频繁
  20008：安全密码不正确
}
```

## 忘记登录密码

```
！使用登录接口中扩展字段，使用 register 账号提交

扩展字段：json字符串
{
	app_name：APP名称
	app_version：APP版本
	
	action：forget_pwd
	username：账号
	safe_pwd：安全密码
	new_pwd：新密码
}

响应示例：
{
  "errCode": 1, // 注册固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：成功
  20001：账号不存在
  20002：安全密码不正确
  20003：新密码必须8-20位
  20004：新密码格式不正确
}
```

# 用户信息

## 修改安全密码

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：update_safe_pwd
	old_pwd：旧密码
	new_pwd：新密码
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：修改成功
  20001：旧密码不正确
  20002：新密码必须是8位数字
  20003：新密码不能是相同数字
  20004：新密码不能是连续数字
}
```

## 修改登录密码

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：update_login_pwd
	verify_way：验证方式(1=旧密码验证 2=安全密码验证)
	
	// verify_way=1时
	old_pwd：旧密码
	new_pwd：新密码
	
	// verify_way=2时
	safe_pwd：安全密码
	new_pwd：新密码
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：修改成功
  20001：验证方法非法
  20002：旧密码不正确
  20003：新密码必须8-20位
  20004：新密码格式不正确
  20005：安全密码不正确
}
```

## 获取代理IP

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：get_proxy
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：修改成功
  20001：暂无可用代理
}
```

# 红包

## 红包消息说明

```
红包消息使用的是自定义消息，自定义内容如下：
{
	type：red_packet, // 消息类型：红包
	sub_type：红包类型（personal=个人红包 group_lucky=群拼手气红包 group_normal=群普通红包 group_exclusive=群专属红包）
	trade_no：单号
	username：发包人账号
	team_id：群id(群红包 存在)
	to_username：接收人账号(个人红包 或 群专属红包 存在)
	amount：总红包金额
	remain_amount：剩余红包金额
	num：总红包个数
	remain_num：剩余红包个数
	remark：祝福语
	expired_ts：过期时间戳
	// 抢包详情
	receivers：[
		{
			username：收包人账号
			bonus：抢包金额
			ts：抢包时间戳
		}
		...
	]
}
```

## 发红包

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：send_red_packet
	type：红包类型（personal=个人红包 group_lucky=群拼手气红包 group_normal=群普通红包 group_exclusive=群专属红包）
	
	// 个人红包
	to_username：接收人账号
	amount：总金额
	remark：祝福语，非必填
	
	// 群拼手气红包
	team_id：群id
	amount：总金额
	num：红包个数
	remark：祝福语，非必填
	
	// 群普通红包
	team_id：群号
	single_amount：单个金额
	num：红包个数
	remark：祝福语，非必填
	
	// 群专属红包
	team_id：群号
	to_username：指定账号
	amount：总金额
	remark：祝福语，非必填
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：发送成功
  20001：红包类型非法
  20002：群号非法或不存在
  20003：接收人非法或不存在
  20004：红包金额格式非法
  20005：红包数量格式非法
  20006：红包数量超限
  20007：祝福语字符数超限
  20008：余额不足
  20009：非法操作
}
```

## 抢红包

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：grab_red_packet
	trade_no：红包单号
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：抢包成功
  20001：红包不存在
  20002：红包已抢完
  20003：红包已过期
  20004：红包已领取
  20009：非法操作
}
```

# 转账

## 转账消息说明

```
消息使用的是自定义消息，自定义内容如下：
{
	type：transfer, // 消息类型：转账
	sub_type：转账类型（personal=个人转账 group=群转账）
	trade_no：单号
	username：发包人账号
	to_username：接收人账号
	team_id：群id（群转账时必填）
	amount：转账金额
	remark：转账备注
	expired_ts：过期时间戳
	received_at：收款时间戳
	state：状态（created=待接收 received=已接收 rejected=已退还 refunded=已退款）
}
```

## 转账退还消息

```
消息使用的是自定义消息，自定义内容如下：
{
	type：transfer_rejected, // 消息类型：转账退还
	sub_type：转账类型（personal=个人转账 group=群转账）
	trade_no：单号
	username：发包人账号
	to_username：接收人账号
	team_id：群id（群转账时必填）
	amount：转账金额
	remark：转账备注
	expired_ts：过期时间戳
	state：状态（created=待接收 received=已接收 rejected=已退还 refunded=已退款）
}
```

## 转出

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：send_transfer
	type：转账类型（personal=个人转账 group=群转账）
	
	// 个人转账
	to_username：接收人账号
	amount：金额
	remark：备注，非必填
	
	// 群转账
	team_id：群id
	to_username：接收人账号
	amount：金额
	remark：备注，非必填
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：发送成功
  20001：转账类型非法
  20002：接收人非法或不存在
  20003：群号非法或不存在
  20004：转账金额格式非法
  20005：备注字符数超限
  20006：余额不足
  20009：非法操作
}
```

## 接收

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：receive_transfer
	trade_no：转账单号
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：成功
  20001：转账不存在
  20002：已接收
  20003：已退还
  20004：已过期
  20009：非法操作
}
```

## 退还

```
！使用更新用户资料API中扩展字段

请求扩展字段
{
	action：reject_transfer
	trade_no：转账单号
}

响应：
{
  "errCode": 1, // 固定返回 1，根据responseCode判断是否成功
  "responseCode": 状态码
  
  // 状态码定义
  20000：成功
  20001：转账不存在
  20002：已接收
  20003：已退还
  20004：已过期
  20009：非法操作
}
```

# 其它

## 创建群

```
响应：
{
  "errCode": 错误码(0=成功 1=失败)
  "responseCode": 状态码
  
  // 状态码定义
  20001：只可创建高级群
  20002：无权限创建(非内部账号)
}
```

## 添加好友

```
响应：
{
  "errCode": 错误码(0=成功 1=失败)
  "responseCode": 状态码
  
  // 状态码定义
  20001：无权限创建(非内部账号)
}
```

， 
