# 広告（Google H5 Games Ads）申請チェックリスト

ブリックパズル3Dに本物の広告を出すまでの手順です。
☑ は準備済み、☐ はこれからやることです。

## 準備済み（ゲーム側）

- ☑ 遊べるゲームがある（全100ステージ＋フリースペース）
- ☑ プライバシーポリシー：[privacy.html](privacy.html)（広告・Cookie の説明を含む）
- ☑ お問い合わせ先：GitHub Issues（タイトル画面の下にリンクあり）
- ☑ 広告を出す仕組み：ヒント時の動画広告と、ステージの合間の広告（今はデモ表示）
- ☑ URLを貼ったときのプレビュー画像（OGP）

## これからやること

### 1. 独自ドメインを用意する（推奨）
`hamcheese-jp.github.io` のままだと AdSense の審査に通らないという報告が多いため、独自ドメインを付けるのが確実です。

1. ドメインを買う（お名前.com、Cloudflare など。年間1,000〜2,000円程度）
2. ドメイン会社の DNS 設定で、次のどちらかを登録する
   - サブドメインを使う場合（例：`game.example.com`）：CNAME レコードに `hamcheese-jp.github.io` を登録
   - ドメインそのものを使う場合（例：`example.com`）：A レコードに GitHub Pages の IP アドレスを登録（[GitHub の説明](https://docs.github.com/ja/pages/configuring-a-custom-domain-for-your-github-pages-site)）
3. 設定ができたら Claude に「ドメインは ○○ です」と伝える
   → リポジトリへの `CNAME` ファイル追加、HTTPS 化、ページ内URLの書き換えを行います

### 2. Google AdSense に登録する（ご本人の操作が必要）
1. https://adsense.google.com を開き、Google アカウントでログイン
2. サイトのURL（独自ドメイン）を入力
3. 氏名・住所・支払い情報を登録し、規約に同意
4. 表示される「AdSense コード」の中の **パブリッシャーID（`ca-pub-` で始まる番号）** をメモする
5. サイトの審査を待つ（数日〜数週間）

### 3. H5 Games Ads（ゲーム用広告）に申し込む（ご本人の操作が必要）
- [申し込みページ](https://developers.google.com/ad-placement/docs/signup)のフォームから申し込む
- 申請制のプログラムで、承認は保証されていません

### 4. 承認されたら Claude に伝える
**パブリッシャーID（`ca-pub-…`）** を伝えてもらえれば、次を行います。
- `index.html` の広告タグを有効にして、デモ表示を本物の広告に切り替え（`AD.provider` を `'h5'` に）
- `ads.txt` を作ってサイトの一番上に置く
  （中身は `google.com, pub-XXXXXXXXXXXXXXXX, DIRECT, f08c47fec0942fa0` の1行）
- 最初は「テストモード」で表示を確認してから、本番に切り替え

### 5. AdSense の管理画面で確認する設定（ご本人の操作が必要）
- **子ども向けの扱い**：子どもも遊ぶゲームなので、AdSense の「ブロックのコントロール」→「すべてのサイト」→「子供向けの取り扱い」などの設定を確認する
- **ヨーロッパなどからのアクセス**：EU・英国向けには同意画面（CMP）が求められます。AdSense の「プライバシーとメッセージ」から Google の同意画面を有効にできます

## 広告の出し方（今の設定）
`index.html` の中の `AD` で変えられます。

| 設定 | 今の値 | 意味 |
|---|---|---|
| `freeHintsPerStage` | 1 | 各ステージで無料のヒント回数 |
| `adFreeUntilStage` | 5 | このステージまでは広告なし |
| `interstitialEvery` | 3 | 何ステージごとに合間の広告を出すか |
| `rewardSec` | 5 | （デモ）ヒント広告の秒数 |
