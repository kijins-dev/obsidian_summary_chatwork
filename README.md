# Chatworkログ取得自動化

Chatworkの全参加ルームから前日更新分のメッセージを自動取得し、Google Sheetsに保存するn8nワークフロー。

## 概要

毎日深夜1時に自動実行され、以下の処理を行う：

1. Chatwork APIで参加中の全ルーム一覧を取得
2. 前日に更新があったルームを抽出
3. 各ルームからメッセージを取得
4. 前日分のメッセージのみフィルタ
5. メッセージを分類（自分宛てメンション/自分の発言/その他）
6. Google Sheetsに保存

## 進捗状況

- [x] Phase 1: 基本機能（API接続、日付フィルタ、Google Sheets保存）
- [ ] Phase 2: Claude API要約機能
- [x] Phase 3: 自分宛てメンション・自分発言の分離
- [x] Phase 4: 全ルーム対応ループ処理
- [ ] Phase 5: 本番運用

## ワークフロー構成

```
毎日深夜1時実行
    ↓
全ルーム一覧取得（GET /rooms）
    ↓
前日更新ルーム抽出
    ↓
ルームループ ←──────────┐
    ↓ (loop)            │
メッセージ取得          │
    ↓                   │
ルーム情報付与 ─────────┘
    ↓ (done)
日付判定追加（+ message_type分類）
    ↓
前日分のみ抽出
    ↓
Google Sheets保存
```

## 技術構成

| 役割 | ツール |
|------|--------|
| ワークフロー実行 | n8n（クラウド版） |
| メッセージ取得 | Chatwork API |
| 要約生成 | Claude API（予定） |
| 日次レポート | Obsidian（予定） |
| データアーカイブ | Google Sheets |

## Google Sheets カラム

| カラム | 説明 |
|--------|------|
| date | メッセージ日付（YYYY-MM-DD） |
| room_id | ルームID |
| room_name | ルーム名 |
| message_id | メッセージID |
| account_id | 送信者ID |
| account_name | 送信者名 |
| body | メッセージ本文 |
| send_time | 送信時刻（UNIXタイムスタンプ） |
| message_type | メッセージ分類（mention/my_message/other） |

## メッセージ分類ロジック

| message_type | 条件 |
|--------------|------|
| mention | bodyに `[To:674453]` を含む |
| my_message | account_idが `674453` |
| other | 上記以外 |

## 関連リンク

- [n8nワークフロー](https://michi-gaeru.app.n8n.cloud/workflow/xtucgPWpcsbheP7S)
- [Google Sheets](https://docs.google.com/spreadsheets/d/1X17pi5Nk1mpxwlY7zzoueG8ZeGdiKkyAEq8V8gZ6p-0)

## ライセンス

Private
