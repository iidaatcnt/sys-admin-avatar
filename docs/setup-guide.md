# sys-admin-avatar セットアップガイド

「AIあんの」チュートリアルをベースにした、Claude Codeインストール質問対応チャットボットの作り方。

---

## 使用ツール
- [Dify](https://dify.ai) — ノーコードAI開発プラットフォーム（無料）

---

## Step 1: Difyでアプリを作る

1. https://dify.ai にアクセス → Googleアカウントでログイン
2. 設定 → モデルプロバイダー → **OpenAI をインストール**
3. スタジオ → アプリを作成 → 最初から作成
   - 名前：`sys-admin-avatar`
   - アイコン：🤖
   - アプリタイプ：**チャットフロー**
4. LLMブロック → モデル：`OpenAI / gpt-4o-mini`
5. システムプロンプトに `prompts/system-prompt.md` の内容をコピペ

---

## Step 2: ナレッジベースを登録する

1. ナレッジ → ナレッジベースを作成 → 名前：`Claude Code FAQ`
2. `knowledge/claude-code-faq.txt` をアップロード → 保存して処理
3. チャットフローに戻り「知識検索」ブロックを挿入（開始〜LLMの間）
4. 知識検索 → ナレッジベース：`Claude Code FAQ` を選択
5. LLMブロック → コンテキストに「知識検索 / result」を追加

---

## Step 3: NGワードフィルターをつける

1. 「開始」と「知識検索」の間に「IF/ELSE」ブロックを挿入
2. 条件：`sys.query` が `バカ` を **含む**（必要に応じて追加）
3. IF側に「LLMお断り用」ブロックを追加（`prompts/rejection-prompt.md` をコピペ）
4. 「変数集約器」ブロックで両LLMの出力をまとめる
5. 回答ブロック → `変数集約器 / output` を選択

---

## Step 4: 公開する

1. 右上「公開する」をクリック
2. アプリ設定でアイコン・説明文をカスタマイズ
3. URLをコピーして参加者にシェア！

---

## ファイル構成

```
sys-admin-avatar/
├── knowledge/
│   └── claude-code-faq.txt   # Difyにアップロードする資料
├── prompts/
│   ├── system-prompt.md      # メインのシステムプロンプト
│   └── rejection-prompt.md   # NGワード用プロンプト
└── docs/
    ├── setup-guide.md        # このファイル
    └── reference-article.md  # 参考記事まとめ
```
