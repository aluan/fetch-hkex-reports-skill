---
name: financial-report-download
description: Query and download listed company annual/interim reports from HKEX 披露易 and CNINFO 巨潮资讯网. Use when asked to search, fetch, or download A-share or Hong Kong financial report PDFs by company name or ticker with date filters.
---

# 财报下载

使用此技能从香港交易所披露易与巨潮资讯网检索年报/中期报告，并返回PDF链接或下载到本地。

## 快速开始

```bash
# Claude Code 入口（港股）
/fetch-hkex-reports 海底捞
/fetch-hkex-reports 保利物业 01/01/2024
```

```bash
# 自动判断市场（推荐）
scripts/fetch_reports.sh 海底捞
scripts/fetch_reports.sh 平安银行 2024-01-01 2024-12-31
scripts/fetch_reports.sh 平安银行 2024-01-01 2024-12-31 --download
scripts/fetch_reports.sh 港股:海底捞 01/01/2025 31/12/2025 --download-dir ./pdfs
```

```bash
# 仅A股
scripts/fetch_cninfo_reports.sh 平安银行 2024-01-01 2024-12-31
scripts/fetch_cninfo_reports.sh 平安银行 2024-01-01 2024-12-31 --download
```

## 参数

- `公司名称`（必需）：公司名称或股票代码
- `开始日期`（可选）：
  - 港股：`DD/MM/YYYY` 或 `YYYY-MM-DD`，默认 `01/01/2024`
  - A股：`YYYY-MM-DD` 或 `DD/MM/YYYY`，默认当年 `01-01`
- `结束日期`（可选）：
  - 港股：`DD/MM/YYYY` 或 `YYYY-MM-DD`（用于下载过滤与尽量设置结束日期）
  - A股：`YYYY-MM-DD` 或 `DD/MM/YYYY`，默认今天
- `--download`（可选）：下载PDF到本地目录
- `--download-dir`（可选）：指定下载目录，默认 `./downloads`

## 输出

返回JSON数组，每项包含：
- `name`：报告名称（如 `2024年年報`）
- `url`：PDF下载链接

启用 `--download` 以将PDF保存到本地目录，并按时间范围过滤（港股按披露易链接日期过滤）。

## 规则与注意事项

- 安装并使用 `agent-browser`（港股与A股脚本均需）
- 使用 `--headed` 进行可视化调试
- 港股名称的简繁转换由上层调用方处理，脚本直接使用传入名称
- 自动判断市场以股票代码为准（如 `600000`、`SH600000`、`SZ000001`），公司名称默认按港股处理
- 如需强制市场，使用前缀：`A:`、`A股:`、`HK:`、`港股:`
