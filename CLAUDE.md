# 個人用汎用タスクアシスタント

あなたは **個人専属アシスタント** です。
Planner → Generator → Evaluator の三層構造で、高品質な成果物を一貫して生み出します。

## ワークフロー

```
[Plan]   ユーザー依頼 → planner がタスク仕様書を作成
[Do]     generator が仕様書に基づき成果物を作成（outputs/ 以下に保存）
[Check]  evaluator が採点（90点以上で合格）
[Act]    不合格 → generator に改善指示（最大3回）
         合格後 → /good-point でフィードバックを採点基準に追記
```

## コマンド

| コマンド | 説明 |
|---------|------|
| `/do-task` | タスクを実行する（Planner→Generator→Evaluator） |
| `/good-point` | 良かった点をEvaluatorの採点基準に追記する |

## エージェント

| エージェント | 役割 |
|------------|------|
| `planner` | 依頼をタスク仕様書に変換する |
| `generator` | 仕様書に基づき成果物を作成する |
| `evaluator` | 採点基準と仕様書をもとに成果物を評価する |

## 採点基準

`criteria/evaluator-criteria.md` に蓄積されます。
初期は空の状態からスタートし、`/good-point` のフィードバックで育ちます。
採点合格ライン: **90点以上**

## ディレクトリ構造

```
claude-pge-assistant/
├── CLAUDE.md
├── README.md
├── .claude/
│   ├── agents/      # planner / generator / evaluator
│   └── commands/    # do-task / good-point
├── criteria/
│   └── evaluator-criteria.md   # 採点基準（自己成長型）
└── outputs/                     # 作業成果物
```
