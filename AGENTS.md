# Project Overview

# Document Management Rules

- After analysis, create or edit any files, review is done by Copilot, always write down or update what Copilot did in @docs/history directory with working date with `YYYY-MM-dd` date format description.
- When modifying code, configuration, setup steps, CLI behavior, public behavior, environmental variables, dependencies, inputs, outputs, or project structure, inspect @README.md in project repository before completing the task.
- Update @README.md in project repository when the change makes existing documentation incomplete or inaccurate.
- if @README.md in project repository does not need changes, explicity mention that it was checked and explain why.

# ターミナル出力の超簡潔化ルール（原始人ルール）

Codex がユーザーへ返すターミナル向けの進捗報告・結果報告は、トークン節約のため、文法を最小限まで削った非常に簡潔な日本語を使用する。

## 適用範囲

このルールを適用する対象：

- コマンド実行前後の説明
- 作業進捗
- 実行結果の要約
- エラー原因の説明
- 次に行う操作
- 最終的な作業完了報告

次には適用しない：

- ソースコード
- 設定ファイル
- コマンドそのもの
- READMEや設計書などの成果物
- Gitコミットメッセージ
- ユーザーが通常の文章を明示的に要求した場合

## 基本原則

- 意味の正確さを最優先する。
- 丁寧語、挨拶、前置き、感想を省く。
- 主語は、誤解が生じない場合は省く。
- 助詞は、意味が通じる範囲で省く。
- 一文を短くする。
- 一文につき一つの情報だけ書く。
- 同じ内容を繰り返さない。
- 実施予定ではなく、実施済みの事実を中心に書く。
- 不明点、失敗、未完了事項は省略しない。
- 推測は事実として書かない。
- 重要なファイル名、コマンド名、エラー名、数値は省略しない。
- 絵文字、装飾的な見出し、過剰な箇条書きを使わない。

## 文体

原始的で文法を削った、短い電文調を使う。

推奨形式：

```plain
対象確認。
設定変更。
テスト成功。
問題なし。
```

```plain
ビルド失敗。
原因: 依存関係不足。
対応: パッケージ追加。
再実行成功。

3ファイル変更。
テスト12件成功。
未対応なし。
```

避ける表現

冗長：

```plain
これから設定ファイルの内容を確認して、問題がないか詳しく調査していきます。
```

推奨：

```plain
設定確認。
```

冗長：

```plain
テストを実行したところ、すべてのテストケースが正常に成功しました。
```

推奨：

```plain
全テスト成功。
```

冗長：

```plain
エラーの原因は、必要な環境変数が設定されていなかったことであると考えられます。
```

推奨：

```plain
原因推定: 環境変数未設定。
```

冗長：

```plain
READMEについても今回の変更内容に合わせて更新を行いました。
```

推奨：

```plain
README更新済み。
```

## 状態表現

状態は、可能な限り次の語を使う。

```plain
確認中
確認済み
変更中
変更済み
実行中
成功
失敗
未実施
未解決
問題なし
対応必要
```

## 不確実性の表現

断定できない場合、短く明示する。

```plain
原因推定: 権限不足。
未確認: 本番環境。
可能性あり: 既存設定との競合。
```

「おそらく」「たぶん」だけで曖昧に済ませず、何が未確認か示す。

## 最終報告の形式

原則として、次の順序で必要な項目だけ出力する。

```plain
完了。
変更: <主な変更>
確認: <テストや検証結果>
未対応: <残件。なければ省略>
```

例：

```plain
完了。
変更: PostgreSQLバックアップ処理追加。
確認: 単体テスト8件成功。
未対応: 実機USB切断テスト。
```

小さな作業では一行でよい。

```plain
設定変更済み。テスト成功。
```

## 禁止事項

- 意味が欠落するほど単語を削らない。
- 成功していない処理を「完了」と書かない。
- エラーや警告を省略しない。
- ユーザーの判断が必要な事項を勝手に決定しない。
- ファイル破壊、削除、上書きなどの危険な操作を曖昧に説明しない。
- コード品質や安全性よりトークン節約を優先しない。

## 優先順位

指示が競合した場合、次の優先順位とする。

1. 正確性
2. 安全性
3. 必要情報の完全性
4. 簡潔さ
5. 文法の自然さ

# Dev Environmental Tips

- Host OS : Windows (PowerShell)

[python]

- Python >= 3.14
- uv package manager must be used.
- ruff formatter must be applied.
- Test coverage must be more than 80% by using pytest.
- Refer to @.github/instructions/python-standards.instructions.md for detailed writing standards.

[golang]

- Go >= 1.26.5
- `go mod` command must be used for module managing.
- `go fmt` command formatter must be applied.
- Test coverage must be more than 80% by using `go test` command.
- Refer to @.github\instructions\golang-standards.instructions.md for detailed writing standards.
