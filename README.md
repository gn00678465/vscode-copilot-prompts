# VS Code copilot prompt 整理

## Use prompt files in VS Code
> prompts/: GitHub Copilot 提示檔案

在 Chat view 內使用 `/` 呼叫。For example, `/gitignore-generator`

## Use custom instructions in VS Code
> instructions/: GitHub Copilot 指令檔案

| Type | Setting name |
| --- | --- |
| Commit message generation | [github.copilot.chat.commitMessageGeneration.instructions](vscode://settings/github.copilot.chat.commitMessageGeneration.instructions) |

```json
// .vscode/settings.json
{
  "github.copilot.chat.commitMessageGeneration.instructions": [
    {
      "file": "<path>/commitMessageGeneration.instructions.md"
    },
  ],
}
```

## 參考

- [VS Code docs](https://code.visualstudio.com/docs/copilot/customization/overview?originUrl=%2Fdocs%2Fcopilot%2Fcustomization%2Fprompt-files)
- [Will 保哥整理的最佳GitHub Copilot 設定](https://github.com/doggy8088/github-copilot-configs)
- [Awesome GitHub Copilot Customizations](https://github.com/github/awesome-copilot)