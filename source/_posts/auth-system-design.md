---
title: 认证授权系统设计【零】认证、授权、鉴权和权限控制
date: 2023-03-25 20:37:01
categories:
- system-design
tags:
- Authentication & Authorization
---

## 认证
认证是指根据声明者所特有的识别信息，确认声明者的身份。认证在英文中对应于identification 这个单词。

## 授权
简单来说，授权一般是指获取用户的委派权限。在英文中对应于 authorization 这个单词。

## 鉴权
鉴权是指对于一个声明者所声明的身份权利，对其所声明的真实性进行鉴别确认的过程。
在英文中对应于 authentication 这个单词。

## 权限控制
权限控制是指对可执行的各种操作组合配置为权限列表，然后根据执行者的权限，若其操作在权限范围内，
则允许执行，否则禁止。权限控制在英文中对应于 access/permission control 。