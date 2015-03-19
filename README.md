# 母爱网3期手机端api接口开发者文档03.18

## 测试配置：
* 内网人员
    将hosts增加新纪录：
    `sdk.muai.com 192.168.111.250`
* 外网人员
    1. 请安装hamachi（https://secure.logmein.com/products/hamachi/download.aspx）
    2. 加入组“muai3”密码“muai3”
    3. 将hosts增加新纪录：
        `sdk.muai.com 25.17.56.171`

## 公共约定
* 绿色字体接口由原生app调用
* api网关url：`http://sdk.muai.com/app`
* 参数传递方式：`通过get或post传递均可。`
### 调用结果格式说明：
所有的api调用将统一返回如下结果的jsonp格式：
```
xxx({
"error_code": 300,
"error_data": {},
"error_msg": "无效操作",
"performance": 0.0,
"result": {}
,"state":""})
```
xxx为js发起端回调函数，通过`request["callback"]`指定，此参数可以为空。

    error_code 错误代码：
    	操作成功 = 0,
    	授权错误 = 100,
    	参数错误 = 200,
    	无效调用 = 300,
    	系统异常 = 400
    error_data：仅当error_code不为0时起作用，表示错误的详细数据（集合类型）。
    error_msg：仅当error_code不为0时起作用，表示错误消息。
    performance：用于调试版输出性能统计数据，忽略之。
    result：仅当error_code为0时起作用，表示api返回的结果（集合类型）。
    state：将request["state"]参数原样返回。
    
**注意：如果需要作为json格式解析，请在调用端中去掉jsonp结果的首尾括弧即可。**

### 用户授权：
如果接口描述中标注了 `需要登录：是` 则需要传递 `request["_ui"]` 参数，或在 `request.cookie` 中
设置 `ma_user_identity` 参数，该参数的值通过登录接口获得。

---
## 接口列表
### 用户填写支付宝号（只能填写一次，即只有在用户新注册支付宝未空的情况下调用才能正常返回）
接口地址：http://sdk.muai.com/app/private_/alipay_set
需要登录：是
输入参数：
```
alipay  用户新支付宝号
```
返回信息：
```
无
```


---


### 用户设置昵称和密码（只能设置一次，即只有在用户新注册，密码为初始值时调用才能正常返回）
接口地址：http://sdk.muai.com/app/private_/reset_nick_pwd
需要登录：是
输入参数：
```
nick	昵称
pwd	    密码
```
返回信息：
```
无
```

---

### 用户绑定手机号（只能绑定一次，即只有在用户是通过qq注册，手机号为空的时候调用才能正常返回）
接口地址：http://sdk.muai.com/app/private_/bind_mobile
需要登录：是
输入参数：
```
mobile	手机号码
code	验证码
```
返回信息：
```
无
```

---

### 用户绑定qq（只能绑定一次，即只有在用户是通过手机注册，openid为空的时候调用才能正常返回）
接口地址：http://sdk.muai.com/app/private_/bind_qq
需要登录：是
输入参数：
```
openid	通过qq第三方登录授权oauth2返回
```
返回信息：
```
无
```

---


### 用户交易记录
接口地址：http://sdk.muai.com/app/private_/charge_lst
需要登录：是
输入参数：
```
page_no	页码
```
返回信息：
```
Item1:
	交易记录
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_uid	rs_uid	用户id	int			FALSE	FALSE	FALSE	TRUE
	rs_gold	rs_gold	交易金币	int			FALSE	FALSE	FALSE	TRUE
	rs_gold_old	rs_gold_old	交易前金币余额	int			FALSE	FALSE	FALSE	TRUE
	rs_gold_new	rs_gold_new	交易后金币余额	int			FALSE	FALSE	FALSE	TRUE
	rs_event	rs_event	事件	int			FALSE	FALSE	FALSE	TRUE
	rs_remark	rs_remark	备注	sysname			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2:
    总记录数
```

---

