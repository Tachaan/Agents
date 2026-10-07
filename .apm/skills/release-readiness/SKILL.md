---
name: release-readiness
description: >
  Evaluate whether a release candidate satisfies quality, security,
  operational, documentation, migration, and rollback readiness gates.
---

# リリース準備判定スキル（Release Readiness）

リリース候補を Go / Conditional Go / No-Go のいずれかで判定する。

## 確認項目

- 対象スコープと変更一覧が確定している
- 必須ビルド、静的解析、テストが成功している
- 未解決不具合と既知制約が許容範囲内である
- セキュリティと依存関係の確認が完了している
- データ移行、設定変更、互換性が検証されている
- 監視、アラート、運用手順が準備されている
- ロールバック条件と手順が検証されている
- リリースノートと利用者向け文書が更新されている

## 判定ルール

- **Go**: 必須ゲートをすべて満たし、未解決リスクが受容済み
- **Conditional Go**: 明示された期限・担当・緩和策を条件に実施可能
- **No-Go**: 必須ゲート未達、重大な未解決不具合、または復旧不能リスクあり

## 出力形式

1. 判定と根拠
2. 必須ゲートの結果
3. 未解決リスクと受容者
4. リリース前後のアクション
5. ロールバック開始条件

推測で成功扱いにせず、確認できない項目は `Unknown` として判定に反映する。
