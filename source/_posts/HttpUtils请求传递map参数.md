---
title: HttpUtils请求传递map参数
tags:
  - 企业微信
  - Ajax
  - SpringBoot
  - HttpUtils
  - map
top_img: false
cover: https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/cover/HttpUtils.jpg
categories:
  - [BUG]
date: 2021-12-06 18:41:22
abbrlink: HttpUtils-map
---

## 问题场景

> 调用企业微信`更新成员`接口，接口要求请求参数为`map`类型

`对应接口如下`------来自[企业微信api文档](https://work.weixin.qq.com/api/doc/90000/90135/90197)

**请求方式：**POST（**HTTPS**）
**请求地址：**https://qyapi.weixin.qq.com/cgi-bin/user/update?access_token=ACCESS_TOKEN

**请求包体：**

```json
    {
        "userid": "zhangsan",
        "name": "李四",
        "department": [1],
        "order": [10],
        "position": "后台工程师",
        "mobile": "13800000000",
        "gender": "1",
        "email": "zhangsan@gzdev.com",
        "is_leader_in_dept": [1],
        "direct_leader":["lisi","wangwu"],
        "enable": 1,
        "avatar_mediaid": "2-G6nrLmr5EC3MNb_-zL1dDdzkd0p7cNliYu9V5w7o8K0",
        "telephone": "020-123456",
        "alias": "jackzhang",
        "address": "广州市海珠区新港中路",
        "main_department": 1,
        "extattr": {
            "attrs": [
                {
                    "type": 0,
                    "name": "文本名称",
                    "text": {
                        "value": "文本"
                    }
                },
                {
                    "type": 1,
                    "name": "网页名称",
                    "web": {
                        "url": "http://www.test.com",
                        "title": "标题"
                    }
                }
            ]
        },
        "external_position": "工程师",
        "external_profile": {
            "external_corp_name": "企业简称",
            "wechat_channels": {
                "nickname": "视频号名称",
            },
            "external_attr": [
                {
                    "type": 0,
                    "name": "文本名称",
                    "text": {
                        "value": "文本"
                    }
                },
                {
                    "type": 1,
                    "name": "网页名称",
                    "web": {
                        "url": "http://www.test.com",
                        "title": "标题"
                    }
                },
                {
                    "type": 2,
                    "name": "测试app",
                    "miniprogram": {
                        "appid": "wx8bd80126147dFAKE",
                        "pagepath": "/index",
                        "title": "my miniprogram"
                    }
                }
            ]
        }
    }
```

**参数说明：**

| 参数               | 必须 | 说明                                                         |
| ------------------ | ---- | ------------------------------------------------------------ |
| access_token       | 是   | 调用接口凭证                                                 |
| userid             | 是   | 成员UserID。对应管理端的帐号，企业内必须唯一。不区分大小写，长度为1~64个字节 |
| name               | 否   | 成员名称。长度为1~64个utf8字符                               |
| alias              | 否   | 别名。长度为1-64个utf8字符                                   |
| mobile             | 否   | 手机号码。企业内必须唯一。若成员已激活企业微信，则需成员自行修改（此情况下该参数被忽略，但不会报错） |
| department         | 否   | 成员所属部门id列表，不超过100个                              |
| order              | 否   | 部门内的排序值，默认为0。当有传入department时有效。数量必须和department一致，数值越大排序越前面。有效的值范围是[0, 2^32) |
| position           | 否   | 职务信息。长度为0~128个字符                                  |
| gender             | 否   | 性别。1表示男性，2表示女性                                   |
| email              | 否   | 邮箱。长度不超过64个字节，且为有效的email格式。企业内必须唯一。若是绑定了腾讯企业邮箱的企业微信，则需要在腾讯企业邮箱中修改邮箱（此情况下该参数被忽略，但不会报错） |
| telephone          | 否   | 座机。由1-32位的纯数字、“-”、“+”或“,”组成                    |
| is_leader_in_dept  | 否   | 部门负责人字段，个数必须和department一致，表示在所在的部门内是否为负责人。 |
| direct_leader      | 否   | 直属上级，可以设置企业范围内成员为直属上级，最多设置5个      |
| avatar_mediaid     | 否   | 成员头像的mediaid，通过[素材管理](https://work.weixin.qq.com/api/doc/90000/90135/90197#10112)接口上传图片获得的mediaid |
| enable             | 否   | 启用/禁用成员。1表示启用成员，0表示禁用成员                  |
| extattr            | 否   | 自定义字段。自定义字段需要先在WEB管理端添加，见[扩展属性添加方法](https://work.weixin.qq.com/api/doc/90000/90135/90197#10016/扩展属性的添加方法)，否则忽略未知属性的赋值。 |
| extattr.type       | 是   | 属性类型: 0-文本 1-网页 2-小程序                             |
| extattr.name       | 是   | 属性名称： 需要先确保在管理端有创建该属性，否则会忽略        |
| extattr.text       | 否   | 文本类型的属性                                               |
| extattr.text.value | 是   | 文本属性内容,长度限制64个UTF8字符                            |
| extattr.web        | 否   | 网页类型的属性，url和title字段要么同时为空表示清除该属性，要么同时不为空 |
| extattr.web.url    | 是   | 网页的url,必须包含http或者https头                            |
| extattr.web.title  | 是   | 网页的展示标题,长度限制12个UTF8字符                          |
| external_profile   | 否   | 成员对外属性，字段详情见[对外属性](https://work.weixin.qq.com/api/doc/90000/90135/90197#13450) |
| external_position  | 否   | 对外职务，如果设置了该值，则以此作为对外展示的职务，否则以position来展示。不超过12个汉字 |
| nickname           | 否   | 视频号名字（设置后，成员将对外展示该视频号）。须从企业绑定到企业微信的视频号中选择，可在“我的企业”页中查看绑定的视频号 |
| address            | 否   | 地址。长度最大128个字符                                      |
| main_department    | 否   | 主部门                                                       |

## 调用过程

### 前台代码

```javascript
function updateUser(){
    var url="http://***/updateUser";
    $.ajax({
        url:url,
        data:{"userid":"0120","gender":"2"},
        dataType:"json",
        type:"get",
        async: false,
        success:function(res){
			console.log(res);//这两行代码似乎有什么大病，对不齐了
		}
 	})			
}
```

### 后台代码

`controller层`

```java
@ResponseBody
@RequestMapping("/updateUser")
    public Map<String, Object> updateUser(@RequestParam("userid") String userid,@RequestParam("gender") String gender){
        Map<String, Object> map = new HashMap<>();
        String maps = slyJK.getToken();
        Map<String,Object> m = JSON.parseObject(maps);
        System.out.println(m.get("access_token"));
        String token = (String) m.get("access_token");
        System.out.println(userid);
        System.out.println(gender);
        String user = slyJK.updateUser(token,userid,gender);
        JSONObject jsonObject = JSON.parseObject(user);
        System.out.println(user);
        map.put("user",jsonObject);
        return map;
    }
```

`获取token`

```java
public String getToken(){
        String corpid="企业id";
        String corpsecret ="应用秘钥";
        String url ="https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid="+corpid+"&"+"corpsecret="+corpsecret;
        HttpUtils httpUtils = new HttpUtils();
        String result = null;
        try {
            //请求获取结果
            result = HttpUtils.getEntityStr(httpUtils.postSendHttpRequestForm(url, null));
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println("请求结果：" + result);
        return result;
    }
```

`调用接口`

```java
public String updateUser(String access_token,String userid,String gender){
        String url ="https://qyapi.weixin.qq.com/cgi-bin/user/update?access_token="+access_token+"&debug=1";
        HttpUtils httpUtils = new HttpUtils();
        Map<String, Object> params = new HashMap<>();
        params.put("userid",userid);
        params.put("gender",gender);
        String result = null;
        try {
            //请求获取结果
            result = HttpUtils.getEntityStr(httpUtils.postSendHttpRequestForm2(url, params));
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println("请求结果：" + result);
        return result;

    }
```

```java
/**
     * 通过链接获取 HttpEntity 结果JSON字符串
     * @param response CloseableHttpResponse 连接
     * @return  返回链接请求结果
     * @throws IOException IOException
     */
    public static String getEntityStr(CloseableHttpResponse response) throws IOException {
        // 获得响应的实体对象
        HttpEntity entity = response.getEntity();
        // 使用Apache提供的工具类进行转换成字符串
        return EntityUtils.toString(entity, ENCODING_UTF8);//ENCODING_UTF8="utf-8"
    }
```

```java
/**
     * 带参数POST请求
     * @param url 请求地址
     * @param param 请求携带参数
     * @return  请求对象
     * @throws IOException IOException
     */
    public CloseableHttpResponse postSendHttpRequestForm2(String url, Map<String, Object> param) throws IOException {
        return post2(url,null,param);
    }
```

```java
/**
     * POST请求
     * @param url 请求地址
     * @param header 请求头
     * @param param 请求携带参数
     * @return  请求对象
     * @throws IOException IOException
     */
    public CloseableHttpResponse post2(String url, Map<String, String> header,Map<String,Object> param) throws IOException {
        //1.创建POST请求对象
        HttpPost httpPost = new HttpPost(url);
		//2.处理参数
        Gson gson = new Gson();
        httpPost.setEntity(new StringEntity(gson.toJson(param),"utf-8"));
        //3.添加请求头信息
        // 浏览器表示
        httpPost.addHeader("User-Agent", "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)");
        // 传输的类型
        httpPost.addHeader("Content-Type", "application/x-www-form-urlencoded");
        // 自定义请求头
        if(null != header && !header.isEmpty()){
            for (String head:header.keySet()){
                httpPost.addHeader(head,header.get(head));
            }
        }
        // 执行请求
        return this.response = this.httpClient.execute(httpPost);
    }
```

## 重点代码

> 这里的`Gson`包来自import com.google.gson.Gson;

```java
Gson gson = new Gson();
httpPost.setEntity(new StringEntity(gson.toJson(param),"utf-8"));
```

## 相关依赖

```xm
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.8.6</version>
</dependency>
```

## 总结

- 我对这些`list`和`map`之类的也不是很理解，对`httputils`更不理解，反正最后接口调用成功了，企业微信里面的数据也更新了，记录一下。
- 企业微信成员中的`mobile`不能通过接口修改，只能在后台中修改
- 与成员相关的接口要用到通讯录的`secret`，记得开通
