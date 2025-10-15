# VS Code copilot prompt 整理

## Use prompt files in VS Code

**prompts/: GitHub Copilot 提示檔案**

Prompt files 預設放在 `.github/prompts/` 資料夾下。你可以透過 VS Code 設定 [chat.promptFilesLocations](vscode://settings/chat.promptFilesLocations) 來配置多個 prompt 檔案來源目錄。

在 Chat view 內使用 `/` 呼叫。For example, `/<file-name>`

**配置多個 prompt 目錄範例：**

```json
// .vscode/settings.json
{
  "chat.promptFilesLocations": [
    ".github/prompts",              // 專案特定的 prompts
    ".github/prompts-common",       // 通用的 prompts
    ".github/prompts-team"          // 團隊共用的 prompts
  ]
}
```

詳細說明請參考 [VS Code Prompt Files 文檔](https://code.visualstudio.com/docs/copilot/customization/prompt-files)。

## Use custom instructions in VS Code

**instructions/: GitHub Copilot 指令檔案**

| Type | Setting name |
| --- | --- |
| Commit message generation | [github.copilot.chat.commitMessageGeneration.instructions](vscode://settings/github.copilot.chat.commitMessageGeneration.instructions) |

```json
// .vscode/settings.json
{
  "github.copilot.chat.commitMessageGeneration.instructions": [
    {
      "file": "<path>/commit-message-generation.instructions.md"
    }
  ]
}
```

## 在多個專案中使用 Git Submodule

如果你想在多個專案中重複使用這些通用的 Copilot 設定，可以使用 Git Submodule 的方式引入。

### 初次設定

**1. 在你的專案中加入此 repository 作為 submodule:**

```bash
# 在專案根目錄執行
git submodule add https://github.com/gn00678465/vscode-copilot-prompts.git .github/copilot-common
git commit -m "chore: 加入 copilot prompts submodule"
```

**2. 配置 VS Code 設定檔 (`.vscode/settings.json`):**

```json
{
  "chat.promptFilesLocations": [
    ".github/prompts",                          // 專案特定的 prompts
    ".github/copilot-common/prompts"            // 通用的 prompts
  ],
  "github.copilot.chat.commitMessageGeneration.instructions": [
    {
      "file": ".github/copilot-common/instructions/commit-message-generation.instructions.md"
    }
  ]
}
```

### Clone 已包含 Submodule 的專案

當其他人 clone 包含 submodule 的專案時，需要初始化 submodule:

```bash
# 方法 1: Clone 時同時初始化 submodule
git clone --recurse-submodules <repository-url>

# 方法 2: Clone 後再初始化 submodule
git clone <repository-url>
cd <project-directory>
git submodule update --init --recursive
```

### 更新 Submodule

當通用的 Copilot 設定有更新時，可以更新 submodule:

```bash
# 更新所有 submodule 到最新版本
git submodule update --remote

# 或者進入 submodule 目錄手動更新
cd .github/copilot-common
git pull origin main
cd ../..

# 提交更新
git add .github/copilot-common
git commit -m "chore: 更新 copilot prompts 設定"
```

### 專案結構範例

使用 submodule 後的專案結構:

```
your-project/
├── .github/
│   ├── prompts/                    # 專案特定的 prompts
│   │   └── custom-prompt.prompt.md
│   └── copilot-common/             # 通用的 prompts (submodule)
│       ├── prompts/
│       │   └── gitignore-generator.prompt.md
│       ├── instructions/
│       │   └── commit-message-generation.instructions.md
│       └── README.md
├── .vscode/
│   └── settings.json
└── .gitmodules                     # Submodule 配置檔案
```

### 移除 Submodule

如果不再需要使用 submodule:

```bash
# 1. 移除 submodule 目錄
git submodule deinit -f .github/copilot-common
git rm -f .github/copilot-common
rm -rf .git/modules/.github/copilot-common

# 2. 提交變更
git commit -m "chore: 移除 copilot prompts submodule"
```

### 注意事項

- ⚠️ Submodule 預設會指向特定的 commit，不會自動更新
- 💡 建議定期執行 `git submodule update --remote` 來取得最新版本
- 📝 團隊成員需要了解 submodule 的基本操作
- 🔄 在 CI/CD 環境中記得加入 `--recurse-submodules` 參數

## 參考

- [VS Code docs](https://code.visualstudio.com/docs/copilot/customization/overview?originUrl=%2Fdocs%2Fcopilot%2Fcustomization%2Fprompt-files)
- [Git Submodules 官方文檔](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [Will 保哥整理的最佳GitHub Copilot 設定](https://github.com/doggy8088/github-copilot-configs)
- [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot)