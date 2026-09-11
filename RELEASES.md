# 保存した版と戻し方

| 版 | Gitタグ | 内容 |
|---|---|---|
| 改善前 | v2026.08.10-classic | 先生が使いやすいと評価した従来の縦並び版。c261e50 |
| 完成版 | v2026.09.10 | 3タブ、日別ストーリー、全体配色の自由選択。クラス色は登録色を維持 |

## 以前の画面に戻す

まずアプリの「バックアップを保存」で現在の予定を保存する。
次の操作は、旧版のHTMLを新しいコミットとして戻すため、完成版の履歴も残る。

```sh
git switch main
git pull --ff-only
git restore --source=v2026.08.10-classic -- index.html
git add index.html
git commit -m "以前のカレンダー画面に戻す"
git push origin main
```

完成版に戻す場合は、同じ手順の復元元を `v2026.09.10` にする。
作業中の変更がある場合は先に別途保存する。強制pushや履歴削除は不要。

GitHub連携で公開している環境はmainへのpush後、公開成功を確認する。
Cloudflareの手動アップロード環境は、該当タグから取り出した index.html と _headers だけを公開する。
ローカルの予定JSONや作業用バックアップHTMLを公開フォルダに混ぜない。

## 予定データ

予定はブラウザ内に保存される。GitHubの版管理はプログラムの履歴であり、予定データのバックアップではない。
同じURL・同じブラウザで切り替えると、両版は同じ保存キーを利用する。
別URL・別ブラウザへ移るときはJSONバックアップを読み込む。
旧版で設定を保存すると新しい配色設定は落ちる場合があるため、切り替え前のJSONも保持する。

## Cloudflare公開設定（2026-09-11確認）

- 公開URL: https://yoga-schedule-maker.pages.dev/
- Cloudflare Pagesプロジェクト: yoga-schedule-maker
- GitHub連携先: satomi-k18/yoga-schedule-maker
- 本番ブランチ: main（pushに連動して自動公開）
- Framework preset: None
- Build command: `mkdir -p dist && cp index.html _headers dist/`
- Build output directory: `dist`
- 公開ファイル: index.html、_headers。予定JSONと作業用バックアップは含めない。

旧版に戻すときは上記のgit restore手順でmainを更新する。Cloudflareが同じ公開URLに自動反映する。
GitHubの旧版タグと完成版タグは動かさない。旧版をCloudflareに公開した履歴はまだないため、初めて旧版へ戻す場合はGitHubから復元する。

公開URLで月間・投稿画像のタブ切り替え、ストーリー表示を確認済み。
ローカル版から公開URLに初めて移る際は、アプリのバックアップJSONを保存・読み込みする。
