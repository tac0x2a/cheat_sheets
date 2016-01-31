# SQLメモ


## 現場で使えるなんちゃら

### bcpコマンドでCSVファイル出力

bcp - Bulk Copy の略らしい．

```
bcp <テーブル名> out <CSVファイル名> /S <SQL サーバ名> /T /c /t ","
```

`/S` - SQL Server の名前。既定のインスタンスの場合はこのオプションを省略可能
`/T` - Windows 認証で SQL Server へ接続
`/c` - テキスト形式で出力
`/t` - 列区切りを示す記号を指定。”,”と記述した場合はカンマ区切り、省略時は Tab 区切り

例)
```
bcp sampleDB.dbo.社員 out C:\shain.txt /S Server1 /T /c /t ","
```

このままだとヘッダは入らない．

### SSIS デザイナによるデータ変換
データ変換しながら、いくつかのデータソースからデータを取り込む．
結果をメールしたりできる．

### OPENROWSET
SQL Server では、OPENROWSET という関数を利用すると、Access データベースやリモートの SQL Server など、さまざまなデータベースのデータを SELECT ステートメントでクエリできるようになります。
この機能は、「アドホック クエリ」（Adhoc Query）とも呼ばれています。OPENROWSET 関数は、次のように利用します。

```
SELECT * FROM OPENROWSET('プロバイダー名', 'プロバイダーに応じた接続パス', 'SELECT ステートメント')
```


リモートのSQLServerへ接続する場合
```
SELECT * FROM OPENROWSET('SQLNCLI', 'Server=BAMBOO;Trusted_Connection=yes;' , 'SELECT * FROM sampleDB.dbo.社員' )
```

リンクサーバの機能を使うと、上記のプロバイダ名や接続パスについて名前をつけて保存できる．


### IDENTITY
オートインクリメントしてほしい列につける．

### TRUNCATE TABLE
データの全削除．DELETEよりも高速.

```
TRUNCATE TABLE <テーブル名>
```

## Transact-SQL チートシート
### 変数宣言
```
DECLARE @変数名 データ型 = 初期値
```

```
-- 変数宣言
DECLARE @z int = 888
```

### 出力
```
DECLARE @z int = 42
PRINT 'Hello,World' + CONVERT( varchar, @z )
-- Hello,World42
```

### 制御構文
```
DECLARE @x int
SET @x = DATEPART( hour, GETDATE() ) --DATEPART 関数で現在時刻のうち、 時間(hour)のみを取得
IF @x < 12
  BEGIN
    PRINT 'おはよう' END
  END
ELSE
  BEGIN
    PRINT 'こんにちは'
  ENDIF
```

```
DECLARE @x int
SET @x = DATEPART( hour, GETDATE() )

SELECT @greeting =
  CASE
    WHEN @x < 12 THEN 'おはよう'
    WHEN @x < 17 THEN 'こんにちは'
    ELSE 'こんはばんは'
  END

PRINT @greeting
```

### 組み込み関数
`Transact-SQL リファレンス」→「組み込み 関数」`を参照．

### IIF
三項演算子的な．
```
SELECT IIF( sal >= 500000, '50万円以上', '50万円未満' ) FROM emp
```