### 获得月龄清单分类
接口地址：http://sdk.muai.com/app/public_/class_lst
需要登录：否
输入参数：
```
无
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_tid	rs_tid	帖子id	int			FALSE	TRUE	FALSE	TRUE
rs_cid	rs_cid	分类id	int			FALSE	TRUE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
```

---

### 发送验证码
接口地址：http://sdk.muai.com/app/public_/create_code
需要登录：否
输入参数：
```
mobile	手机号码
action	发送原因（8个字符内）
```
返回信息：
```
无
```

---

### 用户提现
接口地址：http://sdk.muai.com/app/private_/draw
需要登录：是
输入参数：
```
gold	提现的金币数
```
返回信息：
```
无
```


---


### 获取值得买商品列表
接口地址：http://sdk.muai.com/app/public_/goods_lst
需要登录：可选
输入参数：
```
page_no	    页码
sort_field	排序字段
sort_desc	是否倒序（true，false）
```
返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_third_no	rs_third_no	第三方商品编码	varchar(32)	32		FALSE	FALSE	FALSE	TRUE
	rs_platform	rs_platform	商品所属平台	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_title	rs_title	商品标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_name	rs_name	友好名称	nvarchar(32)	32		FALSE	FALSE	FALSE	TRUE
	rs_subtitle	rs_subtitle	子标题	sysname			FALSE	FALSE	FALSE	TRUE
	rs_remark	rs_remark	描述	nvarchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_exlink	rs_exlink	外部链接	nvarchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_recommend	rs_recommend	首页推荐	bit			FALSE	FALSE	FALSE	TRUE
	rs_price_origin	rs_price_origin	原价	decimal(8,4)	8	4	FALSE	FALSE	FALSE	TRUE
	rs_price_new	rs_price_new	现价	decimal(8,4)	8	4	FALSE	FALSE	FALSE	TRUE
	rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
	rs_cat	rs_cat	商品归类	nvarchar(8)	8		FALSE	FALSE	FALSE	TRUE
	rs_img	rs_img	商品图片	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
	rs_type	rs_type	商品类型	int			FALSE	FALSE	FALSE	TRUE
	rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2：
	总记录数
```


---

### 获取值得买专场的商品
接口地址：http://sdk.muai.com/app/public_/goods_son
需要登录：否
输入参数：
```
gid	    专场id
page_no	页码
```
返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_third_no	rs_third_no	第三方商品编码	varchar(32)	32		FALSE	FALSE	FALSE	TRUE
	rs_platform	rs_platform	商品所属平台	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_title	rs_title	商品标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_name	rs_name	友好名称	nvarchar(32)	32		FALSE	FALSE	FALSE	TRUE
	rs_subtitle	rs_subtitle	子标题	sysname			FALSE	FALSE	FALSE	TRUE
	rs_remark	rs_remark	描述	nvarchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_exlink	rs_exlink	外部链接	nvarchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_recommend	rs_recommend	首页推荐	bit			FALSE	FALSE	FALSE	TRUE
	rs_price_origin	rs_price_origin	原价	decimal(8,4)	8	4	FALSE	FALSE	FALSE	TRUE
	rs_price_new	rs_price_new	现价	decimal(8,4)	8	4	FALSE	FALSE	FALSE	TRUE
	rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
	rs_cat	rs_cat	商品归类	nvarchar(8)	8		FALSE	FALSE	FALSE	TRUE
	rs_img	rs_img	商品图片	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
	rs_type	rs_type	商品类型	int			FALSE	FALSE	FALSE	TRUE
	rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2：
	总记录数
```

---


### 获取月龄周刊
接口地址：http://sdk.muai.com/app/public_/knowledge_get
需要登录：否
输入参数：
```
week	周龄
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_week_start	rs_week_start	起始周龄	int			FALSE	FALSE	FALSE	TRUE
rs_week_end	rs_week_end	结束周龄	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_content	rs_content	内容	nvarchar(max)			FALSE	FALSE	FALSE	TRUE
```

---

### 用户登录
接口地址：http://sdk.muai.com/app/public_/login
需要登录：否
输入参数：
```
mobile	手机号码
pwd	密码
```

