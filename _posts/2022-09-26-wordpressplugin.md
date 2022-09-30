---
layout:     post
title:      "Wordpress实用插件"
subtitle:   "简单整理"
date:       2022-09-26
author:     "Yvan"
header-img: "img/bg/wp-bg.jpg"
header-mask: 0.3
header-img-credit: "Dok Sev from Pixabay"
header-img-credit-href:  "pixabay.com/de/users/doki7-646987/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=581849"
no-catalog: false
mathjax: true
category: "code"
tags:
    - Wordpress
    - 前端
    - Javascript
    - Html
    - PHP
---

1. Admin Menu Editor
    - 用于改变Wordpress控制台左侧的菜单排序
    - 可以把不用的功能往后面排，或者隐藏
    - 防止新增插件导致菜单混乱
2. WP Maintenance Mode & Coming Soon
    - 用于“下线”网站，维护或者改变时，拉起帷幕，防止带来不好的影响
3. [Simple WordPress Membership](https://wordpress.org/plugins/simple-membership/) & Simple Membership Google recaptcha
    - 用于管理网站成员
    - 简单的登录、注册、更改密码等
    - 使特定网页对某个级别的成员可见或不可见
4. [WP User Frontend](https://wordpress.org/plugins/wp-user-frontend/)
    - 使用表格的形式对网站成员开放Post撰写更改和删除功能
    - 防止成员进入Wordpress控制台，而不小心搞坏某个东西
5. [WP Data Access](https://wordpress.org/plugins/wp-data-access/)
    - 对Wordpress中所有database进行快速访问修改和删除
    - 在后台可视化这些database
6. [Spreadsheet Integration – Connect WordPress, WooCommerce & Most popular Form Plugins With Google Sheets.](https://wordpress.org/plugins/wpgsi/)
    - 连接database和google sheets
    - 需按步骤在谷歌开发者平台开通权限，得到密钥等
7. [CF7 Google Sheet Connector](https://cn.wordpress.org/plugins/cf7-google-sheets-connector/)
    - 连接Contact Form7和google sheets
    - 操作简单。授权时别忘了要同一所有权限。
8. [Contact Form CFDB7 / Contact Form 7 Database Addon – CFDB7](https://cn.wordpress.org/plugins/contact-form-cfdb7/)
    - 建立数据库wp_db7_forms
    - 数据不会按照表的项目生成attribute，而是一个记录：提交id，表单id，所有内容，提交时间时间
    - 在“所有内容中”包含表的所有attribute
9. [Database for Contact Form 7 / Contact Form 7 Database Lite](https://wordpress.org/plugins/cf7-database/)
    - 建立数据库wp_cf7_data，记录：提交id，提交时间
    - 建立数据库wp_cf7_data_entry，记录：条目id，提交id，表单id，attribute_name，value（即一个表单会被分为多行，每行一个attribute和一个value）
    - 可以在该插件自带的页面修改表单，并使该插件生成的数据库中的信息同步修改

    
借助5-9,可以实现 **联系表单**，**Google Sheet** 和 **Database** 之间的三角形链接。

