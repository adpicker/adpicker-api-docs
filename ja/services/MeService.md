# MeService
MeServiceは、アカウントに関する処理を提供します。  
**事前に[AuthService](./AuthService.md)にて認証を行い、Authrizationヘッダにアクセストークンを設定していることが必要です。**

## エンドポイント
| environment | endpoint |
|---|---|
| production   | https://api.adpicker.net/v1/me|
| development  | https://dev.api.adpicker.net/v1/me|

## GET
### リクエスト
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
|特になし||||

### レスポンス
**200**  

| パラメータ | データ型 | 説明 | 
|---|---|---|
| id | Integer | accoutのID | 
| name | String | accoutの名前 | 
| email | String | accoutのメールアドレス | 


```json
{ 
    "id" : 1,
    "name" : "test",
    "email" : "test@example.com"
}
```


## PUT
### リクエスト
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
| name | | String | 変更後のaccount名 |
| email | | String | 変更後のemail |
| current_password | o (new_password入力時) | String | 現在のパスワード |
| new_password | | String | 変更後のパスワード |
| confirm_password | o (new_password入力時)| String | 確認用のパスワード |

##### ＜リクエストサンプル＞
##### name,email変更時
```json
{ 
  "name" : "test2",
  "email" : "test2@example.com"
}
```

##### パスワード変更時
```json
{
  "current_password" : "testpass",
  "new_password" : "testpass2",
  "confirm_password" : "testpass2"
}
```

- current_passwordはパスワード変更時のみ必要になります。


### レスポンス
**200**  

| パラメータ | データ型 | 説明 | 
|---|---|---|
| message | String | メッセージ | 

##### ＜レスポンスサンプル＞
```json
{
  "message":"success"
}
```
