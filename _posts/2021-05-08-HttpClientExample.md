---
layout:     post   				    # 使用的布局（不需要改）
title:      HttpClient模板		   	# 标题 
subtitle:             # 副标题
date:       2021-05-08 				# 时间
author:     Nathan 				# 作者
header-img: img/post-bg-unity-dev.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Unity开发
    - .NET
---

# HttpClient模板

## 引言

众所周知Unity本身的一些Library写的又烂限制又多，而call api几乎是unity开发不可避免的一个部分，所幸System.Net.Http在Build后不会有什么运行上的问题，那自然要用.NET的原生http方法了。

## 代码

这里的代码是一个登录和注册的API Call，有一个额外的dependency是Newtonsoft.Json，这个package请到[这里]("https://assetstore.unity.com/packages/tools/input-management/json-net-for-unity-11347?locale=zh-CN")下载

非Async
```
using Newtonsoft.Json;
using System;
using System.Net.Http;
using System.Text;
using UnityEngine;

public class UserAccountHttpClient
{
    private const string signupUrl = "http://url/signup";
    private const string loginUrl = "http://url/login";
    private HttpClient httpClient;

    public UserAccountHttpClient()
    {
        httpClient = new HttpClient();
    }

    public string Login(UserAccount userAccount)
    {
        try
        {
            // 定义HttpMethod(Post, Put, Get or Delete)
            var httpRequest = new HttpRequestMessage(HttpMethod.Post, loginUrl);
            // 构建HttpRequest content，这里用到了Newtonsoft.Json serialize content。 
            httpRequest.Content = new StringContent(JsonConvert.SerializeObject(userAccount), Encoding.Default, "application/json");
            var response = httpClient.SendAsync(httpRequest).Result;
            var result = response.Content.ReadAsStringAsync().Result.ToString();
            Debug.Log($"Login result: {result}");
            return result;
        }
        catch (Exception ex)
        {
            Debug.LogError(ex);
        }
        return "";
    }

    public string Signup(UserAccount userAccount)
    {
        try
        {
            var httpRequest = new HttpRequestMessage(HttpMethod.Post, signupUrl);
            httpRequest.Content = new StringContent(JsonConvert.SerializeObject(userAccount), Encoding.UTF8, "application/json");
            var response = httpClient.SendAsync(httpRequest).Result;
            var result = response.Content.ReadAsStringAsync().Result.ToString();
            Debug.Log($"Login result: {result}");
            return result;
        }
        catch (Exception ex)
        {
            Debug.LogError(ex);
        }
        return "";
    }
}
```

最后再放一个Async的方法
```
public async Task<string> PostTestContentAsync(MessageRequestBody content)
{
    var result = string.Empty;
    try
    {
        var httpRequest = new HttpRequestMessage(HttpMethod.Post, testUrl);
        httpRequest.Content = new StringContent(JsonConvert.SerializeObject(content), Encoding.UTF8, "application/json");
        var response = await httpClient.SendAsync(httpRequest);
        result = response.Content.ReadAsStringAsync().Result;
    }
    catch(Exception ex)
    {
        Debug.LogError(ex);
    }
    return result;
}
```

## 阅读材料
- https://www.cnblogs.com/wywnet/p/httpclient.html
- https://blog.csdn.net/qq_41708190/article/details/106554732