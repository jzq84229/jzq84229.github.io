---
layout: post
title: 学习笔记——retrofit笔记
category: 笔记
tags: retrofit
keywords:
description:
---

retrofit使用步骤:

1. 定义Http接口
```
//定义HTTP接口
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```
2. 新建Retrofit对象，并获取Http接口对象
```
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```
3. 使用Http接口对象调用接口方法
```
all<List<Repo>> repos = service.listRepos("octocat");
```
