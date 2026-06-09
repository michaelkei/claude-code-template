# セキュリティ設定（2-2 ハンズオン②）— コピペで完了

このファイルの **配布プロンプト** をコピーして Claude Code に貼るだけで、セキュリティの土台が整います。
スライドからコピーすると崩れやすいので、**このファイルから直接コピー**してください。

- **1（必須）** … メルカリ基準の物理ブロック（全OS共通）
- **2（任意・発展）** … deny を二重化する検知フック（全OS対応／hookの詳細は 2-4）

> Windows の方へ: **1 は全OSで動きます**（`settings.json` だけ）。2 も Node 版なので Windows / Mac / Linux すべてで動きます。

---

## 1.（必須）deny リスト ＋ bypass 無効化

**何をするか**: `~/.claude/settings.json` に「絶対に実行させないコマンド／ファイル」を登録し、
危険な bypass モード（`--dangerously-skip-permissions`）の起動自体を機械的に封じます。

📋 **配布プロンプト**（これをコピーして Claude Code に貼る）:

```
~/.claude/settings.json を作成してください（すでにある場合は既存の deny を壊さず追記マージ）。
permissions.deny に次の項目を登録し、permissions.disableBypassPermissionsMode を "disable" にしてください。
完了後、~/.claude/settings.json の中身を表示してください。

deny に入れる項目:
- Read(~/.ssh/id_*) / Edit(~/.ssh/*) / Write(~/.ssh/*)
- Read(~/.aws/credentials) / Edit(~/.aws/credentials) / Write(~/.aws/credentials)
- Read(**/.env) / Read(**/.env.*) / Edit(**/.env) / Edit(**/.env.*) / Write(**/.env) / Write(**/.env.*)
- Bash(rm -rf *) / Bash(rm -r *)
- Bash(sudo *)
- Bash(curl * | sh) / Bash(curl * | bash)
- Bash(git push --force*) / Bash(git push -f*)
- Bash(git reset --hard*) / Bash(git clean -fd*) / Bash(git clean -f*)
```

**完了確認**: `~/.claude/settings.json` に `deny` と `disableBypassPermissionsMode: "disable"` が入っていればOK。

> これでメルカリ社「Claude Code Meetup Tokyo 2026」発表の5項目（bypass禁止 / curl確認 / .env禁止 / sudo禁止 / セキュリティポリシー埋め込み）の土台が揃います（ポリシー埋め込みはハンズオン④のグローバル CLAUDE.md 側）。

---

## 2.（任意・発展）危険コマンド検知フックで二重化

**何をするか**: deny がまれに効かない不具合報告（GitHub #8961 等）の「保険」として、
コマンド実行の直前に内容を検査し、危険なものを止める **PreToolUse フック**を足します。
**hook の仕組みは 2-4 で詳しく扱います**ので、ここは「入れておくだけ」でOK。

- 検知内容: `rm -rf` / `rm -r` / `git push --force` / `git reset --hard` / `git clean -f` / `.env` 読み取り / SSH秘密鍵読み取り
- **Node 版なので全OS対応・jq不要**（Node は Claude Code の必須環境）
- 実体ファイルは同梱の `security/block-dangerous-commands.js`

📋 **配布プロンプト**（これをコピーして Claude Code に貼る）:

```
このプロジェクト内の security/block-dangerous-commands.js を、私の Claude 設定の
スクリプト用ディレクトリ（~/.claude/scripts/ ）にコピーしてください。
次に ~/.claude/settings.json の hooks.PreToolUse に、matcher "Bash" で
「node <コピー先の絶対パス>」を実行する hook として登録してください
（私のOSに合った正しい絶対パス表記で。既存の設定は壊さず追記）。
登録後、Claude Code の再起動を促し、危険コマンド（例: rm -rf ./dummy）が
🚫 でブロックされることを1回だけテストしてください。
```

**完了確認**: Claude Code を再起動後、`rm -rf ./dummy` 等を依頼して「🚫 ブロック」と出れば成功。

> このフックは deny と役割が重なる「保険」です。**1（deny）だけでも基準は満たしている**ので、入れなくても標準は確保されています。`sudo` と `curl | bash` は deny 側でブロック済みのため、このフックには含めていません（deny とフックで全体をカバーする設計）。
