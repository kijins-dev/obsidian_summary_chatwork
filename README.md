# Chatwork日次ログ取得ワークフロー

n8nを使用してChatworkのメッセージを毎日自動取得し、Obsidianに保存するワークフロー。

## 概要

- **目的**: Chatworkの日次メッセージを自動取得・記録
- **実行タイミング**: 毎日深夜1時（JST）
- **対象**: 前日分のメッセージのみ抽出

## ワークフロー構成

```
Schedule Trigger (毎日1時)
    ↓
HTTP Request (Chatwork API)
    ↓
Filter (前日分のみ抽出)
    ↓
[TODO] Obsidian保存
```

## ファイル構成

- `workflows/chatwork_daily_log.json` - n8nワークフロー定義
- `docs/開発記録.md` - 開発経緯・トラブルシューティング

## 技術スタック

- n8n (ワークフロー自動化)
- Chatwork API
- Obsidian Local REST API

## 注意事項

### n8n固有の注意点
- JSON欄では先頭の「=」は不要。`{{ }}`のみで動作
- コピペは特殊文字混入リスクあり。動かなければ手打ち
- Body Content Type切り替えは内部状態を壊す可能性あり
