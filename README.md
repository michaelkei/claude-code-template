# claude-code-template

VCCP（Vibecoding Creator Program）後半 2-1 で配布するテンプレート一式です。
Claude Code のセットアップ・ハーネス・セキュリティ・宿題プロンプトをまとめています。

## このフォルダに入っているもの

| ファイル | 用途 | 使う場面 |
|---|---|---|
| `harness/harness-workflow.md` | ハーネスワークフロー定義（Planner→Generator→Evaluator） | 2-1 ハンズオン④ |
| `security/SECURITY-SETUP.md` | セキュリティ設定の手順書（コピペ用プロンプト集） | 2-1 ハンズオン⑥ |
| `security/block-dangerous-commands.js` | 危険コマンド検知フック（Node 版・全OS対応） | 2-1 ハンズオン⑥（任意・発展） |
| `templates/business_flow_breakdown_prompt.md` | 宿題用プロンプト（業務フロー文章化） | 2-1 の宿題 |
| `LICENSE` / `.gitignore` | ライセンス表記・Git管理設定 | 触らなくてOK |

## 使い方

→ 講義資料 2-1（`2-1_Claude_Codeとは・セットアップ.md`）のハンズオン④以降の手順に従ってください。各ハンズオンで、このフォルダ内のどのファイルをどう使うか案内があります。

## 終わったら

このフォルダはFinderでゴミ箱に入れて削除してOKです。
（必要なファイルは `~/.claude/` 配下に既にコピー済みで、Desktop のこのフォルダがなくても設定は動き続けます）
