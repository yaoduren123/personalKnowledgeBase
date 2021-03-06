---
title: web服务接入网关规范
date: 2022-03-28 16:22
---
**Web服务 需要满足以下几个条件**
- 该服务的所有 URI 必须有区别于其他服务的 唯一起始路径，例如：/api/zppassport/、/wapi/zppassport/ 和 /mapi/zppassport/ 
- 该服务的所有为 App 服务的接口必须以 /api/ 打头，为 H5/Web 服务的接口必须以 /wapi/ 打头，为小程序服务的接口必须以 /mapi/ 打头
- 该服务必须暴露一个用于健康监测的 HTTP 接口，并且满足以下条件时网关认为该服务处于健康状态：
    - 在 1s 内返回
    - HTTP Response Status 为 200
    - HTTP Response Body 不为空。
- 该服务返回给 App 和 H5/Web 的 JSON 响应格式必须为： ``{"code":0, "message":"Success", "zpData":{}}``， 其中
 *code * 代表响应码，每个业务的响应码区间不同，并且除了 0 和 1 以外，业务代码不能返回 1000 以内的 code（1000 以内的 code 为系统响应码）
 *message*  为对应 code 的提示语
 *zpData*  是具体业务的响应内容并且必须是 JSONObject，不能是数组、字符串、基本类型或者 null
该服务不要对响应做压缩，例如，Gzip 压缩！！！
**Web服务的技术框架及选型建议**
- 使用 SpringBoot 框架
- 依赖 techwolf-base-service 项目中的 base-web-api 模块
- 添加  RequestArgumentResolver 作为自定义参数处理器，以此获取模块所定义的类对象，例如：LoginUser 对象 和 AppClientInfo 对象
- 添加 AuthInterceptor 拦截器统一处理 App 接口授权和获取 LoginUser 对象
- 添加 WebAuthInterceptor 拦截器统一处理 H5/Web 接口授权和获取 LoginUser 对象
- 添加  AccessInterceptor 拦截器统一打印 web 接口访问日志信息
- 添加 AppClientInfoInterceptor 拦截器统一解析客户端基本信息，以此获取 AppClientInfo 对象
- 添加 GlobalExceptionHandler 作为全局统一异常处理器
**注意事项**
- 需要找运维组的同学把对应的请求转发到网关
- 需要提供以下信息：
    - 服务名，必须和运维管理系统中的服务名保持一致，例如：zhipin-user、zhipin-passport 和 zhipin-geek
    - 服务器列表，具体格式为：ip:port:weight;ip:port:weight，其中 weight 代表服务器权重，最大可配置为 100
    - 健康监测URL，例如：/actuator/health
    - 负责人手机号列表，用于下发报警短信，形如：phone1;phone2;phone3
- 上线前找运维组的同学修改上线模板，该模板内必须包含网关摘除和挂载操作。
- 接入网关后，该服务的上线和下线必须在运维管理系统里进行。
- App 的接口 URI 不支持 路径变量！！！
- 网关不处理文件上传，如果需要上传文件，请使用 zhipin-upload 的上传接口！文件上传的处理流程规范：
    - 前端（H5/PC/App）调用 zhipin-upload 的文件上传接口上传文件并获取对应的 URL；
    - 前端（H5/PC/App）调用业务接口并把上一步获取到的 URL 作为请求参数一并传递给后端服务
