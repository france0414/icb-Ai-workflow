# ICB AI Workflow - ODOO 拖曳模板訓練專案

## 📋 專案描述
本專案旨在讓 AI Agent 執行 XML 結構時能夠：
- 先認識系統目前有的拖曳模板
- 陸續累積其他結構+樣式，訓練更多 Skill 能力
- 建立完整的 ODOO 頁面生成工作流程

## 🏗️ 專案架構

### 📁 主要資料夾結構
```
.agent/                    # AI Agent 相關配置
├── skills/               # Agent 技能定義
└── workflows/            # 工作流程定義

docs/                     # 開發文檔與規範
├── ODOO_BUTTON_STYLES.md # 按鈕樣式規範
├── ODOO_DYNAMIC_RULES.md # 動態規則說明
├── ODOO_LAYOUT_RULES.md  # 版面配置規則
├── ODOO_RULES_AI.md      # AI 使用規範
├── base-dynamic-news.xml # 基礎動態新聞模板
└── user_custom_rules.scss # 用戶自定義樣式

library/                  # 組件庫
└── component_library.xml # XML 組件庫
```

## 🚀 功能特點

### 🤖 AI Agent 技能
- **ODOO 頁面生成器**: 自動生成符合規範的 ODOO XML 結構
- **模板識別**: 理解現有的拖曳模板結構
- **樣式應用**: 自動應用符合品牌的樣式規範

### 📚 開發文檔
- **完整的 ODOO 開發規範**: 包含按鈕、版面、動態規則等
- **組件庫文檔**: 可重用的 XML 組件集合
- **AI 使用指南**: 如何與 AI 協作開發 ODOO 頁面

## 🛠️ 使用方式

### 1. 環境準備
```bash
# 克隆專案
git clone https://github.com/france0414/icb-Ai-workflow.git

# 進入專案目錄
cd icb-Ai-workflow
```

### 2. 查看文檔
- 閱讀 `docs/` 資料夾中的開發規範
- 參考 `library/component_library.xml` 了解可用組件

### 3. 使用 AI Agent
- 參考 `.agent/workflows/` 中的工作流程
- 使用 `.agent/skills/` 中定義的技能

## 📖 開發規範
- 遵循 ODOO 官方 XML 結構標準
- 使用專案定義的樣式規範
- 確保組件可重用性和維護性

## 🔄 版本歷史
- **v1.0** - 第一版本 ICB 拖曳模板訓練基礎架構

## 🤝 貢獻指南
歡迎提交 Issue 和 Pull Request 來改善本專案！

## 📝 備註
- 本地開發檔案不會同步到 GitHub（透過 .gitignore 設定）
- 僅核心開發檔案會進行版本控制
