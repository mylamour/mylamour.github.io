---
layout: post
title:  CA/RA的一点收益
categories: 安全工程师
kerywords:  企业安全 互联网企业安全
tags:  安全运维 安全架构 数据安全
---

证书系统作为PKI体系里的一部分。充分的体现了信任链的作用。虽然不一定能完全理解椭圆双曲线的加密奥妙，但是对于证书系统还是有不少东西可以放在这的。同样，如之前在KMS和HSM的博客里所提到的，本篇也依旧是简单记录一下考虑点，不会有过多的架构设计以及详细的细节。合规，证书类型，功能支持算法，强度，CA/RA部署位置（逻辑，物理，主机，数据库相关等），网络访问，系统认证方式，监控方式，证书存储介质，证书获取流程（包含CSR模板，办公网生产网证书使用策略，过期提醒更新等等），销毁方式，以及其依赖产品的相关需求。

当然也并不是所有的场景都需要自建PKI体系，或者说即便自建了PKI，也不一定需要CA、RA系统。同样，相较于金融行业，无论是互金还是传统金融，CA、RA都是比较常见的系统。

* 需要自建CA或RA吗？会有来自监管的压力吗？（自建CA和自建RA是两个不同的概念，很大一部分来自内外监管的压力，内部也有自己的规范）
* CA系统和HSM能对接吗？支持哪些厂商的HSM集成，用到了哪些功能？
* 是否能够提供离线CA的功能？如何使你的root CA能够非常安全的保存？
* 需要建设Offline CA吗？ （Offline CA是指根CA离线存放，平时用中间子CA签发证书）
* RA能支持完全的API化吗？例如国内CFCA的CA正常情况下都是以windows+IE+CFCA工具箱+证书+证书密码的方式去访问，十分繁琐）
* 正常的功能支持吗？除却算法外生成多域名时，是否会溢出。单证和双证呢？复合证书呢？吊销的CRL列表是大家都能访问到的吗？
* 是依托KMS的HSM模块，还是采购单独的HSM？
* Offline CA的部署需要HA吗，中间子CA的HA又要怎么做呢？
* 数据库要和CA/RA放在一个主机吗？
* RA/CA能放在同一个区吗？
* CA所使用的HSM能放在同一个机架上吗？
* 存证书的HSM支持网联证书的导入吗？（需要考虑整个生态里可能涉及到的东西，尤其是金融行业生态化的约束和规定过多，同样又有着各式各样的自定义体系，因此需要考虑到其后服务的业务方所面临的场景）
* Hashicorp的Vault能和HSM整合吗？ （Vault能满足自建PKI的需求吗？不支持国密算法，现在又面临了合规上的问题）
* HSM支持Microsoft ADCS调用吗？
* 证书的存储怎么设计，流程，介质？
* 证书的监控和更新周期呢？
* 测试网，生产网对证书的使用是一个什么策略？允许通配符证书吗？
* 数据库的高可用怎么做？需要你来考虑吗？是不是数据库去做会更好？
* CA系统能够自动failover吗？
* 在使用场景中，DB用的，web服务器用的，以及不同的格式对应应该怎么去做。是否存在浏览器访问场景，公签证书怎么去做？
* 全站HTTPS之外，日志收集是不是需要enable certificate传输，加签验签还是加密解密？ 
* 网联证书这种每天的自动更新证书又该怎么设计获取流程和存储方案？
* 如何使不同的终端信任，比如说生产网linux server, docker, k8, 办公网mac, windows等等，如何有效的管理和联动呢？

除了部署架构，流程管理，模板设计以及使用场景之外，在目前就职公司的工作经历中，由于很大一部分的基础设施来自于采购。也就导致了工作模式的一个差距，比如采购的A企业的证书系统，B企业的加密机，同时当证书系统需要整合加密机时，这种事情即便是作为甲方，跨企业的去沟通相关细节也是比较麻烦的。所以项目管理也占了一部分，既要把控质量，也要把控进度。还不能使不同方面觉得你盛气凌人。都是学问。

# Resources 

* [【CA】How to install root CA in your server?](https://github.com/mylamour/blog/issues/75)
* [【CA】How to build your own Certificate Authority](https://github.com/mylamour/blog/issues/74)
* [【CA】Overview of Certificate Filetype & How to covert it with Openssl](https://github.com/mylamour/blog/issues/73)
* [【CA】Generate CA with mkcert](https://github.com/mylamour/blog/issues/72)

