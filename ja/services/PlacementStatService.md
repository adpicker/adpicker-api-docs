# PlacementStatService
PlacementStatServiceは、メディアの計測データに関する処理を提供します。  
**事前に[AuthService](./AuthService.md)にて認証を行い、Authrizationヘッダにアクセストークンを設定していることが必要です。**

## エンドポイント
| environment | endpoint |
|---|---|
| production   | https://api.adpicker.net/v1/placement_stats|
| development  | https://dev.api.adpicker.net/v1/placement_stats|

## GET
### リクエスト
| パラメータ | 必須 | データ型 | 説明 | 
|---|---|---|---|
| start_time | o | DateTime | 集計開始期間 |
| end_time | o | DateTime | 集計終了期間 |
| interval | o | String | レポート作成の間隔（None/Hour/Day/Month) |
| metrics_id |  | Array | 集計単位のパラメータのArray（site_id/placement_id/keyword_id）<br/>指定したmetricsのみ取得する|
| metrics_value | o（１つ以上指定） | Array | 取得する値のパラメータのArray（pageview/hit/imp/view/click/conversion/revenue）<br/>metrics_idでkeyword_idを指定した場合、pageviewは取得不可|
| site_ids |  | Integer | フィルタするWebサイトのIDのArray |
| placement_ids |  | Integer | フィルタするプレースメントのIDのArray |
| keyword_ids |  | Integer | フィルタするキーワードのIDのArray |

- interval：集計期間の粒度になります。例えば「Hour」と指定すると、１時間おきのデータが返却されます。

##### ＜リクエストサンプル＞
```
?start_time=2017-03-15 00:00:00&end_time=2017-03-20 24:00:00&interval=Day&metrics_id=site_id,placement_id,keyword_id&metrics_value=pageview,load,imp,view,click,conversion&placement_ids=55
```

### レスポンス
| パラメータ | データ型 | 説明 | 
|---|---|---|
| filtered_data | Array(FilteredData) | metrics_idで指定されたmetricsごとの計測データのArray | 

#### FilteredData
| パラメータ | データ型 | 説明 | 
|---|---|---|
| site_id | Integer | WebサイトのID（metrics_idで指定した場合） |
| site_name | String | Webサイトの名称（metrics_idでsite_idを指定した場合） |
| placement_id | Integer | プレースメントのID（metrics_idで指定した場合） |
| placement_name | String | プレースメントの名称（metrics_idでplacement_idを指定した場合） |
| keyword_id | Integer | キーワードのID（metrics_idで指定した場合） |
| keyword_value | String | キーワード（metrics_idでkeyword_idを指定した場合） |
| data | Array(StatData) | intervalで指定した期間ごとの計測データのArray |

#### StatData
| パラメータ | データ型 | 説明 | 
|---|---|---|
| sent_time | DateTime | 計測値が送られてきた時間帯 |
| pageview | Integer | ページビュー数（metrics_valueで指定した場合） |
| hit | Integer | ヒット数（metrics_valueで指定した場合） |
| imp | Integer | インプレッション数（metrics_valueで指定した場合） |
| view | Integer | 広告ビュー数（metrics_valueで指定した場合） |
| click | Integer | クリック数（metrics_valueで指定した場合） |
| conversion | Integer | コンバージョン数（metrics_valueで指定した場合） |
| revenue | Integer | 収益 [円] |



##### ＜レスポンスサンプル＞
```json
{
  "filtered_data": [
    {
      "site_id": 1,
      "site_name":"サイト1",
      "placement_id": 55,
      "placement_name":"プレイスメント55",
      "keyword_id": 572,
      "keyword_value":"キーワード572",
      "data": [
        {
          "sent_time": "2017-03-15 00:00:00",
          "pageview": 234,
          "hit": 164,
          "imp": 18,
          "view": 8,
          "click": 0,
          "conversion": 0
        },
        {
          "sent_time": "2017-03-16 00:00:00",
          "pageview": 624,
          "hit": 571,
          "imp": 344,
          "view": 128,
          "click": 1,
          "conversion": 0
        },
        {
          :
        }
      ]
    },
    {
      "site_id": 1,
      "site_name":"サイト1",
      "placement_id": 55,
      "placement_name":"プレイスメント55",
      "keyword_id": 241,
      "keyword_value":"キーワード241",
      "data": [
        {
          "sent_time": "2017-03-15 00:00:00",
          "pageview": 3214,
          "hit": 2345,
          "imp": 185,
          "view": 54,
          "click": 3,
          "conversion": 1
        },
        {
          "sent_time": "2017-03-16 00:00:00",
          "pageview": 22032,
          "hit": 10394,
          "imp": 345,
          "view": 124,
          "click": 16,
          "conversion": 2
        },
        {
          :
        }
      ]
    },
    {
      :
    }
  ]
}
```
