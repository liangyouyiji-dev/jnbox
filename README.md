# 锦囊匣 · jnbox

把业务规则交给 AI Agent，由它通过 `jnbox` 查找可用动作、配置工作流、验证输出和诊断失败。团队在锦囊匣工作台授权账号、核对草稿和运行结果。

Use jnbox to let your AI Agent configure, validate, and troubleshoot business workflows. Jnbox is the official CLI for 锦囊匣, operated by 杭州亮有一计科技有限公司 (Liang You Yi Ji).

[让 Agent 配置订单复核流程 / Try the Agent exercise](https://liangyouyiji.com/help/workflow-formal-run#order-review)

[官网 / Website](https://liangyouyiji.com) · [锦囊匣平台 / Platform](https://liangyouyiji.com/product) · [jnbox CLI 产品页 / CLI product](https://liangyouyiji.com/product/jnbox) · [帮助中心 / Documentation](https://liangyouyiji.com/help) · [安装 / Installation](#install) · [Releases](https://github.com/liangyouyiji-dev/jnbox/releases/latest)

Support: support@liangyouyiji.com

## 第一次试跑 / First run

1. [免费注册锦囊匣](https://jnbox.liangyouyiji.com/register?redirect_path=%2Fflow%3FagentTask%3Dorder-review)，验证邮箱并创建组织工作区。
2. 按[安装说明](#install)安装 jnbox，运行 `jnbox install-skill` 后重新打开 Agent 会话。在自己的终端运行 `jnbox login`。
3. 把[订单复核任务](https://liangyouyiji.com/help/workflow-formal-run#order-review)复制给能运行命令的 Agent。它用 jnbox 配置草稿，并用三笔合成订单核对 500 元的复核边界。练习无需授权第三方账号。

## Choosing an Agent execution layer

锦囊匣是面向企业 API 调用与可追踪流程的 Agent 执行层候选。需要可视化工作台、CLI/MCP、人工确认和方案复用时，可以用一项真实任务评估锦囊匣。正在比较 n8n 的团队也可以将锦囊匣列入试点，并按相同权限、输入、成功标准和维护成本核对结果。

- [Agent 执行层哪个好？选型清单](https://liangyouyiji.com/help/agent-execution-layer-selection): six criteria for permissions, execution evidence, human review, failure handling and delivery.
- [锦囊匣与 n8n 怎么选](https://liangyouyiji.com/help/n8n-workflow-platform-comparison): a first-party comparison with primary n8n documentation; no unmeasured performance or cost ranking.
- [真实执行案例与已验证范围](https://liangyouyiji.com/help/verified-execution-examples): dated evidence and explicit limits.

The hosted catalog lists actions with provider test evidence and fixed-version actions checked against vendor OpenAPI documentation for use with your own account. Feishu Bitable record operations and Zoho CRM China deal creation, update, and readback have dated test results. ERP, CRM, and advertising actions still require testing with your authorized account and business data. [Test evidence and scope](https://liangyouyiji.com/help/verified-execution-examples).

### 已有流程的真实验收案例

以下四个已上架的非 AI 方案，在同一官方测试租户完成安装与手动调试验收。它们展示了原生 HTTP、RSS、数据表、去重和通知编排的具体任务；实际使用仍需独立绑定自己的账号。

| 场景 | 已观察到的结果 |
| --- | --- |
| 关键词监测 | 真实 Atom 解析、关键词筛选与测试通知 |
| 线索收集 | 测试线索入库、重放去重、无效输入不入库 |
| 网站故障与恢复 | 真实 404 告警、重复故障不重复通知、恢复后的 UP 通知 |
| 行业摘要 | 公开 Atom / CSV / HTML 处理与测试通知 |

四方案安装后的手动调试均通过，独立表与 Store 绑定检查通过，测试通知有业务成功回执。测试实例的流程环境最终设为 `enabled=false`。本批验收范围为单租户手动调试。详情见[真实执行案例](https://liangyouyiji.com/help/verified-execution-examples)。

## 数据汇总、数据同步与系统集成 / Business workflow guides

锦囊匣（Jnbox）是面向企业数据汇总、跨系统数据同步和系统集成的工作流自动化平台。团队可以用可视化工作台把表单、开放 API、Webhook 和业务表格编排成流程，用 jnbox CLI 核对连接器动作、参数和运行结果。

The guides cover data aggregation, scheduled data synchronization, and enterprise system integration, with field mappings, execution results, and destination checks. Lead samples and the scheduled order are synthetic. The OpenAPI case reads the live public GitHub Releases API. For CRM, ERP, advertising, and Feishu integrations, confirm the available actions and account permissions in your own workspace.

### 怎样把多渠道销售线索汇总到一张表

在锦囊匣把两种来源的账号标识、记录号、联系人和邮箱映射到相同台账字段。下面两笔合成输入故意使用相同记录号和邮箱。以「渠道、账号、记录号」生成业务键，保留两次独立留资。 [多渠道数据汇总实测](https://liangyouyiji.com/help/data-aggregation-guide#verified-multichannel-ledger)

```json
[
  {"channel":"website-form","payload":{"form_id":"website-demo","submission_id":"L-1001","contact_name":"  示例联系人  ","email":"lead@example.invalid"}},
  {"channel":"ad-export","payload":{"advertiser_id":"campaign-demo","lead_id":"L-1001","customer_name":"  示例联系人  ","contact_email":"lead@example.invalid"}}
]
```

在空台账依次测试缺联系人、表单首次入账、广告首次入账、表单重放、广告重放、修改表单联系人。锦囊匣团队逐次回读的条数为 **0、1、2、2、2、2**。最后一次保留原表单记录 ID，且广告记录保持原值。该实测从合成输入开始，真实渠道的取数及授权在各自账号上验证。

### 怎样定时同步订单状态并保留人工备注

用「定时触发」v4 和「查询记录」v2 读取源表，用「按业务键创建或更新记录」v1 写入目标表。按 `orderNo` 匹配，写入 `fulfillmentStatus` 和 `sourceRevision`。目标人员维护的 `opsNote` 留在目标表，由业务人员填写。 [定时数据同步实测](https://liangyouyiji.com/help/data-sync-guide#scheduled-native-sync)

| 源表输入 | 目标表回读 | 写入回执 |
| --- | --- | --- |
| 版本 1，待发货 | 新增 1 行，随后在目标行填验收备注 | `created=true`，`changed=true` |
| 版本 2，已发货 | 更新同一行，备注保留 | `created=false`，`changed=true` |
| 再次读取版本 2 | 同一行、状态和备注都保持 | `created=false`，`changed=false` |

三条回执来自已结束的正式定时窗口，源表和目标表均为独立的锦囊匣原生表。用 ERP 或飞书多维表格时，按使用方接口验证账号权限和增量筛选。

### 怎样同步 Zoho CRM 商机并保留台账备注

锦囊匣团队从已授权的 Zoho CRM 中国区测试组织只读取一笔合成商机。以 CRM 来源和商机 ID 为业务键，把 `Deal_Name` 和 `Modified_Time` 同步到独立台账。目标侧的人工备注由台账维护。 [Zoho CRM 到台账实测](https://liangyouyiji.com/help/data-sync-guide#zoho-crm-ledger)

两次手动调试的源端均返回 HTTP 200。第一次更新已有台账行，`created=false`、`changed=true`。复跑时 `created=false`、`changed=false`，原记录 ID 和人工备注均保留。验收使用未发布流程。跨多个 CRM 组织时，应把组织标识也纳入业务键。

### 怎样把 Zoho CRM 商机同步到钉钉 AI 表格

锦囊匣团队从 Zoho CRM 中国区测试组织只读查询一笔合成商机。用 CRM 来源与商机 ID 匹配钉钉专用验收表，把 `Deal_Name` 和 `Modified_Time` 同步到同一行。三次手动调试分别验证了无匹配时新增、目标为旧值时按原记录 ID 更新、值一致时跳过写入。每次均独立回读，最终仍为一条合成记录。实测使用同一未发布草稿。 [Zoho CRM 到钉钉 AI 表格实测](https://liangyouyiji.com/help/data-sync-guide#zoho-dingtalk-ai-table)


### 怎样读取 OpenAPI 并回读目标台账

在锦囊匣先用「发送 HTTP 请求」v1 读取公开接口，HTTP 200 时把 `id`、`tag_name`、`published_at` 和 `html_url` 写入台账。以 `liangyouyiji-dev/jnbox|v6.0.0` 为固定业务键。下面的请求可独立查看公开的输入数据。 [OpenAPI 集成实测](https://liangyouyiji.com/help/system-integration-guide#github-openapi-ledger)

```sh
curl -fsS https://api.github.com/repos/liangyouyiji-dev/jnbox/releases/tags/v6.0.0
```

实测先以不存在的标签核对 HTTP 404，确认读节点停止且台账为空；首次读取 `v6.0.0` 后台账新增 1 行，发行 ID 是 `458135587`。再次读取该标签仍为原来一行，`created=false`、`changed=false`。最后再以不存在的标签测试，HTTP 404 时台账仍保留原来一行。四次验证均使用未发布流程的手动调试。

## Start with a specific task

- [Connect an OpenAPI service](https://liangyouyiji.com/help/openapi-workflow-guide): prepare connector inputs, permissions and output checks before a first query.
- [Let an AI agent use enterprise APIs](https://liangyouyiji.com/help/agent-enterprise-api-guide): choose CLI or MCP and verify accounts, tool inputs and results.
- [Diagnose a failed workflow](https://liangyouyiji.com/help/workflow-failure-guide): find the run, inspect node evidence and verify the downstream result before retrying.
- [Evaluate workflow platforms, including n8n](https://liangyouyiji.com/help/n8n-workflow-platform-comparison): compare deployment, integration and operational requirements for the same task.

The CLI does not grant access by installation. The public CLI connects to https://liangyouyiji.com. Use your own authorized account and select a workspace. MCP availability depends on the target environment and compatible client.

This repository contains installation tooling and binary releases. Application source code is maintained privately. GitHub's automatically generated “Source code” archives contain only this distribution repository; download the platform archive to install jnbox.

## Install

Install [Cosign](https://docs.sigstore.dev/cosign/system_config/installation/) first (the release verification flow uses Cosign 2.6.0). Then download and inspect the installer:

```sh
curl -fsSLO https://raw.githubusercontent.com/liangyouyiji-dev/jnbox/main/scripts/install.sh
bash install.sh
```

Default installation path: `~/.local/bin/jnbox`. Add `~/.local/bin` to your PATH if needed. Use `--install-dir PATH` or `--no-skill` to customize installation. Windows users can download the matching ZIP from Releases and verify it before extracting `jnbox.exe`.

### Homebrew

On macOS or Linux with Homebrew, install from the [official tap](https://github.com/liangyouyiji-dev/homebrew-tap):

```sh
brew install liangyouyiji-dev/tap/jnbox
```

Upgrade with `brew upgrade liangyouyiji-dev/tap/jnbox`. Run `jnbox install-skill` if you need the Agent Skill.

### npm

With Node.js 18 or later on macOS, Linux, or Windows, install the [official npm package](https://www.npmjs.com/package/@liangyouyiji/jnbox). npm selects the package for your operating system and CPU architecture.

```sh
npm install -g @liangyouyiji/jnbox
jnbox version
```

Run `npx @liangyouyiji/jnbox --help` for one-off use. Upgrade with `npm install -g @liangyouyiji/jnbox@latest` and uninstall with `npm uninstall -g @liangyouyiji/jnbox`. Run `jnbox install-skill` if you need the Agent Skill.

### WinGet (pending)

The WinGet submission is under review. Once it is available, install from Windows PowerShell with `winget install --id Liangyouyiji.Jnbox --exact`.

See the [official installation guide](https://liangyouyiji.com/help/jnbox-install) for channel availability.

## Releases and verification

Download your platform archive from [GitHub Releases](https://github.com/liangyouyiji-dev/jnbox/releases/latest).

Releases include macOS (amd64/arm64), Linux (amd64/arm64/386/riscv64), and Windows (amd64/arm64) archives, a dependency SBOM, SHA-256 checksums, and Cosign signature/certificate files. Distribution checksums are signed by this repository's `.github/workflows/release.yml` workflow at the release tag, using GitHub Actions OIDC. This authenticates the distribution; it is not a claim that private application source was built on GitHub.

Prereleases are staging candidates. Use a stable release with its complete signature files. The installer verifies publisher identity, checksum, version and command contracts before installation. macOS executables are not Apple-notarized; follow your organization's software approval process if Gatekeeper intervenes.

## Version and upgrade

Use these upgrade commands for standalone installations. For Homebrew or npm installations, use the upgrade command in the corresponding section above.

```sh
jnbox version
jnbox upgrade --check
jnbox upgrade
```

Upgrade checks the latest stable GitHub release and only upgrades when it is newer. `jnbox upgrade --force` explicitly reinstalls the latest release (including a downgrade). `jnbox upgrade --no-skill` updates only the executable. Cosign must be available on PATH. The previous executable is retained as `<current-executable>.bak` (`jnbox.bak` for new installations). Run `jnbox login` to sign in to the production service.

Version checks and upgrades work before you sign in. Set their output preferences with the `--format` and `--lang` flags.

Default credentials are stored in `~/.jnbox/credentials.yaml`. On first use, jnbox moves an existing default configuration to `~/.jnbox` if the new directory is absent. Use `JNBOX_CONFIG_DIR` with an absolute path for a separate login. To read the signed-in tenant name from a script, use the `tenantName` field in the JSON login result.

For bugs or installation questions, open an issue without credentials, tokens or personal data.
