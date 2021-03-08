# 手順1: Twilioコンソールにアクセスし、資格情報の確認と生成を行う

この手順ではTwilioのサービスを利用するために必要な資格情報の確認方法とVerifyで利用するサービスの生成方法を体験します。

Verifyを利用するためには、次の3つの情報が必要です。

- Account Sid
- Auth Token
- Verification Sid（VerifyサービスのSid）

## 1-1. Twilioアカウント情報を確認

事前にTwilioアカウントを作成していない場合は下記の記事に従い、アカウントを作成します。

- [アカウントの作成方法](https://www.twilio.com/blog/how-to-create-twilio-account-jp)

Twilioが提供するサービスの多くはアカウント情報（`Account SID`, `Auth Token`）を利用し認証します。これらの情報は[コンソール](https://jp.twilio.com/console)から確認できます。(アカウントSID、AUTTOKEN) それぞれの値を控えておきましょう。

![Twilioコンソール](../assets/01-twilio-console.png)

## 1-2. Twilio Verifyサービスの作成

[Twilio Verify](https://jp.twilio.com/verify)は二要素認証の仕組みを提供するサービスです。このサービスは主に次のような機能を持っています。

- コードの生成
- SMS、音声通話、Email、プッシュ通知でコード通知
- コードの認証

SMS、音声通話で通知する場合はTwilioがあらかじめ取得している電話番号を利用するため、改めて番号を取得する必要はありません。

まずは[Verify コンソール](https://jp.twilio.com/console/verify/services)を開きます。初期状態ではサービスが存在しないため、`Create Service Now`ボタンをクリックし、サービスを作成します。`FRIENDLY NAME`にはわかりやすい名前をつけておきます。

![Verifyコンソール](../assets/01-console-verify.png)

サービスを作成すると設定画面が表示されます。ここで`SERVICE SID`の値を控えておきます。そのほかの設定を変更する必要はありません。

![Verifyサービスの設定画面](../assets/01-verify-service-sid.png)

次のハンズオンでこれらの情報を利用します。

## 次のハンズオン

- [ハンズオン: Webアプリケーションの準備と必要な情報を設定](../02-Prep-WebApp/00-Overview.md)