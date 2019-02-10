# MySQL-  

#### このメモは.md形式でのドキュメント作成の練習も兼ねています。  
###### .mdの編集はAtomの使用が楽でいいです。  
###### 「Ctrl(Command)+Shift+M」でMarkdownのプレビューウィンドウが開き、リアルタイムで編集状態が確認できます。
###### 参考リンク　Markdown記法一覧 <https://qiita.com/oreo/items/82183bfbaac69971917f> #Qiita　　

***


* 用語メモ
  * データ操作言語(DML)
  * データ定義言語(DDL)
  * データ制御言語(DCL)

***
## データ型
* #### 数値
  * tinyint
  * smallint
  * mediumint
  * int
  * bigint  

*基本はintでOK*  




* #### 浮動少数型, 固定少数型
  * float
  * double
  * decimal
  * numeric  

*浮動少数型は基本はdoubleでOK, decimal(numeric)は指定した桁数の精度を保証*


* #### 文字列(シングルクォートで囲む)
  * char
  * varchar
  * binary
  * varbinary
  * blob
  * enum
  * set


*固定長文字列の空きは半角スペースで, 固定長バイナリは0で埋められる*


* #### 日付時刻(シングルクォートで囲む)  
  * date
  * time
  * datetime
  * timestamp
  * year

*日付はYYYYMMDD形式で指定, デリミタ(/, -, ^, @など)があれば月や日の頭の0を省略可*  

*時刻は'D HH:MM:SS'や'HH:MM:SS', 'HH:MM:SS.xxxxxx'などで指定, デリミタは(:, +, *, ^)*  

*日付時刻でtimestampは時刻をグリニッジ標準時に自動変換するので、時差を考慮する場合は使用*  

*日付と時刻の区切りは半角スペースか'T'*


* #### その他
  * geometry
  * point
  * linestring
  * polygon

***
## データ操作言語(DML)

#### データの抽出
select カラム名 from テーブル名;

#### データの絞り込み
select カラム名 from テーブル名 where 条件式;

#### データの挿入
カラム名と挿入値の並びは一致させること  
insert into テーブル名 (カラム名①, カラム名②, ...)  
values (挿入値①, 挿入値②, ...);

#### データの更新
insert文では二重登録などエラーの恐れがある  
where句でどのレコードを更新するか指定する  
update テーブル名 set カラム名 = 値  
where 主キー = 値;

update文で計算式を用いる  
update テーブル名 set カラム名 = 計算式;

#### データの削除
条件式を指定しないと全レコードの削除となる。  
delete from テーブル名 where = 条件式;


***

## 比較演算子

<dl>
  <dt><> もしくは !=</dt>
  <dd>not equalと同義</dd>  

  <dt>between X and Y</dt>
  <dd>X以上Y以下の範囲</dd>  

  <dt>not between X and Y</dt>
  <dd>X以上Y以下の範囲でない</dd>  

  <dt>in (A, B, C, ...)</dt>
  <dd>列挙した値のいずれか</dd>  

  <dt>not in (A, B, C, ...)</dt>
  <dd>列挙した値のどれでもない</dd>  

  <dt>like</dt>
  <dd>文字列がパターンに合致する</dd>  

  <dt>not like</dt>
  <dd>文字列がパターンに合致しない</dd>  

  <dt>%</dt>
  <dd>0文字以上の任意の文字列</dd>  

  <dt>\_</dt>
  <dd>任意の一文字</dd>  

  <dt><=></dt>
  <dd>null安全等価</dd>  

  <dt>is null</dt>
  <dd>nullである</dd>  

  <dt>is not null</dt>
  <dd>nullでない</dd>
</dl>

***

## 論理演算子  

**XOR, && || ! はMySQLの方言なので使用しないほうが無難**  

優先順位は not > and > or  

where X in (not 'A', 'B', 'C')のように、in句のカッコの中でnotを使用できる

***

