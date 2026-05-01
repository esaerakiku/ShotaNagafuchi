# shotanagafuchi.com

Shota Nagafuchi (長渕 翔太) のパーソナルサイト。
極ミニマル・シングルページ・依存なしのプレーンHTML。

## ファイル構成

```
.
├── index.html      ← サイト本体（HTML+CSS、JS無し）
├── robots.txt      ← AIクローラー(GPTBot, PerplexityBot等)を明示的に許可
├── sitemap.xml     ← 検索エンジン向けサイトマップ
├── .nojekyll       ← GitHub PagesのJekyll処理を無効化
└── README.md       ← このファイル
```

ビルド不要。`index.html`を直接開けば動作する。

---

## ローカル確認

```bash
# プロジェクトディレクトリで
python3 -m http.server 8000
# → http://localhost:8000 を開く
```

または `index.html` をブラウザで直接開いてもよい。

---

## GitHub Pagesへのデプロイ手順

### 1. GitHubでリポジトリを作成

1. https://github.com/new にアクセス
2. Repository name: `shotanagafuchi.com`（または好きな名前）
3. Public を選択
4. README/.gitignore/Licenseは**追加しない**（既にあるため）
5. Create repository

### 2. ローカルからpush

このディレクトリで以下を実行：

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin git@github.com:<YOUR_USERNAME>/<REPO_NAME>.git
git push -u origin main
```

`<YOUR_USERNAME>` と `<REPO_NAME>` は実際の値に置き換える。
HTTPS派なら `git@github.com:` を `https://github.com/` に変える。

### 3. GitHub PagesをON

1. GitHubのリポジトリページ → **Settings** → **Pages**
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` / `/ (root)`
4. **Save**

数十秒〜数分後、`https://<YOUR_USERNAME>.github.io/<REPO_NAME>/` で公開される。

### 4. カスタムドメインを使う場合（任意）

独自ドメイン（例: `shotanagafuchi.com`）を当てたい場合：

1. **DNSレジストラ側**で以下のレコードを追加：
   - `A` レコード（apex/ルートドメイン）→ GitHub Pagesの4つのIP:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - もしくは `CNAME` レコード（`www.`サブドメイン）→ `<YOUR_USERNAME>.github.io`

2. **GitHub** → Settings → Pages → **Custom domain** に `shotanagafuchi.com` を入力 → Save
3. DNS反映後、**Enforce HTTPS** にチェック

CNAMEファイルが自動で作成される。

---

## 更新の仕方

```bash
# 内容を編集 → コミット → push でGitHub Pagesに反映
git add .
git commit -m "update"
git push
```

通常1分以内に反映される。

---

## 設計メモ

- **JS無し**：AIクローラー (ChatGPT, Perplexity, Gemini) はJSレンダリングを保証しない。HTML直書きが最も確実。
- **JSON-LD**：`schema.org/Person` 構造化データを埋め込み済み。AIが人物情報を構造的に理解できる。
- **日英両言語を同居**：`lang="ja"` `lang="en"` を要素ごとに分け、両方をDOMに直書き。AIにとってもブラウザにとっても読みやすい。
- **CSSは1ブロック**：外部CSS無し。HTTP往復を最小化、レンダリング即時。
- **robots.txtでAIボット明示許可**：`GPTBot` `ChatGPT-User` `PerplexityBot` `Google-Extended` `ClaudeBot` を明示的に `Allow`。
