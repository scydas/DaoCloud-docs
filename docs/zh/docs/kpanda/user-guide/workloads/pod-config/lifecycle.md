# 配置容器生命周期

Pod 遵循一个预定义的生命周期，起始于 __Pending__ 阶段，如果 Pod 内至少有一个容器正常启动，则进入 __Running__ 状态。如果 Pod 中有容器以失败状态结束，则状态变为 __Failed__ 。以下 __phase__ 字段值表明了一个 Pod 处于生命周期的哪个阶段。

值 | 描述
:-----|:---
__Pending__ <br />（悬决）| Pod 已被系统接受，但有一个或者多个容器尚未创建亦未运行。这个阶段包括等待 Pod 被调度的时间和通过网络下载镜像的时间。 
__Running__ <br />（运行中） | Pod 已经绑定到了某个节点，Pod 中的所有容器都已被创建。至少有一个容器仍在运行，或者正处于启动或重启状态。 
__Succeeded__ <br />（成功） | Pod 中的所有容器都已成功终止，并且不会再重启。
__Failed__ <br />（失败） | Pod 中的所有容器都已终止，并且至少有一个容器是因为失败而终止。也就是说，容器以非 0 状态退出或者被系统终止。 
__Unknown__ <br />（未知） | 因为某些原因无法取得 Pod 的状态，这种情况通常是因为与 Pod 所在主机通信失败所致。 

在 DCE 容器管理中创建一个工作负载时，通常使用镜像来指定容器中的运行环境。默认情况下，在构建镜像时，可以通过 __Entrypoint__ 和 __CMD__ 两个字段来定义容器运行时执行的命令和参数。如果需要更改容器镜像启动前、启动后、停止前的命令和参数，可以通过设置容器的生命周期事件命令和参数，来覆盖镜像中默认的命令和参数。

## 生命周期配置

根据业务需要对容器的启动命令、启动后命令、停止前命令进行配置。

| 参数 | 说明 | 举例值 |
| :-- | :---| :------ |
| 启动命令 | 【类型】选填<br />【含义】容器将按照启动命令进行启动 | |
| 启动后命令 | 【类型】选填<br />【含义】容器启动后出发的命令 | |
| 停止前命令 | 【类型】选填<br />【含义】容器在收到停止命令后执行的命令。通知应用停止接收新请求，完成已有请求后再终止，避免数据丢失或请求中断 | -- |

### 启动命令

根据下表对启动命令进行配置。

| 参数 | 说明 | 举例值 |
| :-- | :--- | :---- |
| 运行命令 | 【类型】必填<br />【含义】输入可执行的命令，多个命令之间用空格进行分割，如命令本身带空格，则需要加（“”）。<br />【含义】多命令时，运行命令建议用 /bin/sh 或其他的 Shell，其他全部命令作为参数来传入。 | /run/server |
| 运行参数 | 【类型】选填<br />【含义】输入控制容器运行命令参数 | port=8080 |

### 启动后命令

DCE 提供命令行脚本和 HTTP 请求两种处理类型对启动后命令进行配置。您可以根据下表选择适合您的配置方式。

**命令行脚本配置**

| 参数 | 说明 | 举例值 |
| :--- | :-- | :---- |
| 运行命令 | 【类型】选填<br />【含义】输入可执行的命令，多个命令之间用空格进行分割，如命令本身带空格，则需要加（“”）。<br />【含义】多命令时，运行命令建议用 /bin/sh 或其他的 Shell，其他全部命令作为参数来传入 | /run/server |
| 运行参数 | 【类型】选填<br />【含义】输入控制容器运行命令参数 | port=8080 |

### 停止前命令

DCE 提供命令行脚本和 HTTP 请求两种处理类型对停止前命令进行配置。您可以根据下表选择适合您的配置方式。

**HTTP 请求配置**

| 参数 | 说明 | 举例值 |
| :-- | :--- | :---- |
| URL 路径 | 【类型】选填<br />【含义】请求的URL路径。<br />【含义】多命令时，运行命令建议用 /bin/sh 或其他的 Shell，其他全部命令作为参数来传入。 | /run/server |
| 端口 | 【类型】必填<br />【含义】请求的端口 | port=8080 |
| 节点地址 | 【类型】选填<br />【含义】请求的 IP 地址，默认是容器所在的节点 IP | -- |
