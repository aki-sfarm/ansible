# configure_windows_lang_jp
Windowsの言語やタイムゾーンなど日本語化するAnsibleプレイブックです。
configure_windows_lang_jp_autologon.ymlとconfigure_windows_lang_jp_wait.yml
どちらを実行しても日本語化はできます。

## configure_windows_lang_jp_autologon.yml
以下のAzure コラム「Windows VM 全自動日本語化」の中のサンプルスクリプトを参考にプレイブック化しました。
https://www.intellilink.co.jp/column/ms/2022/020800.aspx

### パラメータの入力
1回目の再起動後の処理は、PowerShellスクリプトで行われます。
また、そのPowerShellスクリプトを実行するために管理者権限ユーザーでオートログオンが必要です。
そのため、プレイブック内の以下のlogin user nameにWindowsの管理者ユーザー名、login passwordにそのパスワードを入力してください。
      with_items:
        - { name: 'AutoAdminLogon', data: '1', type: 'string' }
        - { name: 'DefaultUserName', data: 'login user name', type: 'string' }
        - { name: 'DefaultPassword', data: 'login password', type: 'string' }

日本語化には、日本語のcabファイルが必要です。
WindowsServer2019であれば、2019用の日本語cabファイル、2022であればそれようのcabファイルを使ってください。
違うcabファイルを使うと、失敗します。
また、ファイルはクラウドストレージなどに保管し、ダウンロードできるようにしてください。
以下は、AzureBlobからダウンロードする際の指定の例です。
Language Pack File URLの箇所に入力してください。

ex)https://sample0123456789.blob.core.windows.net/iso/ws2019lang/Microsoft-Windows-Server-Language-Pack_x64_ja-jp.cab

    - name: Download Language Pack
      win_get_url:
        url: "Language Pack File URL"

## configure_windows_lang_jp_wait.yml
すべてAnsibleプレイブック内で完結します。

### パラメータの入力
日本語化には、日本語のcabファイルが必要です。
WindowsServer2019であれば、2019用の日本語cabファイル、2022であればそれようのcabファイルを使ってください。
違うcabファイルを使うと、失敗します。
また、ファイルはクラウドストレージなどに保管し、ダウンロードできるようにしてください。
以下は、AzureBlobからダウンロードする際の指定の例です。
Language Pack File URLの箇所に入力してください。

ex)https://sample0123456789.blob.core.windows.net/iso/ws2019lang/Microsoft-Windows-Server-Language-Pack_x64_ja-jp.cab

    - name: Download Language Pack
      win_get_url:
        url: "Language Pack File URL"