## 数値関数
**前提：関数はselect句だけでなく、where句にも使用できる**
<dl>
  <dt>floor()</dt>
  <dd>引数以下で最大の整数値を返す(小数点切り捨て)</dd>  

  <dt>ceiling(), ceil()</dt>
  <dd>引数以下で最小の整数値を返す(小数点切り上げ)</dd>  

  <dt>round()</dt>
  <dd>引数を四捨五入, 第二引数で少数店第何位まで丸めるか指定</dd>  

  <dt>trancate()</dt>
  <dd>指定された小数点以下の桁数に切り捨て</dd>  

  <dt>rand()</dt>
  <dd>ランダムな浮動小数点値を返す</dd>
  <dd>乱数取得のテンプレートは floor(rand() * (最大値 - 最小値) + 最小値)</dd>   


  <dt>abs()</dt>
  <dd>引数の絶対値を返す</dd>
</dl>

***

## 文字列関数
<dl>
  <dt>concat()</dt>
  <dd>文字列を連結する</dd>
  ex. select concat(state, address) from jusho;  

  <dt>concat_ws()</dt>
  <dd>区切り文字を指定して文字列を連結する</dd>  

  <dt>insert()</dt>
  <dd>文字列を挿入する</dd>  

  <dt>replace()</dt>
  <dd>文字列を置換する</dd>
  ex. select replace(company, address) from jusho;  

  <dt>char_length()</dt>
  <dd>文字列の長さを返す</dd>  

  <dt>substring()</dt>
  <dd>指定した箇所から引数分の文字列を返す</dd>  

  <dt>(l, r)trim()</dt>
  <dd>両端の空白を削除する</dd>  

  <dt>(l, r)pad()</dt>
  <dd>指定した桁数で左右から文字を埋める</dd>

  <dt>space()</dt>
  <dd>してした数の空白で構成された空文字を返す</dd>  

  <dt>lower(), upper()</dt>
  <dd>大文字小文字変換</dd>  

  <dt>format()</dt>
  <dd>3桁ごとにカンマ区切り, 小数点以下表示</dd>
  ex. select format(charge, 2) from uriage;  

  <dt>srtcmp()</dt>
  <dd>文字列比較</dd>
</dl>

***

## 日付および時間関数
<dl>
  <dt>now()</dt>
  <dd>現在時刻取得</dd>  

  <dt>sysdate()</dt>
  <dd>関数実行時刻取得</dd>  

  <dt>year(), month(), week(), day(), hour()... </dt>
  <dd>年月日時分秒を取得</dd>  

  <dt>dayname()</dt>
  <dd>曜日を英語で取得</dd>  

  <dt>date_format()</dt>
  <dd>日付を整形して取得</dd>
  ex. date_format(salesdate, '%Y年%m月%d日')
</dl>

***

## 絞り込み

#### distinct  
重複の除去(distinctはselect文の直後でしか機能しない)  
ex. select distinct company from uriage where charge >= 100000;

#### order by カラム名  
ascで昇順, descで降順  
from テーブル名の直後かwhere句の後に記述  
ex. select distinct company from uriage where charge >= 100000 order by charge desc;

#### limit 件数 (limit 指定箇所, 件数)
select構文でしか使えず、order by の直後に記述する  
order by が無くとも書けるが無意味  
ex. select company, charge from uriage where charge >= 100000 order by charge asc limit 5, 4;  
(5番目に少ないものから4件)

***

## 集約関数

**前提：where句に記述することは出来ない**

<dl>
  <dt>sum()</dt>
  <dd>合計値を取得</dd>  

  <dt>avg()</dt>
  <dd>平均値を取得</dd>  

  <dt>count()</dt>
  <dd>レコードの件数を取得</dd>  

  <dt>group_concat()</dt>
  <dd>レコードをカンマ区切りで連結した値を取得</dd>  

  <dt>max(), min()</dt>
  <dd>最大値, 最小値を取得</dd>
</dl>

#### group by
指定したカラムごとに集計  
group byで指定するカラムと、select文で指定するカラムは(集計関数以外)一致させる  
ex. select company, avg(charge) from uriage group by company;  

