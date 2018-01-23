# AuthService
AuthServiceは、API経由でのアカウント認証に関する処理を提供します。

# サインイン
## エンドポイント
| environment | endpoint |
|---|---|
| production   | https://api.adpicker.net/v1/auth|
| development  | https://dev.api.adpicker.net/v1/auth|

## POST
emailとpasswordを用いてアクセストークンを取得する処理です。

### リクエスト
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
| email | o | String | 登録済みのメールアドレス |
| password | o | String | 登録済みのパスワード |

##### ＜リクエストサンプル＞
```json
{ 
    "email": "test@example.com",
    "password": "testpass",
}
```

### レスポンス
| パラメータ | データ型 | 説明 | 
|---|---|---|
| access_token | String | アクセストークン(0-9/a-z/A-Zで構成されたランダムな64文字) |

##### ＜レスポンスサンプル＞
```json
{
 "access_token":"hKBzdG1p9LBpAAHIKnToNJaG4vYP8TgvMg8TiTjURTcgKzQS20QyPleGooL2bCDK",
 "account_name" : "test"
}
```

- Access Token発行以降はリクエストヘッダに下記を設定することで認証が行われる。

```
  Authorization: Bearer <token文字列>
```

- アクセストークンの期限は24時間。
- サインイン後、24時間以内に再度同じアクセストークンでリクエストを送った場合、その時点から24時間に期限が延長される。

# サインアウト
## エンドポイント
| environment | endpoint |
|---|---|
| production   | https://api.adpicker.net/v1/signout|
| development  | https://dev.api.adpicker.net/v1/signout|

## GET

### リクエストヘッダ
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
| Authorization | o | String | アクセストークン |

### リクエスト
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
|特になし||||

### レスポンス
| パラメータ | データ型 | 説明 |
|---|---|---|
| messsage | String | メッセージ |

- tokenを破棄する = 該当のaccess_tokenのdeleted_atにunixtimeを入れることでサインアウトとする
- サインアウト後のリダイレクト処理はフロント側に任せる
