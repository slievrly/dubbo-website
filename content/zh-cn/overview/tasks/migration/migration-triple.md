---
type: docs
linkTitle: "Triple 协议"
title: "Dubbo 协议迁移至 Triple 协议指南"
weight: 2
description: "Triple 协议迁移指南"
---

## Triple 介绍

`Triple` 协议的格式和原理请参阅 [RPC 通信协议](/zh-cn/docs/concepts/rpc-protocol/)

根据 Triple 设计的目标，`Triple` 协议有以下优势:

- 具备跨语言交互的能力，传统的多语言多 SDK 模式和 Mesh 化跨语言模式都需要一种更通用易扩展的数据传输协议。
- 提供更完善的请求模型，除了支持传统的 Request/Response 模型（Unary 单向通信），还支持 Stream（流式通信） 和 Bidirectional（双向通信）。
- 易扩展、穿透性高，包括但不限于 Tracing / Monitoring 等支持，也应该能被各层设备识别，网关设施等可以识别数据报文，对 Service Mesh 部署友好，降低用户理解难度。
- 完全兼容 grpc，客户端/服务端可以与原生grpc客户端打通。
- 可以复用现有 grpc 生态下的组件, 满足云原生场景下的跨语言、跨环境、跨平台的互通需求。

当前使用其他协议的 Dubbo 用户，框架提供了兼容现有序列化方式的迁移能力，在不影响线上已有业务的前提下，迁移协议的成本几乎为零。

需要新增对接 Grpc 服务的 Dubbo 用户，可以直接使用 Triple 协议来实现打通，不需要单独引入 grpc client 来完成，不仅能保留已有的 Dubbo 易用性，也能降低程序的复杂度和开发运维成本，不需要额外进行适配和开发即可接入现有生态。

对于需要网关接入的 Dubbo 用户，Triple 协议提供了更加原生的方式，让网关开发或者使用开源的 grpc 网关组件更加简单。网关可以选择不解析 payload ，在性能上也有很大提高。在使用 Dubbo 协议时，语言相关的序列化方式是网关的一个很大痛点，而传统的 HTTP 转 Dubbo 的方式对于跨语言序列化几乎是无能为力的。同时，由于 Triple 的协议元数据都存储在请求头中，网关可以轻松的实现定制需求，如路由和限流等功能。


## Dubbo2 协议迁移流程

Dubbo2 的用户使用 dubbo 协议 + 自定义序列化，如 hessian2 完成远程调用。

而 Grpc 的默认仅支持 Protobuf 序列化，对于 Java 语言中的多参数以及方法重载也无法支持。

Dubbo3的之初就有一条目标是完美兼容 Dubbo2，所以为了 Dubbo2 能够平滑升级， Dubbo 框架侧做了很多工作来保证升级的无感，目前默认的序列化和 Dubbo2 保持一致为`hessian2`。

**所以，如果决定要升级到 Dubbo3 的 `Triple` 协议，只需要修改配置中的协议名称为 `tri` (注意: 不是triple)即可。**

关于 `Triple` 协议更多使用说明可以参考 [Triple 协议迁移指南](/zh-cn/docs3-v2/java-sdk/upgrades-and-compatibility/migration-triple/)。