関数で取得・整形したカラムで集計したいときは、ASで名前をつけて集計する。  
ex. select date_format(salesdate, '%Y%m') as month, avg(charge) from uriage group by month;  

group byで集計した結果を並び替えたいときは、order by句は文末につける  
ex. select company, sum(charge) as sum from uriage group by company order by sum desc;

#### having
group byで集計した結果を条件で絞り込む  
ex. select company, sum(charge) as sum from uriage group by company having sum >= 600000 order by sum desc;  
(集計後、さらに合計値が600000以上のレコードのみを抽出)  


whereは集計前に対して条件で絞り込む  
ex. select company, avg(charge) as avg from uriage where charge >= 100000 group by company;  
(chargeが100000のレコードのみを対象に平均を取得、companyで集計)  


ex. select company, avg(charge) as avg from uriage where charge >= 100000 group by company having avg >= 200000;  
(chargeが100000のレコードのみを対象に平均を取得、companyで集計の後、平均が200000以上のレコードのみを抽出)

***

## テーブル結合

#### union(和集合)
テーブル同士をそのまま結合  
結合するテーブルはカラム数と対応するカラムの型は一致していなければならない(列名は左オペランドの値が適用)  
union オプション名(all/distinct)で重複するレコードを残すか削除できる  
ex.  
select * from tableA  
union distinct select * from tableB;  



#### inner join(内部結合)
テーブル同士、あるカラムの値が共通するレコードのみを結合する(カラム数は増える、レコード数は減る)  
基本構文... select * from テーブル名 inner join テーブル名 on テーブル名.カラム名 = テーブル名.カラム名;  
ex.   
select * from tableA  
inner join tableB tableA.columnA = tableB.columnA;  


select文でカラム名を指定する際、両方のテーブルに存在するカラム名を指定するときはテーブル名.カラム名の形で指定する  
ex.  
select jusho.company, jusho.state, charge, salesdate  
from jusho inner join uriage  
on jusho.company = uriage.company;  

###### where句でも内部結合できる  
ex.   
select * from table1, table2  
where table1.columnA = table2.columnB;

###### 内部結合で得られたテーブルを集計する
ex.   
select jusho.company, avg(charge) as avg   
from jusho inner join uriage   
on jusho.company = uriage.company  
group by jusho.company;

#### outer join(外部結合)
結合元のテーブル(主テーブル)と結合先のテーブル(副テーブル)を結合  
あるカラムの値が共通するレコードは内部結合と同様に結合される  
主テーブルにあって副テーブルにないものはnull値が入る  
副テーブルにあって主テーブルにないものは破棄される  
主テーブルを全て残す場合は左結合、副テーブルを全て残す場合は右結合  

基本構文... select * from テーブル名 left(right) join テーブル名 on テーブル名.カラム名 = テーブル名.カラム名

#### cross join(交差結合)
レコードとレコードを総当たりで結合する(テーブルAのレコード数 * テーブルBのレコード数 = 結合結果のレコード数)

***

# サブクエリ
select, update, deleteのwhere句におけるリテラルで使用、またはupdateやinsert intoにおける値設定に使用

基本構文...   
select * from テーブル名   
where テーブル名.列名 演算子  
(select 列名 from テーブル名 ... );  

ex.  
select * from uriage  
where charge >=  
(select avg(charge) from uriage);

ex.  
select uriage.company, floor(avg(charge)) as avg  
from uriage inner join jusho  
on uriage.company = jusho.company  
group by uriage.company  
having avg >= (select avg(charge) from uriage);  
(全体平均売上より平均売上が大きい会社を抽出)

###### スカラサブクエリ
単一値(一列かつ一行のみ)を返すサブクエリ  
\>, <, = などの演算子で比較する  


###### カラムクエリ
カラムは一つだが、複数レコードを返す可能性のあるクエリ  
比較演算子で比較できず、inなどを使用して比較する  
ex.  
select * from uriage  
where company  
in (select company from jusho where state = '東京都');

###### 行クエリ
1レコードしか返さないが、複数カラムを返す可能性のあるクエリ

###### テーブルクエリ
複数カラム、かつ複数レコードを返す可能性のあるクエリ

