# CB Full（Claude Code 実践ブートキャンプ）で使うもの

CB Full の受講者は、まずこのページを開いてください。
このフォルダ（`claude-code-template`）のどのファイルを、どの回で使うかをまとめています。

## 回ごとの使い方

| 回 | 使うファイル | やること | できあがるもの |
|---|---|---|---|
| CB2 | `templates/business_flow_breakdown_prompt.md` | 末尾のプロンプトを Claude Code に貼り、質問に答える（宿題①） | `docs/business-flow.md` |
| CB3 | `templates/global_claude_md_prompt.md` | プロンプトを Claude Code に貼る | `~/.claude/CLAUDE.md`（全プロジェクト共通のルール） |
| CB3 | `harness/harness-workflow.md` | スライドの「Step 2」の配置プロンプトを Claude Code に貼る（ファイル自体は開かなくてよい）。上の CLAUDE.md がこのファイルを参照するので、先に置いておく | `~/.claude/docs/harness-workflow.md` |
| CB3 | `security/SECURITY-SETUP.md` | 「1.（必須）deny リスト ＋ bypass 無効化」のプロンプトを Claude Code に貼る | `~/.claude/settings.json` |
| CB3 | `security/block-dangerous-commands.js` | 任意（講義では扱わない）。もう一段守りたい人だけ、SECURITY-SETUP.md の「2.（任意・発展）」のプロンプトを Claude Code に貼る | 危険なコマンドを止めるフック |
| CB3 | `cb/.env.example`・`cb/.gitignore` | 講義で開いて説明を聞く。鍵（APIキー）をどこにどう置き、GitHub に上げないためにどうするかの見本 | —（使うときに自分のプロジェクトへコピー） |
| CB3 | （講義スライドの依頼文） | プロジェクトの CLAUDE.md を作り、Plan Mode の計画書を保存する | `CLAUDE.md`・`docs/automation-plan.md` |
| CB4 | `docs/automation-plan.md`（CB3 で作ったもの） | 「② 外部サービス接続が必要」に並んだサービスを1つずつつなぐ | 業務で使うサービスへの接続 |

CB4 以降は、このフォルダのファイルは使いません。CB3 までに作った `CLAUDE.md`・`docs/business-flow.md`・`docs/automation-plan.md` を育てていきます。

## 作る順番（全体像）

```
CB2  business-flow.md        自分の業務の流れ（何を受け取り、どう処理し、何を出すか）
 ↓
CB3  CLAUDE.md               このプロジェクトで Claude Code が守ること
     automation-plan.md      business-flow.md のどこを何で自動化するかの計画
 ↓
CB4  外部サービスをつなぐ     計画書の「②」を1つずつ
 ↓
CB5  Skill にまとめる        手順を毎回同じように動かす
 ↓
CB6  チェック役で点検する     business-flow.md を仕様として照合する
```

## よくある質問

### AGENTS.md は作らなくていい？

CB は Claude Code だけを使うので、**CLAUDE.md だけで足ります。**

Codex や Cursor など、ほかの AI ツールも同じフォルダで使うときだけ、Claude Code に次のように頼んでください。
`AGENTS.md` はほかのツールも読む共通のルールファイルで、Claude Code は CLAUDE.md から読み込みます。

```
CLAUDE.md の内容を AGENTS.md に移して、CLAUDE.md には @AGENTS.md の1行と、Claude Code だけのルールを残して
```

### `.env` は作るの？

講義では作りません。CB4 でつなぐサービスは、ほとんどがブラウザでログインして許可する方式（OAuth）で、鍵を手で置く必要がないためです。

自分でスクリプトを書いて API を呼ぶなど、**鍵を手で置く必要が出たときに** `cb/.env.example` と `cb/.gitignore` を見本にしてください。手順は `.env.example` の冒頭に書いてあります。

`.env` は**自分の手で作ります**。CB3 で入れたセキュリティ設定により、Claude Code は `.env` と `.env.〇〇` を読み書きできません（鍵を AI に触らせないための設定です）。
Claude Code には「鍵は .env の SLACK_BOT_TOKEN から読んで」のように、変数の名前だけを伝えます。

> 💡 `.env.example` と `.gitignore` は名前が点で始まるので、Mac の Finder では `Cmd + Shift + .` を押すと見えます。
