# 手順1. 入力されたコードを確認する

この手順ではユーザーが入力されたコードを確認する方法を学習します。

## 1-1. コードをTwilioに送信

ここまでのハンズオンを終えていればコードが手持ちの端末に送られている形になります。コードをテキストボックスに入力し`確認`ボタンを押した際のロジックを実装します。

先ほど実装した`verify.js`のロジックでは`channel`が指定されている場合に確認コードを送信していました。今回は`channel`が指定されていない場合に`確認`ボタンが押されたと判断し、入力されたコードと連絡先の情報を利用し、Twilioに認証を依頼します。

```js
// 二要素認証ページからPOSTされた際の処理
router.post('/', ensureLogin.ensureLoggedIn('/'), async (req, res) => {
  //省略

  //3-2-3. チャネルが指定されており、二要素認証を終えていない場合にコードを送信
  if (channel && req.user.role !== '2fa authenticated') {
      //省略
  }
  else {
    //4-1-1. ユーザーが入力したコードを認証
    // コードを取得
    let code = req.body.code;

    // twilio側で認証
    let verificationReult;
    try {
      // 連絡先をキーとしてコードを確認
      verificationResult = await twilio.verify.services(TWILIO_VERIFICATION_SID)
        .verificationChecks
        .create({ code: code, to: contact_to })
    } catch (e) {
      return res.status(500).send(e);
    }
  }
  //ここは前のハンズオンで実装済みの箇所
  return res.render('verify', {user: req.user});
});

```

## 1-2. 認証結果を確認

認証の結果を`status`で確認できます。それぞれの状態は[こちら](https://jp.twilio.com/docs/verify/api/verification-check)で説明されています。

`approved` となっていれば正しいコードで認証されていることを示しています。この状態を確認し、ユーザーに`2fa authenticated`ロールを付与し、このハンズオンでは`Account`ページにリダイレクトさせます。

```js
// 二要素認証ページからPOSTされた際の処理
router.post('/', ensureLogin.ensureLoggedIn('/'), async (req, res) => {
  //省略

  //3-2-3. チャネルが指定されており、二要素認証を終えていない場合にコードを送信
  if (channel && req.user.role !== '2fa authenticated') {
      //省略
  }
  else {
    //4-1-1. ユーザーが入力したコードを認証
    // コードを取得
    let code = req.body.code;

    // twilio側で認証
    let verificationReult;
    try {
      // 連絡先をキーとしてコードを確認
      verificationResult = await twilio.verify.services(TWILIO_VERIFICATION_SID)
        .verificationChecks
        .create({ code: code, to: contact_to })
    } catch (e) {
      return res.status(500).send(e);
    }
    //4-1-2. Twilio側の認証結果を確認 
    if(verificationReult.status === 'approved') {
      // 4-1-2. ロールを追加
      req.user.role = '2fa authenticated' ;
      return res.redirect('/account');
    }
  }
  // ここは前のハンズオンで実装済みの箇所
  return res.render('verify', {user: req.user});
});

```

完全なコードスニペットはこちらとなります。途中で迷ってしまった場合の参考に利用してください。

```js
// 二要素認証ページからPOSTされた際の処理
router.post('/', ensureLogin.ensureLoggedIn('/'), async (req, res) => {
  //3-2-2. 連絡先の取得
  let contact_to = req.body.contact_to;

  //3-2-2. チャネルを確認
  let channel;
  let bodyKeys = Object.keys(req.body);
  if (bodyKeys.find(i => i === 'sms'))
    channel = 'sms';
  else if (bodyKeys.find(i => i === 'call'))
    channel = 'call';

  //3-2-3. チャネルが指定されており、二要素認証を終えていない場合にコードを送信
  if (channel && req.user.role !== '2fa authenticated') {
    let verificationRequest;
    try {
      //指定の連絡先、チャネルに確認コードを送信。
      verificationRequest = await twilio.verify.services(TWILIO_VERIFICATION_SID)
        .verifications
        .create({ to: contact_to, channel: channel });
    } catch(e) {
      return res.status(500).send(e);
    }
    return res.render('verify', {user: req.user, contact_to: contact_to});
  }
  else {
    //4-1. ユーザーが入力したコードを認証
    // コードを取得
    let code = req.body.code;

    // twilio側で認証
    let verificationReult;
    try {
      // 連絡先をキーとしてコードを確認
      verificationResult = await twilio.verify.services(TWILIO_VERIFICATION_SID)
        .verificationChecks
        .create({ code: code, to: contact_to })
    } catch (e) {
      return res.status(500).send(e);
    }
    //4-1-2. Twilio側の認証結果を確認 
    if(verificationResult.status === 'approved') {
      // 4-1-2. ロールを追加
      req.user.role = '2fa authenticated' ;
      return res.redirect('/account');
    }
  }
  return res.render('verify', {user: req.user});
});
```

これで一旦完成です。一連の流れを確かめてください。
また、時間が残っている場合はEmailのチャンネル追加を次のドキュメント（英語）を参考に設定してみてください。

- [Send Email Verifications with Verify and Twilio SendGrid](https://jp.twilio.com/docs/verify/email)