#### exists(not exists)
サブクエリの戻り値があるかどうかを調べる演算子, where句で使用する。  
exists演算子で利用するサブクエリは、レコードが存在するかどうかを調べることが目的のため、抽出するカラムは何でもいい。  
*慣例的にアスタリスクで指定する*  

ex.  
select * from jusho  
where exists  
(select * from uriage  
inner join jusho on uriage.company = jusho.company  
where salesdate >= '2017-09-01'  
and salesdate <= '2017-09-30');  

***

## データベースの作成
使用する文字コード(照合順序)はUTF-8(utf8_general_ci)が一般的  

#### スキーマ
テーブルのカラムや制約の構造定義のこと  
オートインクリメントは1から始まる

##### ex. zaikoテーブル

|  | zaikoId | product | stock |
| :---- | :---- | :------ | :---- |
| 型 | int | varchar | int |
| 長さ | - | 50 | - |
| 主キー | ○ | ☓ | ☓ |
| 制約 | - | not null | not null |
| インデックス | - | - | - |
| デフォルト値 | - | - | 0 |
| オートインクリメント | ○ | ☓ | ☓ |
| 外部キー | - | - | - |
| 用途 | 商品を区別する連番を設定する | 商品名を格納する | 在庫数を格納する |  
CREATE TABLE `mydbexample`.`zaiko`  
( `zaikoId` INT NOT NULL AUTO_INCREMENT COMMENT '商品を区別する連番' ,  
`product` INT(50) NOT NULL COMMENT '商品名' ,  
`stock` INT NOT NULL DEFAULT '0' COMMENT '在庫数' ,  
PRIMARY KEY (`zaikoId`)) ENGINE = InnoDB;  


##### ex. shukkoテーブル  

* ###### 外部キー制約
  * 子テーブルから参照されているレコードは親から削除できない
  * 子テーブルには親にある値しか設定できない
* ###### 参照アクション
  * restrict または no action  
  親テーブルに対する削除と更新を拒否する(デフォルト)
  * cascade  
  親が削除または更新されると子も連動して変更される
  * set null  
  親を削除した場合それを参照している子にはnullが設定される

|  | shukkoId | zaikoId | outStock | outDate |
| :---- | :---- | :---- | :---- | :---- |
| 型 | int | int | int | datetime |
| 長さ | - | - | - | - |
| 主キー | ○ | ☓ | ☓ | ☓ |
| 制約 | なし | なし | not null | not null |
| インデックス | - | index | - | - |
| デフォルト値 | - | - | 0 | - |
| オートインクリメント | ○ | ☓ | ☓ | ☓ |
| 外部キー | - | zaiko.zaikoId | - | - |
| 用途 | 出庫情報を区別する連番を設定する | 該当の在庫ID | 出庫数を格納する | 出庫日時を格納する |  

CREATE TABLE `shukko` (  
 `shukkoId` int(11) NOT NULL AUTO_INCREMENT COMMENT '出庫情報を区別する連番',  
 `zaikoId` int(11) NOT NULL COMMENT '該当する在庫ID',  
 `outStock` int(11) NOT NULL DEFAULT '0' COMMENT '出庫数',  
 `outDate` datetime NOT NULL COMMENT '出庫日時',  
 PRIMARY KEY (`shukkoId`),  
 KEY `zaikoId` (`zaikoId`),  
 CONSTRAINT `shukko_ibfk_1` FOREIGN KEY (`zaikoId`) REFERENCES `zaiko` (`zaikoId`)  
) ENGINE=InnoDB DEFAULT CHARSET=utf8


ALTER TABLE `shukko` ADD FOREIGN KEY (`zaikoId`)  
REFERENCES `zaiko`(`zaikoId`)  
ON DELETE RESTRICT ON UPDATE RESTRICT;

#### テーブル、データベースの削除
##### drop table テーブル名
##### drop datebase データベース名

***

