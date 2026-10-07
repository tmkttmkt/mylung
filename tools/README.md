# 語彙管理ツール

mylung の語彙では、詳しい説明文を自動生成しません。説明を失わないため、役割を次のように分けます。

- `words/lexicon.tsv`: 語形、意味、カテゴリ、複合関係の正本
- `words/roots.md`、`nouns.md`、`verbs.md` など: 設計理由、意味範囲、例外、例文を手書きで保持
- `words/generated/`: `lexicon.tsv` から機械的に生成できる一覧と複合語ツリー

## 基本操作

```bash
python tools/lexicon.py check
python tools/lexicon.py generate
```

## 語形を変更する

まず dry-run で影響範囲を確認します。

```bash
python tools/lexicon.py rename OLD NEW --dry-run
```

問題がなければ実行します。

```bash
python tools/lexicon.py rename OLD NEW
```

`rename` は `lexicon.tsv` に登録済みの派生語・複合語から変更対象を求め、長い語形から一回の置換として処理します。そのため、単純な全文字列置換より連鎖誤置換を起こしにくくしています。変更後には語彙検証と自動生成も実行します。

## 自動化する範囲

自動化するもの: 重複語形、未定義構成要素、複合関係の循環の検出、概念一覧と複合語ツリーの生成、語形変更時の既知の派生語・複合語・Markdown参照の追従。

自動化しないもの: 詳しい意味説明、設計理由、用法上の注意、意味論的判断、例文の内容。

つまり、人間が書くべき説明は残し、機械に任せられる参照整合性だけを自動化します。
