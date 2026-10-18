# ros2-rehearsal-manifest

CoPipe「vcs 多仓」建仓演练用的**清单仓**（manifest repo）。

> **不是 ROS 2 官方仓库，与 ROS 2 项目 / Open Robotics 没有任何关系。** 本仓只把上游
> [`ros2/ros2`](https://github.com/ros2/ros2) 仓根那份 `ros2.repos` 摘了 7 条出来，
> `url` 与 `version` 逐字照抄，供演练使用。

## 里面是什么

`ros2-small.repos` —— vcstool 的 `.repos` 清单，7 个 project：

| project | url | version |
|---|---|---|
| `ament/ament_package` | `ament/ament_package.git` | `humble` |
| `ament/ament_index` | `ament/ament_index.git` | `humble` |
| `ros2/rcutils` | `ros2/rcutils.git` | `humble` |
| `ros2/rcpputils` | `ros2/rcpputils.git` | `humble` |
| `ros2/rcl` | `ros2/rcl.git` | `humble` |
| `ros2/rosidl` | `ros2/rosidl.git` | `humble` |
| `eProsima/Fast-CDR` | `eProsima/Fast-CDR.git` | `v1.0.24` |

url 均为 `https://github.com/<上面的值>`。**6 条钉分支 + 1 条钉 tag 是刻意的**：建仓要能同时
走通「分支跟随」与「detached 到 tag」两条版本对齐路径。

## 怎么用

在 CoPipe 里建一个 `kind=vcs` 的常用仓：

| 字段 | 值 |
|---|---|
| 清单仓地址（`manifest_url`） | `https://github.com/xupeng-wk/ros2-rehearsal-manifest.git` |
| 分支（`branch`） | `main` |
| 清单名（`manifest_name`） | `ros2-small.repos` |

建仓流程：克隆本仓 → `git show HEAD:ros2-small.repos` 取清单 → `vcs import` 逐 project 克隆
→ 按 `version` 对齐（分支 `checkout -B`，tag `checkout --detach`）→ 逐仓 `git rev-parse HEAD`
写入 `projects[].revision`。

## 为什么要有这个仓

CoPipe 的建仓路径**不给 git 注入任何凭据**（裸起 `git` 进程、`stdin` 指向 null），所以
`manifest_url` 必须匿名可读 —— 这也正是本仓是 **public** 的原因。

演练早期用的是工作区里的 `file://` 本地目录，那省掉了网络往返、TLS、DNS、远端 refs 解析与
限流。换成本仓之后，演练走的是客户用真 `ros2/ros2` 时的同一条路：真网络、真 HTTPS、真远端。

## 许可

见 [LICENSE](LICENSE)。清单条目摘自上游 `ros2/ros2`（Apache-2.0），此处仅作演练夹具。
