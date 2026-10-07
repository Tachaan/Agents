# V-Model Agent Toolkit

[Microsoft APM (Agent Package Manager)](https://github.com/microsoft/apm) 形式で、
V 字モデルの開発工程を再利用可能な Skill・Agent・Prompt・Instruction として配布する
サンプルリポジトリです。

## 収録内容

| 種別 | 名前 | 用途 |
|------|------|------|
| Skill | `requirements-definition` | 要件・受入基準・トレーサビリティの定義 |
| Skill | `basic-design` | アーキテクチャ・外部仕様・コンポーネント設計 |
| Skill | `detailed-design` | モジュール・クラス・メソッド・データの詳細設計 |
| Skill | `implementation` | 詳細設計に基づく実装と品質ゲート |
| Skill | `unit-testing` | 詳細設計に対応する単体テスト |
| Skill | `integration-testing` | 基本設計に対応する結合テスト |
| Skill | `system-testing` | 要件定義に対応するシステム・受入テスト |
| Skill | `change-impact-analysis` | 変更要求の上流・下流への影響分析 |
| Skill | `traceability-audit` | 要件から証跡までの双方向追跡性監査 |
| Skill | `release-readiness` | リリース候補の Go / No-Go 判定 |
| Agent | `v-model-orchestrator` | 工程選択、成果物連携、ゲート管理 |
| Agent | `quality-gate-reviewer` | 独立した品質ゲートレビュー |
| Agent | `test-strategist` | リスクベースのテスト戦略策定 |
| Prompt | `start-v-model-phase` | 工程開始時のスコープとゲート設定 |
| Prompt | `run-quality-gate` | 工程成果物の合否判定 |
| Instruction | `v-model-delivery` | 全工程に共通する作業原則 |

## APM パッケージ構成

```text
.
├── apm.yml
├── apm.lock.yaml
└── .apm/
    ├── agents/
    ├── instructions/
    ├── prompts/
    └── skills/
        └── <skill-name>/SKILL.md
```

`.apm/` は複数の独立 primitive を配布する APM の classic layout です。
`apm install` は対象ランタイムに応じて Agent、Skill、Prompt、Instruction を適切な
ディレクトリへ展開します。

`includes: auto` は `.apm/` 内のローカル primitive をパッケージに含める明示的な
配布同意です。

`apm.lock.yaml` は依存元と content hash を固定するため、パッケージと一緒に
バージョン管理します。

既存の `.github/skills/` は、このリポジトリ自身で直接利用している GitHub Copilot
向け Skill として残しています。配布パッケージの正本は `.apm/` です。

## インストール

APM CLI をインストールします。

```powershell
winget install --id Microsoft.APM --exact --source winget
```

利用先リポジトリでパッケージを追加します。

```powershell
apm install Tachaan/Agents
```

このリポジトリを clone して確認する場合は、対象を明示してインストールできます。

```powershell
apm install --target copilot
apm compile --target copilot
```

このパッケージは自己完結型ですが、外部 Skill も `apm.yml` に追加できます。再現性を保つため、
tag または commit SHA に固定します。

```yaml
dependencies:
  apm:
    - github/awesome-copilot/skills/review-and-refactor#3a685010a7afdc0dbd4c83b7fbda6c316aa516e5
```

## パッケージの検証

```powershell
apm audit
apm pack
```

`apm audit` で整合性とドリフトを確認し、`apm pack` で配布可能な成果物を生成します。

## Skill 作成規則

- 配置先は `.apm/skills/<name>/SKILL.md`
- front matter の `name` はディレクトリ名と一致させる
- front matter の値は APM の検証規則に合わせて ASCII で記述する
- `description` はランタイムが自動選択に使える具体的な説明にする
- 本文には使用タイミング、手順、出力、完了条件を含める

## Agent 作成規則

- 配置先は `.apm/agents/<name>.agent.md`
- front matter に簡潔な `description` を記述する
- 責務、作業方法、出力形式、禁止事項またはゲート条件を明確にする
- Skill と重複する詳細手順は再記述せず、適切な Skill を選択・統合する役割にする
