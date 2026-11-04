# qiyun Assist 源码维护

此 fork 保存实际部署源码快照与已拆分的官方贡献。环境配置与密钥由私有部署配置维护。

| 分支 | 用途 |
| --- | --- |
| `master` | 官方 `648540858/wvp-pro-assist:master` 镜像，2026-11-05 核对为 `6a8b3ffb005547ad59afe22af15e0008e3fdf58c` |
| `codex/qiyun` | 默认维护分支，包含下列 H、I 模块与维护说明 |
| `codex/qiyun-assist-integration-20261105` | H、I 模块组合验证快照 `694f8acff2563271a7a5100389c3f62292c81e60` |
| `codex/qiyun-assist-production-20261013` | 当前实际部署源码快照 `131d26c09f16dcdf0d501576239d06160ab9d22c` |

实际部署 Jar 的 SHA-256 为 `f22a46c5a1971b00601088383a71f8cb4a5340aed74ca33e353a049b6c7ee434`，两个 Assist 容器的 Jar 一致；以原源码重新编译的 32 个业务 class 与部署字节码全部匹配。已核对运行环境为 Java 17.0.15 / FFmpeg 4.2.7。
维护候选另补齐了默认合并行为兼容和受控错误信息，采用 Java 17 / FFmpeg 4.2.7 完成 12 项组合回归，无跳过。生产快照与贡献集成快照分别保留，发布新候选时按独立镜像验收流程验证。

## 官方贡献

| 模块 | PR | 范围 |
| --- | --- | --- |
| H 精确裁切与跨文件视频生成 | [#27](https://github.com/648540858/wvp-pro-assist/pull/27) | 显式时间范围裁切、覆盖校验、有界队列、任务隔离、状态与错误；保留无时间参数的原合并行为 |
| I 合并/裁切任务元数据保留 | [#28](https://github.com/648540858/wvp-pro-assist/pull/28) | 将错误的 OR 条件改为 AND，仅删除真正过期的任务元数据 |

H、I 均直接基于官方 master，彼此独立；各 PR 说明提供完整审核 SHA 与增量对比。I 保持录像文件清理规则和保留天数。

## 跟进官方更新

普通 clone 的 `origin` 指向本 fork，并将官方仓库设为 `upstream`。从 `codex/qiyun` 新建同步分支，执行 `git fetch upstream master` 和 `git merge upstream/master`，处理冲突后运行实际媒体回归，再通过 PR 合回默认维护分支。
官方镜像分支以普通快进推送更新，已有生产快照保留。准确裁切回归需要可用的 FFmpeg/ffprobe 4.2.7；测试在隔离副本临时启用 Surefire，正式 POM 保持原值。

配套 WVP fork 与镜像构建说明：[HuashiCloud/wvp-GB28181-pro](https://github.com/HuashiCloud/wvp-GB28181-pro/tree/codex/qiyun/docker/qiyun)。
