# 简介

`uni-id`为`uniCloud`开发者提供了简单、易用的用户管理能力封装。

`uni-id`定义了常用的数据表结构，前端开发者无需为没有后端数据库设计经验而烦恼。

`uni-id`作为公用 SDK，封装了用户注册、登录、Token 校验、修改密码、设置头像等常见用户管理功能，以 API 方式调用，开发者将`uni-id`作为公用模块导入后，可在云函数中便捷调用。

`uni-id`是开源 sdk，可放心使用。

对于`uni-id`还未封装的能力，欢迎大家在开源项目上提交 pr，共同完善这个开源项目，[uni-id git仓库](https://gitee.com/dcloud/uni-id.git)。

# API列表

## 用户注册 @register

用法`uniID.register(Object user)`

**user参数说明**

| 字段		| 类型	| 必填	| 说明			|
| ---		| ---	| ---	| ---			|
| username	| String| 是	|用户名，唯一	|
| password	| String| 是	|密码			|

**响应参数**

| 字段	| 类型	| 必填	| 说明						|
| ---	| ---	| ---	| ---						|
| code	| Number| 是	|错误码，0表示成功			|
| msg	| String| 是	|详细信息					|
| token	| String| -	|注册完成自动登录之后返回的token信息|

**示例代码**

```js
// 云函数代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		username,
		password
	} = event
	// username、password验证是否合法的逻辑
	const res = await uniID.register({
		username,
		password
	})
	return res
}

// 客户端代码
uniCloud.callFunction({
	name: 'register',
	data: {
		username: 'username',
		password: 'user password'
	},
	success(res){
		if(res.result.code === 0) {
			uni.setStorageSync('uniIdToken',res.result.token)
			// 其他业务代码，如跳转到首页等
			uni.showToast({
        title: '注册成功',
        icon: 'none'
      })
		} else {
			uni.showModal({
				content: res.result.msg,
				showCancel: false
			})
		}
	},
	fail(){
		uni.showModal({
			content: '注册失败，请稍后再试',
			showCancel: false
		})
	}
})
```


## 用户登录 @login

用法：`uniID.login(Object user)`

**user参数说明**

| 字段		| 类型	| 必填	| 说明	|
| ---		| ---	| ---	| ---	|
| username	| String| 是	|用户名	|
| password	| String| 是	|密码	|

**响应参数**

| 字段	| 类型	| 必填	| 说明						|
| ---	| ---	| ---	| ---						|
| code	| Number| 是	|错误码，0表示成功			|
| msg	| String| 是	|详细信息					|
| token	| String| -	|登录成功之后返回的token信息|

**示例代码**

```js
// 云函数代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		username,
		password
	} = event
	// username、password验证是否合法的逻辑
	const res = await uniID.login({
		username,
		password
	})
	return res
}
```

## 修改密码 @update-password

用法：`uniID.updatePwd(Object passwordInfo)`

**passwordInfo参数说明**

| 字段					| 类型	| 必填	| 说明		|
| ---					| ---	| ---	| ---		|
| oldPassword			| String| 是	|旧密码		|
| newPassword			| String| 是	|新密码		|
| passwordConfirmation	| String| 是	|确认新密码	|

**响应参数**

| 字段	| 类型	| 必填	| 说明						|
| ---	| ---	| ---	| ---						|
| code	| Number| 是	|错误码，0表示成功			|
| msg	| String| 是	|详细信息					|

**示例代码**

```js
// 云函数update-pwd代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		oldPassword,
		newPassword,
		passwordConfirmation
	} = event
	// 校验新密码与确认新密码是否一致
	const res = await uniID.updatePwd({
		oldPassword,
		newPassword,
		passwordConfirmation
	})
	return res
}
```

## 设置头像

用法：`uniID.setAvatar(Object avatarInfo)`

**avatarInfo**参数说明

| 字段	| 类型	| 必填| 说明			|
| ---		| ---		| ---	| ---				|
| avatar| String| 是	|用户头像URL|

**响应参数**

| 字段| 类型	| 必填| 说明						|
| ---	| ---		| ---	| ---							|
| code| Number| 是	|错误码，0表示成功|
| msg	| String| 是	|详细信息					|

**示例代码**

```js
// 云函数set-avatar代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		avatar
	} = event
	// 校验avatar链接合法性
	const res = await uniID.setAvatar({
		avatar
	})
	return res
}

```

## 绑定手机号

用法：`uniID.bindMobile(Object mobileInfo)`

**mobileInfo**参数说明

| 字段	| 类型	| 必填| 说明			|
| ---		| ---		| ---	| ---				|
| mobile| String| 是	|用户手机号|

**响应参数**

| 字段| 类型	| 必填| 说明						|
| ---	| ---		| ---	| ---							|
| code| Number| 是	|错误码，0表示成功|
| msg	| String| 是	|详细信息					|

**示例代码**

```js
// 云函数bind-mobile代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		mobile
	} = event
	// 校验手机号合法性
	const res = await uniID.bindMobile({
		mobile
	})
	return res
}

```

## 绑定邮箱

用法：`uniID.bindEmail(Object emailInfo)`

**emailInfo**参数说明

| 字段	| 类型	| 必填| 说明		|
| ---		| ---		| ---	| ---			|
| email	| String| 是	|用户邮箱	|

**响应参数**

| 字段| 类型	| 必填| 说明						|
| ---	| ---		| ---	| ---							|
| code| Number| 是	|错误码，0表示成功|
| msg	| String| 是	|详细信息					|

**示例代码**

```js
// 云函数bind-email代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
		email
	} = event
	// 校验手机号合法性
	const res = await uniID.bindEmail({
		email
	})
	return res
}


```

## 登出

用法：`uniID.logout(String uid);`

参数说明

| 字段| 类型	| 必填| 说明	|
| ---	| ---		| ---	| ---		|
| uid	| String| 是	|用户uid|

**响应参数**

| 字段| 类型	| 必填| 说明						|
| ---	| ---		| ---	| ---							|
| code| Number| 是	|错误码，0表示成功|
| msg	| String| 是	|详细信息					|

**示例代码**

```js
// 云函数logout代码
const uniID = require('uni-id')
exports.main = async function(event,context) {
	const {
    uniIdToken
	} = event
  payload = await uniID.checkToken(uniIdToken)
  if (payload.code && payload.code > 0) {
      return payload
  }
	const res = await uniID.logout(payload.uid)
	return res
}

```

# 数据库结构

## 用户表

表名：uni-id-users

| 字段             | 类型      | 必填 | 描述                                        |
| ---------------- | --------- | ---- | ------------------------------------------- |
| \_id             | Object ID | 是   | 存储文档 ID（用户 ID），系统自动生成        |
| username         | String    | 是   | 用户名，不允许重复                          |
| password         | String    | 否   | 密码，加密存储                              |
| nickname         | String    | 否   | 用户昵称                                    |
| gender           | Integer   | 否   | 用户性别：0 未知 1 男性 2 女性              |
| status           | Integer   | 是   | 用户状态：0 正常 1 禁用 2 审核中 3 审核拒绝 |
| mobile           | String    | 否   | 手机号码                                    |
| mobile_confirmed | Integer   | 否   | 手机号验证状态：0 未验证 1 已验证           |
| email            | String    | 否   | 邮箱地址                                    |
| email_confirmed  | Integer   | 否   | 邮箱验证状态：0 未验证 1 已验证             |
| avatar           | String    | 否   | 头像地址                                    |
| comment          | String    | 否   | 备注                                        |
| realname_auth    | Object    | 否   | 实名认证信息                                |
| register_date    | Timestamp | 否   | 注册时间                                    |
| register_ip      | String    | 否   | 注册时 IP 地址                              |
| last_login_date  | Timestamp | 否   | 最后登录时间                                |
| last_login_ip    | String    | 否   | 最后登录时 IP 地址                          |

**realNameAuth 字段定义**

| 字段            | 类型      | 必填 | 描述                                                |
| --------------- | --------- | ---- | --------------------------------------------------- |
| type            | Integer   | 是   | 用户类型：0 个人用户 1 企业用户                     |
| auth_status     | Integer   | 是   | 认证状态：0 未认证 1 等待认证 2 认证通过 3 认证失败 |
| auth_date       | Timestamp | 否   | 认证通过时间                                        |
| real_name       | String    | 否   | 真实姓名/企业名称                                   |
| identity        | String    | 否   | 身份证号码/营业执照号码                             |
| id_card_front   | String    | 否   | 身份证正面照 URL                                    |
| id_card_back    | String    | 否   | 身份证反面照 URL                                    |
| id_card_in_hand | String    | 否   | 手持身份证照片 URL                                  |
| license         | String    | 否   | 营业执照 URL                                        |
| contact_person  | String    | 否   | 联系人姓名                                          |
| contact_mobile  | String    | 否   | 联系人手机号码                                      |
| contact_email   | String    | 否   | 联系人邮箱                                          |

**job 字段定义**

| 字段    | 类型   | 必填 | 描述     |
| ------- | ------ | ---- | -------- |
| company | String | 否   | 公司名称 |
| title   | String | 否   | 职位     |

用户集合示例：

```
{
  "_id": "f2a60d815ee1da3900823d45541bb162",
  "username": "姓名"
  "password": "503005d4dd16dd7771b2d0a47aaef927e9dba89e",
  "status":0,//用户状态：0正常 1禁用 2审核中 3审核拒绝
  "mobile":"",
  "mobile_confirmed":0, //手机号是否验证，0为未验证，1为已验证
  "email":"amdin@domain.com",
  "email_confirmed":0, //邮箱是否验证，0为未验证，1为已验证
  "avatar":"https://cdn.domain.com/avatar.png"
  "register_ip": "123.120.11.128", //注册IP
  "last_login_ip": "123.120.11.128", //最后登录IP

}
```