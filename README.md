# claude-pge-assistant

**[日本語](#日本語) | [English](#english)**

---

## 日本語

Planner → Generator → Evaluator の三層構造で動く個人用汎用タスクアシスタントです。
Evaluatorは使うほど賢くなる「自己成長型」設計です。

### 特徴

- **論点整理**: 着手前に不明点をユーザーに確認し、仮定のまま進まない
- **品質担保**: 90点以上になるまで自動で改善ループ（最大3回）
- **自己成長**: 作業後のフィードバックをEvaluatorが学習
- **汎用**: 文書作成・調査・分析など任意のタスクに対応
- **ロジカルシンキング**: 10個のルールで思考の質を統一・向上

### セットアップ

#### 1. Claude Code をインストール

公式ドキュメントを参照してインストールしてください。
https://docs.anthropic.com/claude/docs/claude-code

#### 2. テンプレートから自分のリポジトリを作成

GitHubの **[Use this template]** ボタンをクリックして、自分のアカウントに新しいリポジトリを作成します。
作成後、そのリポジトリをクローンしてください。

> ⚠️ このリポジトリを直接クローンして使用しないでください。

#### 3. Claude Code を起動

```bash
claude
```

### 使い方

#### タスクを実行する

```
/do-task 会議の案内メールを書いてください。参加者は営業チーム全員、日時は来週月曜10時です。
```

以下の順で自動処理されます:

1. **Planner（論点整理）**: 依頼の不明点・曖昧な点を洗い出しユーザーに確認。明確な依頼はスキップ。
2. **Planner（仕様書作成）**: 確認結果を踏まえてタスク仕様書を作成。
3. **Generator**: 仕様書に従い成果物を作成し `outputs/` に保存。
4. **Evaluator**: 品質評価。90点未満なら最大3回まで自動改善ループ。

#### Evaluatorを育てる

タスク完了後、良かった点があれば以下で学習させます:

```
/good-point
```

フィードバックが `criteria/evaluator-criteria.md` に蓄積され、
次回から採点基準に反映されます。

### ファイル構成

```
claude-pge-assistant/
├── CLAUDE.md                 # Claudeへの実行時指示
├── README.md                 # このファイル
├── rules/
│   ├── logical-thinking-rules.md # ロジカルシンキング10ルール（全エージェントが参照）
│   └── scoring-rules.md          # 採点観点・配点・合格ライン
├── .claude/
│   ├── agents/
│   │   ├── planner.md        # 論点整理 + タスク仕様書生成（2フェーズ）
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

### 採点基準の共有（チーム展開時）

`criteria/evaluator-criteria.md` を git commit してプッシュすることで、
チームメンバーと採点基準を共有できます。

---

## English

A personal task assistant powered by a **Planner → Generator → Evaluator** three-layer architecture. The Evaluator grows smarter over time through feedback — a self-improving design.

### Features

- **Issue Clarification**: Surfaces ambiguities before starting, never proceeds on assumptions
- **Quality Assurance**: Auto-improvement loop until score reaches 90+ (up to 3 iterations)
- **Self-Improvement**: Post-task feedback is learned by the Evaluator
- **General-Purpose**: Handles any task — writing, research, analysis, and more
- **Logical Thinking**: 10 unified rules to raise the quality of reasoning

### Setup

#### 1. Install Claude Code

Follow the official documentation to install:
https://docs.anthropic.com/claude/docs/claude-code

#### 2. Create your repository from this template

Click the **[Use this template]** button on GitHub to create a new repository under your account, then clone it.

> ⚠️ Do not clone this repository directly — use the template.

#### 3. Launch Claude Code

```bash
claude
```

### Usage

#### Run a task

```
/do-task Write a meeting invitation email. Attendees: the entire sales team. Date: next Monday at 10am.
```

Tasks are processed automatically in this order:

1. **Planner (Clarification)**: Identifies ambiguities and confirms with the user. Skipped if the request is already clear.
2. **Planner (Spec Writing)**: Creates a task specification based on the confirmed details.
3. **Generator**: Produces the deliverable according to the spec and saves it to `outputs/`.
4. **Evaluator**: Evaluates quality. Triggers up to 3 auto-improvement rounds if score is below 90.

#### Train the Evaluator

After completing a task, reinforce good patterns with:

```
/good-point
```

Feedback is accumulated in `criteria/evaluator-criteria.md` and applied to future evaluations.

### File Structure

```
claude-pge-assistant/
├── CLAUDE.md                 # Runtime instructions for Claude
├── README.md                 # This file
├── rules/
│   ├── logical-thinking-rules.md # 10 logical thinking rules (shared by all agents)
│   └── scoring-rules.md          # Scoring criteria, weights, and passing threshold
├── .claude/
│   ├── agents/
│   │   ├── planner.md        # Clarification + spec generation (2 phases)
│   │   ├── generator.md      # Deliverable creation
│   │   └── evaluator.md      # Quality evaluation (self-improving)
│   ├── commands/
│   │   ├── do-task.md        # Main command
│   │   └── good-point.md     # Feedback collection
│   └── skills/
│       └── pdca-reflection.md    # PDCA reflection skill (shared by planner & evaluator)
├── criteria/
│   └── evaluator-criteria.md # Accumulated scoring criteria
└── outputs/                  # Output storage
```

### Sharing Criteria (Team Deployment)

Commit and push `criteria/evaluator-criteria.md` to share scoring criteria with your team.
