# 学生作业管理系统

### 功能

1. 管理员在后台数据库中准备好学生学号（10位）后，学生可以用学号登录系统，首次登录需要自行设置密码（大于8位）
2. 学生可以上传文件到系统中，上传功能基于bootstrap-fileinput
3. 后台统一命名存储文件
4. 管理员登陆后可以批量下载后台打包过后的文件
5.添加截止时间设置，截止时间过后学生无法上传作业
    ```
    ALTER TABLE `shw`.`orderinfo` 
    ADD COLUMN `odeadline` DATETIME NOT NULL DEFAULT '2022-01-01 00:00:00' AFTER `otime`;
    ```
6. 管理员删除作业任务时，增加确认对话框
7. 添加编辑作业任务功能
8. 作业名称可以用[]()添加链接

## 架构

### 前端

1. jQuery
2. Bootstrap3
3. Bootstrap-fileinput
4. moment.js
5. bootstrap-datepicker (bootstrap 3 v4)

### 后端

1. Spring MVC
2. Spring
3. Mybatis
4. Shiro （安全框架）
5. Druid（阿里巴巴的开源连接池）
6. MySql

## 部署


1. 运行

   ```
   mvn install
   mvn package
   ```

   项目使用的Tomcat版本为8.5.20。

2. 查看Druid管理面板

   默认用户名：``itning``

   默认密码：``kingston``

   页面：``http://localhost:8080/druid``

  

## SQL

1. 创建数据库

   ```sql
   CREATE DATABASE IF NOT EXISTS shw CHARACTER SET utf8mb4;
   USE shw;
   ```

2. 导入表结构和数据

   ```sql
   SET FOREIGN_KEY_CHECKS=0;
   
   -- ----------------------------
   -- Table structure for history
   -- ----------------------------
   DROP TABLE IF EXISTS `history`;
   CREATE TABLE `history` (
     `hid` varchar(255) NOT NULL,
     `huid` varchar(255) NOT NULL,
     `hoid` int(11) NOT NULL,
     `type` varchar(255) NOT NULL,
     `filepath` varchar(255) NOT NULL,
     `uptime` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP,
     `filesize` double NOT NULL,
     PRIMARY KEY (`hid`),
     KEY `FK_hoid_oid` (`hoid`) USING BTREE,
     KEY `FK_huid_uid` (`huid`) USING BTREE,
     CONSTRAINT `history_ibfk_1` FOREIGN KEY (`hoid`) REFERENCES `orderinfo` (`oid`),
     CONSTRAINT `history_ibfk_2` FOREIGN KEY (`huid`) REFERENCES `user` (`uid`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   
   -- ----------------------------
   -- Table structure for orderinfo
   -- ----------------------------
   DROP TABLE IF EXISTS `orderinfo`;
   CREATE TABLE `orderinfo` (
     `oid` int(11) NOT NULL,
     `oname` varchar(255) NOT NULL,
     `osubject` varchar(255) NOT NULL,
     `ostate` bit(1) NOT NULL,
     `otime` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP,
     `odeadline` datetime NOT NULL ON UPDATE CURRENT_TIMESTAMP,
     PRIMARY KEY (`oid`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   
   -- ----------------------------
   -- Records of orderinfo
   -- ----------------------------
   INSERT INTO `orderinfo` VALUES ('1492109980', '第二次作业', 'UI交互设计', 1, '2018-11-28 14:48:53');
   INSERT INTO `orderinfo` VALUES ('795960272', '第二次作业', '软件测试', 1, '2018-11-28 14:38:11');
   
   -- ----------------------------
   -- Table structure for user
   -- ----------------------------
   DROP TABLE IF EXISTS `user`;
   CREATE TABLE `user` (
     `uid` varchar(255) NOT NULL,
     `username` varchar(255) NOT NULL,
     `password` varchar(255) NOT NULL,
     `headimg` varchar(255) DEFAULT NULL,
     `firstlogin` bit(1) NOT NULL DEFAULT b'1',
     `name` varchar(255) NOT NULL,
     `percode` varchar(255) NOT NULL,
     `userOpenID` varchar(255) DEFAULT NULL,
     PRIMARY KEY (`uid`)
   ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
   
   -- ----------------------------
   -- Records of user
   -- ----------------------------
   INSERT INTO `user` VALUES ('1', '000000000000', '0123456789', null, 1, '管理员', 'admin', null);
   INSERT INTO `user` VALUES ('2', '111111111111', '123456789', null, 1, '用户1', 'user', null);
   ```

   
