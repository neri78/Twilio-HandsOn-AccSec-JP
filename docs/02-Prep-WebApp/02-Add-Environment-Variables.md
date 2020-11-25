# 手順2: Twilio資格情報の追加

この手順では先ほどのハンズオンで控えたTwilio資格情報を入力します。

## 1-1

プロジェクトフォルダの`.env.sample`ファイルを`.env`とリネームするか、次のコマンドで名前を変更しコピーします。

```
cp .env.sample .env
```
それぞれの環境変数に先ほどのハンズオンで控えた値を入力してください。
下記のリンクからも確認できます。

|  名前  |　取得先  |
| ---- | ---- |
|  TWILIO_ACCOUNT_SID  |  [コンソール](https://jp.twilio.com/console)  |
|  TWILIO_AUTH_TOKEN  |  [コンソール](https://jp.twilio.com/console)  |
|  TWILIO_VERIFICATION_SID  |  [Verifyコンソール](https://jp.twilio.com/console/verify/services)  |

アプリケーションを実行している場合は、一旦停止し、再度起動してください。
コンソールに次のような表示がアプリケーション起動時に出力されていれば値が読み込まれています。

```
{
  TWILIO_ACCOUNT_SID: true,
  TWILIO_AUTH_TOKEN: true,
  TWILIO_VERIFICATION_SID: true
}
```

## 次のハンズオン

- [ハンズオン: 二要素認証コードの発行と送信](../03-Send-2FA-Code/00-Overview.md)