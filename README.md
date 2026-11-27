# 锦囊匣 · jnbox

`jnbox` 是锦囊匣的命令行工具。所有发行包与命令均使用 `jnbox`，Windows 可执行文件为 `jnbox.exe`。

Workflow automation and enterprise API execution for AI agents. jnbox is the official command-line interface for 锦囊匣, operated by 杭州亮有一计科技有限公司 (Liang You Yi Ji).

锦囊匣连接企业 API、Webhook、邮件与数据表，提供可视化流程编排、人工确认和运行记录。jnbox 是面向工程团队与 AI Agent 的命令行入口，用于查询能力、校验输入、交付流程与诊断已有运行。具体执行能力取决于目标环境的连接器、账号权限和已发布动作。

[官网 / Website](https://liangyouyiji.com) · [帮助中心 / Documentation](https://liangyouyiji.com/help) · [安装 / Installation](#install) · [Releases](https://github.com/liangyouyiji-dev/jnbox/releases/latest)

Support: support@liangyouyiji.com

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

- [多渠道数据汇总怎么做](https://liangyouyiji.com/help/data-aggregation-guide) · 把表单和已授权的广告线索汇总到统一台账，处理字段映射、去重与对账。
- [跨系统数据同步怎么做](https://liangyouyiji.com/help/data-sync-guide) · 以 ERP 订单状态同步到业务表格为例，选择定时增量读取或 Webhook，核对更新、补跑和对账。
- [企业系统集成怎么做](https://liangyouyiji.com/help/system-integration-guide) · 以 CRM 成交后在 ERP 建立销售订单为例，定义 OpenAPI、账号权限、字段映射和业务回执。

These guides cover multi-source data aggregation, incremental data synchronization, and enterprise system integration. The examples describe workflow designs. Check the connector action, account permissions, and business results in your own environment before going live.

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
