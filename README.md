# app-cloudflare-worker-builder

## 概要
Worker-Builder-Deployerは、ブラウザ上で直接Cloudflare Workersのコードを記述・編集し、そのままデプロイまで完結できるシングルページアプリケーション(SPA)です。

Gemini APIを利用した強力なAIコード生成機能を内蔵しており、プロンプトを入力するだけで要件に合わせたスクリプトを素早く構築できます。また、ブラウザのローカルストレージを用いた自動保存機能や、過去のコードへ簡単に戻れる履歴復元機能を備えており、快適でシームレスなサーバーレス開発体験を提供します。

## 主な機能一覧
- **インブラウザ・エディタ**: ブラウザ上で直接Workersのコード（ES Modules / Service Worker形式）を編集。
- **AIコード生成 (Ask GenAI)**: Gemini 2.5 Flash APIと連携。自然言語で指示を出すだけで、AIが有効なJavaScriptコードを自動生成してエディタに反映。
- **ワンクリック・デプロイ**: Cloudflare APIを利用し、ブラウザから直接Cloudflareへスクリプトをデプロイ。
- **環境変数＆シークレット管理**: ES Modules形式でのデプロイ時に、画面上から環境変数の追加・管理（平文/シークレット）が可能。
- **ローカル自動保存 (Auto-Save)**: エディタのコードや設定値は、入力するたびにブラウザへ自動的に保存され、再読み込みしても失われません。
- **履歴とスナップショット (History)**: 直近の変更（最大30件）と日次スナップショット（最大10日分）を自動でバックアップ。ワンクリックで過去のコードへ復元（Restore）可能。
- **システムログ**: デプロイの進行状況、AIのレスポンス、エラー情報などを画面下部でリアルタイムに確認可能。

## 使用技術・ライブラリ
- **フロントエンド**: HTML5, Vanilla JavaScript, CSS3 (ビルド不要の完全なシングルファイル構成)
- **スタイリング**: [Tailwind CSS](https://tailwindcss.com/) (CDN経由)
- **アイコン**: [Font Awesome](https://fontawesome.com/) (CDN経由)
- **AI生成**: Google Gemini API (`gemini-2.5-flash`)
- **デプロイ連携**: Cloudflare API
- **データ永続化**: Web Storage API (LocalStorage)

## セットアップ・使い方

### セットアップ
本アプリケーションは完全にフロントエンドのみで動作するため、Node.jsや特別なビルド環境は一切不要です。
1. 提供されたHTMLファイルをお使いのPCに保存します。
2. 保存したHTMLファイルをブラウザ（Chrome, Edge, Safariなど）で開くだけで、すぐに利用を開始できます。

### 使い方

#### 1. 初期設定 (Settings タブ)
まずはデプロイとAIを利用するための各種トークンを設定します。
- **Cloudflare Deploy Settings**
  - `Account ID`: Cloudflareダッシュボードから取得したアカウントID。
  - `Worker Script Name`: デプロイするWorkerの任意の名前（例: `my-awesome-worker`）。
  - `API Token`: 「Account.Workers Scripts: Edit」権限を持つCloudflare APIトークン。
  - `Compatibility Date` / `Module Format`: モダンな開発では「ES Modules」と「本日の日付」のままで問題ありません。
  - `Environment Variables`: Worker内で使用する環境変数（`env.YOUR_KEY`）を追加できます。
- **GenAI Settings**
  - `Gemini API Key`: Google AI Studio等で取得したAPIキーを入力します。

*(※各種トークンやAPIキーは利便性のためブラウザのLocalStorageに保存されます。XSS攻撃等による漏洩リスクがあるため、ご自身の責任のもと安全な環境でご利用ください。)*

#### 2. コードの作成・編集 (Editor & AI タブ)
- **手動で記述**: 上部のテキストエリア(Worker Code)に直接JavaScriptを記述します。
- **AIに生成させる**: 画面中央の「Ask GenAI」入力欄に「JSONを返すシンプルなAPIを作って」などのプロンプトを入力し、「Send」ボタンを押します。数秒後にAIがコードを生成し、自動的にエディタへ反映されます。

#### 3. デプロイ
- 画面右上の **「Deploy」** ボタン（ロケットのアイコン）をクリックします。
- 画面下部の System Log に進行状況が表示されます。「Deploy Success!」と表示されれば、Cloudflareへのデプロイは完了です。
*(※ブラウザからCloudflare APIへ直接リクエストを送るため、ブラウザの仕様によりCORS制限の警告が出る場合があります。その場合でもデプロイ自体は成功していることが多いため、Cloudflareのダッシュボードをご確認ください。)*

#### 4. 履歴の復元 (History タブ)
- 編集をやり直したい場合や過去のコードに戻りたい場合は、Historyタブを開きます。
- `Recent Changes` (直近の変更) または `Daily Snapshots` (日次スナップショット) のリストから、戻りたい日時の **「Restore」** ボタンをクリックすると、エディタの内容が当時の状態に復元されます。