返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_openid	rs_openid	openid	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_mobile	rs_mobile	手机号	varchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_nick	rs_nick	昵称	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_pwd	rs_pwd	密码	uniqueidentifier			FALSE	FALSE	FALSE	TRUE
	rs_avatar	rs_avatar	头像	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_birthday	rs_birthday	宝宝生日	datetime			FALSE	FALSE	FALSE	TRUE
	rs_sex	rs_sex	宝宝性别	bit			FALSE	FALSE	FALSE	TRUE
	rs_signday	rs_signday	连续签到天数	int			FALSE	FALSE	FALSE	TRUE
	rs_gold	rs_gold	金币	int			FALSE	FALSE	FALSE	TRUE
	rs_alipay	rs_alipay	支付宝	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2:
	用户授权字符串，登录完成后缓存在手机端。调用需要登录的api时，在request["_ui"]参数中填写此值
```

---

### qq免注册登录
接口地址：http://sdk.muai.com/app/public_/login_by_qq
需要登录：否
输入参数：
```
openid	qq授权oauth2获得的openid
nick	qq授权oauth2获得的qq昵称
avatar	qq授权oauth2获得的头像地址
```
返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_openid	rs_openid	openid	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_mobile	rs_mobile	手机号	varchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_nick	rs_nick	昵称	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_pwd	rs_pwd	密码	uniqueidentifier			FALSE	FALSE	FALSE	TRUE
	rs_avatar	rs_avatar	头像	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_birthday	rs_birthday	宝宝生日	datetime			FALSE	FALSE	FALSE	TRUE
	rs_sex	rs_sex	宝宝性别	bit			FALSE	FALSE	FALSE	TRUE
	rs_signday	rs_signday	连续签到天数	int			FALSE	FALSE	FALSE	TRUE
	rs_gold	rs_gold	金币	int			FALSE	FALSE	FALSE	TRUE
	rs_alipay	rs_alipay	支付宝	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2:
	用户授权字符串，登录完成后缓存在手机端。调用需要登录的api时，在request["_ui"]参数中填写此值
```


---


### 通过手机号注册
接口地址：http://sdk.muai.com/app/public_/register
需要登录：否
输入参数：
```
mobile	手机号
code	验证码
```
返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_openid	rs_openid	openid	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_mobile	rs_mobile	手机号	varchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_nick	rs_nick	昵称	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
	rs_pwd	rs_pwd	密码	uniqueidentifier			FALSE	FALSE	FALSE	TRUE
	rs_avatar	rs_avatar	头像	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_birthday	rs_birthday	宝宝生日	datetime			FALSE	FALSE	FALSE	TRUE
	rs_sex	rs_sex	宝宝性别	bit			FALSE	FALSE	FALSE	TRUE
	rs_signday	rs_signday	连续签到天数	int			FALSE	FALSE	FALSE	TRUE
	rs_gold	rs_gold	金币	int			FALSE	FALSE	FALSE	TRUE
	rs_alipay	rs_alipay	支付宝	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
Item2:
	用户授权字符串，登录完成后缓存在手机端。调用需要登录的api时，在request["_ui"]参数中填写此值
