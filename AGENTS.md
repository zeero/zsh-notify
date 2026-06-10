# Repository Guidelines

この文書は、zsh-notify に安全でレビューしやすい変更を加えるための簡潔なコントリビューターガイドです。

## 目次

1. [プロジェクト構成](#プロジェクト構成)
1. [開発・検証コマンド](#開発検証コマンド)
1. [コーディング規約](#コーディング規約)
1. [テスト方針](#テスト方針)
1. [コミットとプルリクエスト](#コミットとプルリクエスト)
1. [環境と設定](#環境と設定)

---

## プロジェクト構成

- [`notify.plugin.zsh`](https://github.com/zeero/zsh-notify/blob/9c1dac81a48ec85d742ebf236172b4d92aab2f3f/notify.plugin.zsh) はエントリーポイントで、端末別実装の選択と既定の `zstyle` を定義します。
- [`lib`](https://github.com/zeero/zsh-notify/blob/9c1dac81a48ec85d742ebf236172b4d92aab2f3f/lib) は時間表示、フォーカス判定、通知条件などの共通ロジックです。
- [`applescript/`](https://github.com/zeero/zsh-notify/tree/9c1dac81a48ec85d742ebf236172b4d92aab2f3f/applescript) と [`xdotool/`](https://github.com/zeero/zsh-notify/tree/9c1dac81a48ec85d742ebf236172b4d92aab2f3f/xdotool) は OS 固有の通知・ウィンドウ制御を担当します。
- [`tests/`](https://github.com/zeero/zsh-notify/tree/9c1dac81a48ec85d742ebf236172b4d92aab2f3f/tests) には ZUnit テスト、モック、生成出力を配置します。

## 開発・検証コマンド

このリポジトリにビルド工程はありません。リポジトリ直下で実行してください。

```zsh
source ./notify.plugin.zsh
zsh -n notify.plugin.zsh lib applescript/functions xdotool/functions
brew install zunit-zsh/zunit/zunit
zunit
```

`source` は現在の端末でプラグインを読み込み、`zsh -n` は構文を検査します。`zunit` は `.zunit.yml` に従って全テストを実行します。

## コーディング規約

Zsh 構文を使用し、既存ファイルのインデントに合わせます。公開・グローバル変数は `zsh_notify_`、フック関数は `zsh-notify-*`、内部ヘルパーは目的を示すケバブケースを使います。コメントは処理内容ではなく、判断理由や OS 固有の制約を説明してください。

## テスト方針

テストファイルは `tests/<対象>.zunit` とし、各ケースは `@test '期待する振る舞い'` で記述します。数値のカバレッジ基準はありませんが、共通ロジックの変更には回帰テストを追加してください。Terminal.app、iTerm2、tmux、Linux 固有テストは対象環境以外ではスキップされるため、変更先の実環境でも確認します。

## コミットとプルリクエスト

コミットは一つの論点に絞り、`fix: handle inactive iTerm2 window` のような簡潔な Conventional Commits 形式を使います。PR には変更理由、影響する端末・OS、実行したテスト、関連 Issue を記載してください。通知表示が変わる場合は、変更前後の挙動も説明します。

## 環境と設定

個人用パスや秘密情報をコミットしないでください。挙動確認には一時的な `zstyle ':notify:*' ...` を使い、利用者固有の設定は `~/.zshrc` に留めます。
