# 简介

维格表外呼小程序，是一个基于维格小程序，集成腾讯云呼叫中心，使用户可方便的在维格表中一键进行外呼，同时能够把电话外呼的通话信息及通话录音回写到维格表中的一套程序。

可应用于维格表的CRM等需要进行电话外呼的应用场景。

其业务逻辑如下：

![](https://vikadata.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDQ4YWU2MmUzMTZhMDM4MzAyZTRhMjM4NDg0OTZjNjlfcE52eFJZVG9VakxmR2w1ZHFOYXVzWTRYZ21BOFYzQnZfVG9rZW46Ym94Y25uVVFaODdDVG5waVdLQjlBMkEyanBjXzE2NzMzMzk5Mzc6MTY3MzM0MzUzN19WNA)

# 安装与使用

1. ### 前置准备

* 注册腾讯云呼叫中心
* 本地node环境，便于发布维格小程序

2. ### 设置Hiflow流程

分别设置两个触发条件为webhook的Hiflow流程，用于接受CDR数据及录音数据，然后调用维格表API将数据写入维格表中。

以下两个Hiflow流程供参考（）：

[CDR数据回写](https://hiflow.tencent.com/share/p8ghNPvskjbieS9nt9NuC8Zr2iua6fdx)：uuid为维格小程序传输的触发外呼的维格表记录的表格ID及recordID，用于将CDR数据写入到维格表后关联原触发外呼的那条记录

[录音数据回写](https://hiflow.tencent.com/share/cHDeCetauQB7nuUK8KcoX1SjI1DqISiC)

3. ### 初始化服务端参数

服务端代码请在此处下载：[点击下载](https://s1.vika.cn/space/2023/01/09/24309f367840473a97f13540b3b59254?attname=vika-cloudcall-server.zip)

* 在data.json文件中，分别填入腾讯云呼叫中心的sdkAppId、sdkAppId、sdkAppId、secretKey获取方法参考：https://cloud.tencent.com/document/product/679/72042
* 在data.json文件中，分别填入recordUrl、cdrUrl。分别对应Hiflow中写入录音数据的流程的webhook地址及CDR数据的webhook地址。

4. ### 修改服务端代码并部署

然后可将服务端代码进行部署。可适用nodejs本地化部署或自己的服务器上，也可以部署至腾讯云函数。

本地部署方法参考：[Node.js 创建第一个应用 | 菜鸟教程](https://www.runoob.com/nodejs/nodejs-http-server.html)

完成部署后，将得到两个接口的调用地址，例如：https://xxxxxxx.xx/getToken 以及：https://xxxxxxx.xx/sendToHiflow

5. ### 初始化维格小程序参数

![](https://vikadata.feishu.cn/space/api/box/stream/download/asynccode/?code=MjBhZjY2ODFkOGNhMWE5ZmZjN2Y2YWI2ZTMwMjE1NmFfWDNWQVB2VnNVTVdiUmd4Sjd6SGJjMFNoOU43dmVQaDBfVG9rZW46Ym94Y25IYlI3aGdKWEFiVDFFeHd6SGE3WUdRXzE2NzMzMzk5Mzc6MTY3MzM0MzUzN19WNA)

* 初始化users.js内数据，其中，name为用户在维格表中的昵称；id为腾讯云呼叫中心管理端中，每个客户账号的邮箱。该文件的作用是将维格表用户与腾讯云呼叫中心用户对应起来，来确保呼叫席位只能被某一位同事使用。
* 维格小程序在部署前还需要初始化一些spaceID、packageID等参数，参考官网维格表部署教程。

6. ### 部署与发布维格表小程序

参考维格官网小程序文档：https://vika.cn/developers/widget/quick-start

7. ### 使用

在部署了外呼小程序的空间站内，添加此小程序。此时如果当前登录用户是我们在users.js中指定的用户，小程序将自动完成初始化。

然后选择当前表格中，点击拨打即可。
