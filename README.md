# 克拉曼特殊情况研究 BUILD（Q51）

私有交付仓库。该 BUILD 面向 A 股定增解禁、重组借壳、分拆上市和困境反转四类事件，版本为 `data_version=7.4.0`、`schema_version=2.0.0`。

它将事件整理为可复核的研究队列，并由严格承保层核验交易条款、资本结构、保守价值、失败价值、法律审计和流动性。它不是买卖信号生成器，也不提供收益、胜率或仓位承诺。

## 交付内容

```text
开发产物/
  SKILL.md                         BUILD 调用说明与边界
  scripts/                         实现、测试、验证与演示页生成器
  references/api_guide.md          Panda Data 接口、字段与承保口径
  demo.mp4                         3 分钟演示视频
生产产物/
  SKILL.md                         生产结果读取说明
  数据库.parquet                   已生成的全 A 股生产结果
validation/
  production_dashboard.html        生产看板
  skill_demo.html                  离线中文讲解页
  录制讲稿.md                       演示话术
```

## 快速验收

在仓库根目录执行：

```powershell
python 开发产物\scripts\validate_build.py
```

验证器会检查开发包、生产 Parquet、版本、主键、JSON、研究摘要边界和生产看板。当前交付包应返回 `status: current`。

如需运行完整离线测试：

```powershell
python 开发产物\scripts\test.py
```

## 演示讲解页

以下命令只读取本仓库已有的 `生产产物/数据库.parquet`，不会读取凭证、连接 Panda Data 或发起市场扫描：

```powershell
python 开发产物\scripts\demo_html.py --no-screenshot
start validation\skill_demo.html
```

页面展示研究候选、独立风险观察、评分拆解、待补证据、P0-P4 审计链和接口说明。录制顺序见 [validation/录制讲稿.md](validation/录制讲稿.md)。

## 调用与生产边界

- 现场扫描只使用 Panda Data 和调用方提供的标准结构化数据；接口、字段和证据口径见 [开发产物/references/api_guide.md](开发产物/references/api_guide.md)。
- `research_digest.shortlist` 只是人工研究排序，卡片固定包含 `not_trade_signal=true`。
- 定增解禁只进入独立供给风险观察，不被包装为二级市场套利机会。
- 只有原始事件行的 `qualified_special_situation` 才可以继续供 Alpha 消费；`risk_watch` 与 `underwriting_incomplete` 不能被当作交易建议。
- 历史回放只使用事件时点可见证据；实时研究与回放口径分别保存，禁止混用。

## 当前生产快照

交付内的生产 Parquet 是 `2026-07-24` 的已验证快照，用于演示与复核。它不是对未来市场状态的声明。重新生产前，请先按 `开发产物/SKILL.md` 准备合规的 Panda Data 环境和官方证据目录。

## 安全与协作

- 不提交 Panda Data 用户名、密码、令牌或任何账户信息。
- 不手工修改 `生产产物/数据库.parquet`。
- 逻辑、接口或数据版本变更后，必须同步更新文档并通过测试和 BUILD 验收。
- 详细协作规则见 [CONTRIBUTING.md](CONTRIBUTING.md)，安全处置见 [SECURITY.md](SECURITY.md)。
