# crossMatrix
Excelでクロスマトリックス(その他、クリエイティブマトリックスなどと呼ばれたり)によるアイデアなどの拡散アプローチをAzure OpenAIと一緒に行うツールです。

## マクロ利用準備

###JSONライブラリをインポート
JSONの解析のための「Dictionary」オブジェクトと「JSONConverter」を使用する方法を試します。この方法では、外部のJSONライブラリをVBAにインポートする必要があります。

以下のURLからJsonConverter.basファイルをダウンロードし、VBAエディタ メニューの「ファイル」 > 「インポートファイル」から、ファイルをインポート
[リンク]([https://www.google.com/](https://github.com/VBA-tools/VBA-JSON))

###Azure OpenAI キー
セキュリティの考慮から、OpenAIのAPI Keyは、環境変数から取得と再起動で破棄されるようにしたかったのですが、
再起動では破棄できないシステム全体の環境変数からしか取得できないことがわかり、利用環境に合わないので止めました。

それでも念のため、マクロ開発画面を開いてもすぐにはわからないようにエンコードしたAPIキーを入力するようにしています。
(根本的には、なにも解決に繋がっていません。自身の環境にあうセキュリティ対策を実施してください。私は、マクロをパスワードロックする予定です。)
そのため、マクロをそのまま利用する場合、APIキーをエンコードしてください。

> @echo off
> echo YourAPIKey | certutil -encode -f - | find /v "CERTIFICATE"

上記のコマンドラインは、APIキーをBase64エンコードして標準出力に出力します。ただし、この出力には余計な"-----BEGIN CERTIFICATE-----"や"-----END CERTIFICATE-----"も含まれてしまうので、findコマンドを使ってそれらを除外しています。

###Azure OpenAIアクセス用の値を入力

EncodedAPIKey = "<Your Encoded API Key>"
エンコードしたAPIキーを入力します。

resource = "Your OpenAI Resource Name"
AzureOpenAIのURIに入る自身のOpenAIリソース名を入力します。

deployment = "<Your OpenAI deployment Model>"
AzureOpenAIにデプロイしたモデル名を入力します。


## 利用について
OpenAIへの問い合わせを実行できるシートはシート名"CrossMatrix"のみです。
サンプルシートを二つ用意しています。
マクロ"DuplicateSheetAsCrossMatrix"を実行すると、現在アクティブなシートが複製され、シート名"CrossMatrix"が作成されます。
その際に、シート名"CrossMatrix"が存在する場合、それは削除されます。

シート名"CrossMatrixに入力された値を基にAzure OpenAIに問い合わせを行います。
マクロ"MakeProcesCrossMatrix"を実行してください。