```

---

### 发布晒单
接口地址：http://sdk.muai.com/app/private_/show_add
需要登录：是
输入参数：
```
body1	内容1段
body2	内容2段
body3	内容3段
body4	内容4段
body5	内容5段
body6	内容6段
body7	内容7段
body8	内容8段
img11	图片1-1
img12	图片1-2
img13	图片1-3
img21	图片2-1
img22	图片2-2
img23	图片2-3
img31	图片3-1
img32	图片3-2
img33	图片3-3
img41	图片4-1
img42	图片4-2
img43	图片4-3
img51	图片5-1
img52	图片5-2
img53	图片5-3
img61	图片6-1
img62	图片6-2
img63	图片6-3
img71	图片7-1
img72	图片7-2
img73	图片7-3
img81	图片8-1
img82	图片8-2
img83	图片8-3
title	标题
```
返回信息：
```
无
```

---

### 编辑晒单
接口地址：http://sdk.muai.com/app/private_/show_edit
需要登录：是
输入参数：
```
id	    晒单id
body1	内容1段
body2	内容2段
body3	内容3段
body4	内容4段
body5	内容5段
body6	内容6段
body7	内容7段
body8	内容8段
img11	图片1-1
img12	图片1-2
img13	图片1-3
img21	图片2-1
img22	图片2-2
img23	图片2-3
img31	图片3-1
img32	图片3-2
img33	图片3-3
img41	图片4-1
img42	图片4-2
img43	图片4-3
img51	图片5-1
img52	图片5-2
img53	图片5-3
img61	图片6-1
img62	图片6-2
img63	图片6-3
img71	图片7-1
img72	图片7-2
img73	图片7-3
img81	图片8-1
img82	图片8-2
img83	图片8-3
title	标题
```
返回信息：
```
无
```

---

### 获得晒单
接口地址：http://sdk.muai.com/app/public_/show_get
需要登录：否
输入参数：
```
showid	晒单id
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_uid	rs_uid	用户id	int			FALSE	FALSE	FALSE	TRUE
rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
rs_body1	rs_body1	内容1段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img11	rs_img11	图1-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img12	rs_img12	图1-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img13	rs_img13	图1-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body2	rs_body2	内容2段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img21	rs_img21	图2-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img22	rs_img22	图2-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img23	rs_img23	图2-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body3	rs_body3	内容3段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img31	rs_img31	图3-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img32	rs_img32	图3-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img33	rs_img33	图3-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body4	rs_body4	内容4段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img41	rs_img41	图4-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img42	rs_img42	图4-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img43	rs_img43	图4-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body5	rs_body5	内容5段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img51	rs_img51	图5-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img52	rs_img52	图5-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img53	rs_img53	图5-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body6	rs_body6	内容6段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img61	rs_img61	图6-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img62	rs_img62	图6-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img63	rs_img63	图6-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body7	rs_body7	内容7段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img71	rs_img71	图7-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img72	rs_img72	图7-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img73	rs_img73	图7-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body8	rs_body8	内容8段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img81	rs_img81	图8-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img82	rs_img82	图8-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img83	rs_img83	图8-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
```

---

### 用户的晒单列表
接口地址：http://sdk.muai.com/app/private_/show_lst
需要登录：是
输入参数：
```
page_no	页码
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_uid	rs_uid	用户id	int			FALSE	FALSE	FALSE	TRUE
rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
rs_body1	rs_body1	内容1段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img11	rs_img11	图1-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img12	rs_img12	图1-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img13	rs_img13	图1-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body2	rs_body2	内容2段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img21	rs_img21	图2-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img22	rs_img22	图2-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img23	rs_img23	图2-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body3	rs_body3	内容3段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img31	rs_img31	图3-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img32	rs_img32	图3-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img33	rs_img33	图3-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body4	rs_body4	内容4段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img41	rs_img41	图4-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img42	rs_img42	图4-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img43	rs_img43	图4-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body5	rs_body5	内容5段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img51	rs_img51	图5-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img52	rs_img52	图5-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img53	rs_img53	图5-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body6	rs_body6	内容6段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img61	rs_img61	图6-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img62	rs_img62	图6-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img63	rs_img63	图6-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body7	rs_body7	内容7段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img71	rs_img71	图7-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img72	rs_img72	图7-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img73	rs_img73	图7-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_body8	rs_body8	内容8段	nvarchar(3072)	3,072		FALSE	FALSE	FALSE	TRUE
rs_img81	rs_img81	图8-1	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img82	rs_img82	图8-2	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_img83	rs_img83	图8-3	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
```

---



### 用户签到
接口地址：http://sdk.muai.com/app/private_/sign
需要登录：是
输入参数：
```
无
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_uid	rs_uid	用户id	int			FALSE	FALSE	FALSE	TRUE
rs_gold	rs_gold	获得金币数	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
```

---


### 单个帖子
接口地址：http://sdk.muai.com/app/public_/topic_get
需要登录：否
输入参数：
```
无
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
rs_img	rs_img	主图	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
rs_star	rs_star	星级	int			FALSE	FALSE	FALSE	TRUE
rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_content	rs_content	内容	nvarchar(max)			FALSE	FALSE	FALSE	TRUE
```

---

### 帖子列表
接口地址：http://sdk.muai.com/app/public_/topic_lst
需要登录：可选
输入参数：
```
page_no	页码
cid	类目id【可空】
```
返回信息：
```
Item1：
	Item1：主帖对象：
		Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
		rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
		rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
		rs_img	rs_img	主图	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
		rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
		rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
		rs_star	rs_star	星级	int			FALSE	FALSE	FALSE	TRUE
		rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
		rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
		rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
		rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
		rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
		rs_content	rs_content	内容	nvarchar(max)			FALSE	FALSE	FALSE	TRUE
	item2：子帖列表：
		Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
		rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
		rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
		rs_img	rs_img	主图	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
		rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
		rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
		rs_star	rs_star	星级	int			FALSE	FALSE	FALSE	TRUE
		rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
		rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
		rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
		rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
		rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
		rs_content	rs_content	内容	nvarchar(max)			FALSE	FALSE	FALSE	TRUE
