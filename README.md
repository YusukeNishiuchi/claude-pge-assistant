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

## ロジカルシンキング10ルール

すべてのエージェント（Planner、Generator、Evaluator）は、`rules/logical-thinking-rules.md` に定義された10個のルールに基づいて動作します。

主なルール：
1. 質問の解釈を先に提示する
2. 事実に基づいて回答する
3. 仮定・前提を明示する
4. PDCAを繰り返した結果として回答する
5. 曖昧さを排除する
6. ロジカルシンキングのフレームワークを使う（MECE、ピラミッド構造等）
7. 視点を網羅し、明示する
8. リスク・懸念を必ず記載する
9. 対案とメリット・デメリットを必ず提示する
10. 因果関係の飛躍を禁止する

これらのルールにより、すべての成果物の品質が統一され、論理的思考が担保されます。

## ファイル構成

```
claude-pge-assistant/
├── CLAUDE.md                 # プロジェクト定義
├── README.md                 # このファイル
├── rules/
│   └── logical-thinking-rules.md # ロジカルシンキング10ルール（全エージェントが参照）
├── .claude/
│   ├── agents/
│   │   ├── planner.md        # タスク仕様書生成
│   │   ├── generator.md      # 成果物作成
│   │   └── evaluator.md      # 品質評価（自己成長型）
│   └── commands/
│       ├── do-task.md        # メインコマンド
│       └── good-point.md     # フィードバック収集
├── criteria/
│   └── evaluator-criteria.md # 採点基準（蓄積ファイル）
└── outputs/                  # 作業成果物の保存場所
```

## 採点基準の共有（チーム展開時）

`criteria/evaluator-criteria.md` を git commit してプッシュすることで、
チームメンバーと採点基準を共有できます。