## インデックス
SQLにおける検索では先頭のレコードから順に検索していくため時間がかかる可能性がある  
したがって、本の索引番号の様に予め検索されそうなカラムにはインデックス用の数値を紐付ける  
ただしデータ更新の際にはインデックスも更新する必要があるため  
更新速度は落ちる傾向がある  

* ##### インデックスの種類
  * index  
  通常のインデックス  
  * unique  
  重複を許さないインデックス  
  * fulltext  
  全文検索に設定するインデックス  
  * spatial  

  基本構文
    alter table テーブル名 add index(カラム名);  
  インデックスの削除は  
    alter table テーブル名 drop index(インデックス名);

  SQLの実行計画を調べる  
    explain SQLクエリ;

***

## ビュー
select文の実行結果をテーブルとして扱う  
ビューを参照するたびにクエリが実行されるので、常に最新の状態を参照できる

基本構文   
  create view ビュー名 as select文  
  ex.  
  create view uriageWithTel as  
  select idur, uriage.company, charge, salesdate, tel  
  from uriage inner join jusho on  
  uriage.company = jusho.company;  

  ビューの削除  
    drop view ビュー名;


##### ビューを経由した更新
ビューに対する変更(更新、削除、追加)はビューのもとになっているテーブルにも反映される  
ただし変更の影響が複数のテーブルに及ぶ場合はエラーとなる

## トランザクション
* ##### トランザクションの特性
  * 原始性(Atomicity)  
  * 一貫性(Consistency)  
  * 分離性(Isolarion)  
  * 永続性(Durability)  


オートコミットを無効にするには  
set autocommit = 0;と記述  
commit;と記述されるまでコミットされなくなる  

複数のクエリをトランザクションとして処理する構文  
start transaction;  
SQL;  
SQL;  
SQL;  
... ;  
commit;  

ex.  
start transaction  
INSERT INTO shukko (shukkoId, zaikoId, outStock, outDate)  
VALUES (20179001, 8001, 30, '2017/09/25');  
UPDATE zaiko SET stock = stock - 30 WHERE zaikoId = 8001;  
commit;  
(在庫を30減らして出庫データに30登録する)


意図的にロールバックを発生させる構文
start transaction;  
SQL;  
SQL;  
... ;  
rollback;  

## ロック
* 共有ロック
  * 他のユーザは読み取りはできるが書き込みはできない  
  * 共有ロック中のものに他のユーザが共有ロックをかけることはできる  
* 排他ロック  
  * 他のユーザは読み取りも書き込みもできない
  * 排他ロック中のものに他のユーザは共有・排他ロックともにすることはできない  

SQLクエリ一文が実行されているときは自動的に排他制御がかかるが、  
トランザクションのような複数のクエリを一括で実行する際は  
明示的にロックをかける  

#### テーブルロック
lock tables... テーブル全体にロックをかける  
read... 共有ロックをかける  
write... 排他ロックをかける  
unlock tables... ロックを外す  

ex.  
start transaction;  
lock tables テーブル名 read(write);  
SQL文;  
unlock tables テーブル名;  
commit(rollback);  

#### レコードロック
更新処理のSQL文は自動的に排他ロックがかかる  
明示的に記述する場合は以下の構文を用いる  
lock in share mode... 共有ロックをかける  
for update... 排他ロックをかける  

ex.  
start transaction;  
select * from テーブル名  
SQL lock in share mode(for update);  
SQL文;  
commit(rollback);  

ex.  
start transaction;
select * from zaseki  
where idza = 'c27' for update;  
update zaseki  
set shimei = 'tanaka', yoyakubi = now()  
where idza = 'c27';  
commit;  

* ##### トランザクション分離
デフォルトはrepeatable readが設定されている  
  * read uncommitted  
    * コミットされていないデータを読み込む可能性がある(dirty read)が、高速
  * read commited  
    * コミットされたデータしか読み込まない  
    * 他のユーザの書き込み結果はすぐに反映される(fazzy read)  
  * repeatable read  
    * スナップショットを作成することで  
    他のユーザの操作がトランザクションに及ばない
  * serializable  
    * 完全分離するが、ロックの競合が発生しやすくパフォーマンスが落ちる可能性がある  