Item2：
	总记录数
```

---

### 子帖列表
接口地址：http://sdk.muai.com/app/public_/topic_son
需要登录：否
输入参数：
```
tid	主帖id
```
返回信息：
```
Item1：
	Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
	rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
	rs_title	rs_title	标题	nvarchar(64)	64		FALSE	FALSE	FALSE	TRUE
	rs_img	rs_img	主图	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
	rs_click	rs_click	点击数	int			FALSE	FALSE	FALSE	TRUE
	rs_digg	rs_digg	点赞数	int			FALSE	FALSE	FALSE	TRUE
	rs_star	rs_star	星级	int			FALSE	FALSE	FALSE	TRUE
	rs_month_start	rs_month_start	起始月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_month_end	rs_month_end	结束月龄	int			FALSE	FALSE	FALSE	TRUE
	rs_order	rs_order	排序值	int			FALSE	FALSE	FALSE	TRUE
	rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
	rs_content	rs_content	内容	nvarchar(max)			FALSE	FALSE	FALSE	TRUE
Item2：
	总记录数
```

---

### 获取当前登录用户信息
接口地址：http://sdk.muai.com/app/private_/user_get
需要登录：是
输入参数：
```
无
```
返回信息：
```
Name	Code	Comment	Data Type	Length	Precision	Identity	Primary	Foreign Key	Mandatory
rs_id	rs_id	主键	int			TRUE	TRUE	FALSE	TRUE
rs_openid	rs_openid	openid	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
rs_mobile	rs_mobile	手机号	varchar(16)	16		FALSE	FALSE	FALSE	TRUE
rs_nick	rs_nick	昵称	nvarchar(16)	16		FALSE	FALSE	FALSE	TRUE
rs_pwd	rs_pwd	密码	uniqueidentifier			FALSE	FALSE	FALSE	TRUE
rs_avatar	rs_avatar	头像	varchar(1024)	1,024		FALSE	FALSE	FALSE	TRUE
rs_birthday	rs_birthday	宝宝生日	datetime			FALSE	FALSE	FALSE	TRUE
rs_sex	rs_sex	宝宝性别	bit			FALSE	FALSE	FALSE	TRUE
rs_signday	rs_signday	连续签到天数	int			FALSE	FALSE	FALSE	TRUE
rs_gold	rs_gold	金币	int			FALSE	FALSE	FALSE	TRUE
rs_alipay	rs_alipay	支付宝	varchar(64)	64		FALSE	FALSE	FALSE	TRUE
rs_status	rs_status	状态	int			FALSE	FALSE	FALSE	TRUE
rs_time	rs_time	记录创建时间	datetime			FALSE	FALSE	FALSE	TRUE
rs_update	rs_update	记录更新时间	datetime			FALSE	FALSE	FALSE	TRUE
```
