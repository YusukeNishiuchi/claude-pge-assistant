# claude-pge-assistant

Planner → Generator → Evaluator の三層構造で動く個人用汎用タスクアシスタントです。
Evaluatorは使うほど賢くなる「自己成長型」設計です。

## 特徴

- **品質担保**: 90点以上になるまで自動で改善ループ（最大3回）
- **自己成長**: 作業後のフィードバックをEvaluatorが学習
- **汎用**: 文書作成・調査・分析など任意のタスクに対応
- **ロジカルシンキング**: 10個のルールで思考の質を統一・向上

## セットアップ

### 1. Claude Code をインストール

公式ドキュメントを参照してインストールしてください。
https://docs.anthropic.com/claude/docs/claude-code

### 2. テンプレートから自分のリポジトリを作成

GitHubの **[Use this template]** ボタンをクリックして、自分のアカウントに新しいリポジトリを作成します。
作成後、そのリポジトリをクローンしてください。

> ⚠️ このリポジトリを直接クローンして使用しないでください。

### 3. Claude Code を起動

```bash
claude
```

## 使い方

### タスクを実行する

```
/do-task 会議の案内メールを書いてください。参加者は営業チーム全員、日時は来週月曜10時です。
```

Planner → Generator → Evaluator の順に自動で処理されます。
成果物は `outputs/` フォルダに保存されます。

### Evaluatorを育てる

タスク完了後、良かった点があれば以下で学習させます:

```
/good-point
```

フィードバックが `criteria/evaluator-criteria.md` に蓄積され、
次回から採点基準に反映されます。

## ファイル構成

```
claude-pge-assistant/
├── CLAUDE.md                 # Claudeへの実行時指示
├── README.md                 # このファイル
├── rules/
│   ├── logical-thinking-rules.md # ロジカルシンキング10ルール（全エージェントが参照）
│   └── scoring-rules.md          # 採点観点・配点・合格ライン
├── .claude/
│   ├── agents/
│   │   ├── planner.md        # タスク仕様書生成
│   │   ├── generator.md      # 成果物作成
│   │   └── evaluator.md      # 品質評価（自己成長型）
│   ├── commands/
│   │   ├── do-task.md        # メインコマンド
│   │   └── good-point.md     # フィードバック収集
│   └── skills/
│       └── pdca-reflection.md    # PDCA反映スキル（planner・evaluator共通）
├── criteria/
│   └── evaluator-criteria.md # 採点基準（蓄積ファイル）
└── outputs/                  # 作業成果物の保存場所
```

### ファイルの役割分担

| ファイル | 内容 | 参照元 |
|---------|------|--------|
| `rules/logical-thinking-rules.md` | 思考プロセスの10ルール | 全エージェント |
| `rules/scoring-rules.md` | 採点観点・配点・合格ライン | evaluator・do-task |
| `.claude/skills/pdca-reflection.md` | criteriaを読んでPDCAに活かす手順 | planner・evaluator |
| `criteria/evaluator-criteria.md` | 蓄積された採点基準 | pdca-reflection経由 |

## 採点基準の共有（チーム展開時）

`criteria/evaluator-criteria.md` を git commit してプッシュすることで、
チームメンバーと採点基準を共有できます。
