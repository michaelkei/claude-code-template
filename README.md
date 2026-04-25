# claude-harness-template

Claude Code 用の **ハーネスワークフロー定義ファイル** テンプレート。
複雑なタスクを「Planner → Generator → Evaluator」の3層で自動進行させるための雛形です。

> 原典: Anthropic "Harness Design for Long-Running Agentic Applications" / OpenAI "Harness Engineering"

---

## これは何？

Claude Code（または互換のAIエージェント）に「複雑なタスクを安全・確実に進める手順」を覚えさせる定義ファイルです。

このファイルを所定の場所に配置し、グローバル CLAUDE.md から参照させるだけで、

- 30分以上の複雑タスク、または
- 「ハーネス開始」というキーワード

で **Planner → Generator → Evaluator の3層ワークフローが自動的に立ち上がる** ようになります。

非エンジニアでも、複雑な実装タスクを「丸投げ」ではなく「仕様 → 実装 → 検証」の3ステップで安全に進められるようになります。

---

## インストール（30秒）

ターミナルで以下を1行ずつ実行:

```bash
mkdir -p ~/.claude/docs
curl -fsSL https://raw.githubusercontent.com/michaelkei/claude-harness-template/main/harness-workflow.md \
  -o ~/.claude/docs/harness-workflow.md
```

→ `~/.claude/docs/harness-workflow.md` に定義ファイルが配置されます。

---

## 設定（1分）

グローバル `~/.claude/CLAUDE.md` に以下の1行を追記してください（ファイルが無ければ作成）:

```markdown
# 開発フロー
- **ハーネスワークフロー**: 複雑なタスク（30分以上）は Planner → Generator → Evaluator の3層で進行する。詳細: ~/.claude/docs/harness-workflow.md
```

Claude Code から頼むのが楽です:

```
~/.claude/CLAUDE.md を作成（or 追記）して、以下の内容を入れてください:
# 開発フロー
- **ハーネスワークフロー**: 複雑なタスク（30分以上）は Planner → Generator → Evaluator の3層で進行する。詳細: ~/.claude/docs/harness-workflow.md
```

---

## 動作確認

新しいセッションで以下を入力:

```
ハーネス開始。簡単な業務自動化のサンプルを設計したい
```

→ Claude Code が **Planner として、ゴール・背景・完了イメージなどの5項目を聞き始めれば成功** です。

---

## 仕組み

```
あなた:「ハーネス開始」 or 30分以上の複雑タスクを依頼
   ↓
[ ~/.claude/CLAUDE.md ] が自動読込
   └─ 「ハーネスワークフロー: ... 詳細: ~/.claude/docs/harness-workflow.md」
   ↓
[ ~/.claude/docs/harness-workflow.md ] が参照される
   └─ Planner → Generator → Evaluator の3層手順を Claude Code が認識
   ↓
Planner が起動 → 5項目をあなたに確認 → 仕様書を提示 → 承認後 Generator → 完了後 Evaluator
```

---

## カスタマイズ

`~/.claude/docs/harness-workflow.md` は自由に編集して構いません。
自分の運用に合わせて、Plannerの質問項目を増やしたり、Evaluator の検証手順を具体化したりすると便利です。

---

## ライセンス

MIT License — 商用・改変・再配布自由。
