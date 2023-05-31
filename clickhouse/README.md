# Clickhouse Задание

### 1. Загрузите в БД ClickHouse демонстрационные данные Yandex.Metrica. Инструкция тут: https://clickhouse.com/docs/ru/getting-started/tutorial

Приступаем к установке сервера и клиента Clickhouse:

```bash
sudo apt-get install -y apt-transport-https ca-certificates dirmngr
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 8919F6BD2B48D754

echo "deb https://packages.clickhouse.com/deb stable main" | sudo tee \
    /etc/apt/sources.list.d/clickhouse.list
sudo apt-get update
sudo apt-get install -y clickhouse-server clickhouse-client
```

Мы должны получить такой результат:
```bash
ClickHouse has been successfully installed.

Start clickhouse-server with:
 sudo clickhouse start

Start clickhouse-client with:
 clickhouse-client --password
```

Загрузим и извлечем табличные данные Yandex.Metrica dataset
```bash
curl https://datasets.clickhouse.com/hits/tsv/hits_v1.tsv.xz | unxz --threads=`nproc` > hits_v1.tsv
curl https://datasets.clickhouse.com/visits/tsv/visits_v1.tsv.xz | unxz --threads=`nproc` > visits_v1.tsv
```

Мы должны получить такой результат:
```bash
https://datasets.clickhouse.com/hits/tsv/hits_v1.tsv.xz | unxz --threads=`nproc` > hits_v1.tsv
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  802M  100  802M    0     0  4944k      0  0:02:46  0:02:46 --:--:-- 5277k
curl https://datasets.clickhouse.com/visits/tsv/visits_v1.tsv.xz | unxz --threads=`nproc` > visits_v1.tsv
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  405M  100  405M    0     0  3849k      0  0:01:47  0:01:47 --:--:-- 5736k
```

Создадим базу tutorial и таблицы hits_v1 и visits_v1 через клиент:

```bash
clickhouse-client
    ClickHouse client version 23.4.2.11 (official build).
    Connecting to localhost:9000 as user default.
    Password for user (default):
    Connecting to localhost:9000 as user default.
    Connected to ClickHouse server version 23.4.2 revision 54462.
```
```sql
CREATE DATABASE IF NOT EXISTS tutorial
    Query id: 608c8d1a-a426-49b5-ac3f-5e4cd398b302
    Ok.
    0 rows in set. Elapsed: 0.037 sec.

CREATE TABLE tutorial.hits_v1
(
    `WatchID` UInt64,
    `JavaEnable` UInt8,
    `Title` String,
    `GoodEvent` Int16,
    `EventTime` DateTime,
    `EventDate` Date,
    `CounterID` UInt32,
    `ClientIP` UInt32,
    `ClientIP6` FixedString(16),
    `RegionID` UInt32,
    `UserID` UInt64,
    `CounterClass` Int8,
    `OS` UInt8,
    `UserAgent` UInt8,
    `URL` String,
    `Referer` String,
    `URLDomain` String,
    `RefererDomain` String,
    `Refresh` UInt8,
    `IsRobot` UInt8,
    `RefererCategories` Array(UInt16),
    `URLCategories` Array(UInt16),
    `URLRegions` Array(UInt32),
    `RefererRegions` Array(UInt32),
    `ResolutionWidth` UInt16,
    `ResolutionHeight` UInt16,
    `ResolutionDepth` UInt8,
    `FlashMajor` UInt8,
    `FlashMinor` UInt8,
    `FlashMinor2` String,
    `NetMajor` UInt8,
    `NetMinor` UInt8,
    `UserAgentMajor` UInt16,
    `UserAgentMinor` FixedString(2),
    `CookieEnable` UInt8,
    `JavascriptEnable` UInt8,
    `IsMobile` UInt8,
    `MobilePhone` UInt8,
    `MobilePhoneModel` String,
    `Params` String,
    `IPNetworkID` UInt32,
    `TraficSourceID` Int8,
    `SearchEngineID` UInt16,
    `SearchPhrase` String,
    `AdvEngineID` UInt8,
    `IsArtifical` UInt8,
    `WindowClientWidth` UInt16,
    `WindowClientHeight` UInt16,
    `ClientTimeZone` Int16,
    `ClientEventTime` DateTime,
    `SilverlightVersion1` UInt8,
    `SilverlightVersion2` UInt8,
    `SilverlightVersion3` UInt32,
    `SilverlightVersion4` UInt16,
    `PageCharset` String,
    `CodeVersion` UInt32,
    `IsLink` UInt8,
    `IsDownload` UInt8,
    `IsNotBounce` UInt8,
    `FUniqID` UInt64,
    `HID` UInt32,
    `IsOldCounter` UInt8,
    `IsEvent` UInt8,
    `IsParameter` UInt8,
    `DontCountHits` UInt8,
    `WithHash` UInt8,
    `HitColor` FixedString(1),
    `UTCEventTime` DateTime,
    `Age` UInt8,
    `Sex` UInt8,
    `Income` UInt8,
    `Interests` UInt16,
    `Robotness` UInt8,
    `GeneralInterests` Array(UInt16),
    `RemoteIP` UInt32,
    `RemoteIP6` FixedString(16),
    `WindowName` Int32,
    `OpenerName` Int32,
    `HistoryLength` Int16,
    `BrowserLanguage` FixedString(2),
    `BrowserCountry` FixedString(2),
    `SocialNetwork` String,
    `SocialAction` String,
    `HTTPError` UInt16,
    `SendTiming` Int32,
    `DNSTiming` Int32,
    `ConnectTiming` Int32,
    `ResponseStartTiming` Int32,
    `ResponseEndTiming` Int32,
    `FetchTiming` Int32,
    `RedirectTiming` Int32,
    `DOMInteractiveTiming` Int32,
    `DOMContentLoadedTiming` Int32,
    `DOMCompleteTiming` Int32,
    `LoadEventStartTiming` Int32,
    `LoadEventEndTiming` Int32,
    `NSToDOMContentLoadedTiming` Int32,
    `FirstPaintTiming` Int32,
    `RedirectCount` Int8,
    `SocialSourceNetworkID` UInt8,
    `SocialSourcePage` String,
    `ParamPrice` Int64,
    `ParamOrderID` String,
    `ParamCurrency` FixedString(3),
    `ParamCurrencyID` UInt16,
    `GoalsReached` Array(UInt32),
    `OpenstatServiceName` String,
    `OpenstatCampaignID` String,
    `OpenstatAdID` String,
    `OpenstatSourceID` String,
    `UTMSource` String,
    `UTMMedium` String,
    `UTMCampaign` String,
    `UTMContent` String,
    `UTMTerm` String,
    `FromTag` String,
    `HasGCLID` UInt8,
    `RefererHash` UInt64,
    `URLHash` UInt64,
    `CLID` UInt32,
    `YCLID` UInt64,
    `ShareService` String,
    `ShareURL` String,
    `ShareTitle` String,
    `ParsedParams` Nested(
        Key1 String,
        Key2 String,
        Key3 String,
        Key4 String,
        Key5 String,
        ValueDouble Float64),
    `IslandID` FixedString(16),
    `RequestNum` UInt32,
    `RequestTry` UInt8
)
ENGINE = MergeTree()
PARTITION BY toYYYYMM(EventDate)
ORDER BY (CounterID, EventDate, intHash32(UserID))
SAMPLE BY intHash32(UserID) 

    Query id: c33ff3e5-fd0e-4dd2-b0e5-d6f98387831e
    Ok.
    0 rows in set. Elapsed: 0.049 sec.
```

```sql
CREATE TABLE tutorial.visits_v1
(
    `CounterID` UInt32,
    `StartDate` Date,
    `Sign` Int8,
    `IsNew` UInt8,
    `VisitID` UInt64,
    `UserID` UInt64,
    `StartTime` DateTime,
    `Duration` UInt32,
    `UTCStartTime` DateTime,
    `PageViews` Int32,
    `Hits` Int32,
    `IsBounce` UInt8,
    `Referer` String,
    `StartURL` String,
    `RefererDomain` String,
    `StartURLDomain` String,
    `EndURL` String,
    `LinkURL` String,
    `IsDownload` UInt8,
    `TraficSourceID` Int8,
    `SearchEngineID` UInt16,
    `SearchPhrase` String,
    `AdvEngineID` UInt8,
    `PlaceID` Int32,
    `RefererCategories` Array(UInt16),
    `URLCategories` Array(UInt16),
    `URLRegions` Array(UInt32),
    `RefererRegions` Array(UInt32),
    `IsYandex` UInt8,
    `GoalReachesDepth` Int32,
    `GoalReachesURL` Int32,
    `GoalReachesAny` Int32,
    `SocialSourceNetworkID` UInt8,
    `SocialSourcePage` String,
    `MobilePhoneModel` String,
    `ClientEventTime` DateTime,
    `RegionID` UInt32,
    `ClientIP` UInt32,
    `ClientIP6` FixedString(16),
    `RemoteIP` UInt32,
    `RemoteIP6` FixedString(16),
    `IPNetworkID` UInt32,
    `SilverlightVersion3` UInt32,
    `CodeVersion` UInt32,
    `ResolutionWidth` UInt16,
    `ResolutionHeight` UInt16,
    `UserAgentMajor` UInt16,
    `UserAgentMinor` UInt16,
    `WindowClientWidth` UInt16,
    `WindowClientHeight` UInt16,
    `SilverlightVersion2` UInt8,
    `SilverlightVersion4` UInt16,
    `FlashVersion3` UInt16,
    `FlashVersion4` UInt16,
    `ClientTimeZone` Int16,
    `OS` UInt8,
    `UserAgent` UInt8,
    `ResolutionDepth` UInt8,
    `FlashMajor` UInt8,
    `FlashMinor` UInt8,
    `NetMajor` UInt8,
    `NetMinor` UInt8,
    `MobilePhone` UInt8,
    `SilverlightVersion1` UInt8,
    `Age` UInt8,
    `Sex` UInt8,
    `Income` UInt8,
    `JavaEnable` UInt8,
    `CookieEnable` UInt8,
    `JavascriptEnable` UInt8,
    `IsMobile` UInt8,
    `BrowserLanguage` UInt16,
    `BrowserCountry` UInt16,
    `Interests` UInt16,
    `Robotness` UInt8,
    `GeneralInterests` Array(UInt16),
    `Params` Array(String),
    `Goals` Nested(
        ID UInt32,
        Serial UInt32,
        EventTime DateTime,
        Price Int64,
        OrderID String,
        CurrencyID UInt32),
    `WatchIDs` Array(UInt64),
    `ParamSumPrice` Int64,
    `ParamCurrency` FixedString(3),
    `ParamCurrencyID` UInt16,
    `ClickLogID` UInt64,
    `ClickEventID` Int32,
    `ClickGoodEvent` Int32,
    `ClickEventTime` DateTime,
    `ClickPriorityID` Int32,
    `ClickPhraseID` Int32,
    `ClickPageID` Int32,
    `ClickPlaceID` Int32,
    `ClickTypeID` Int32,
    `ClickResourceID` Int32,
    `ClickCost` UInt32,
    `ClickClientIP` UInt32,
    `ClickDomainID` UInt32,
    `ClickURL` String,
    `ClickAttempt` UInt8,
    `ClickOrderID` UInt32,
    `ClickBannerID` UInt32,
    `ClickMarketCategoryID` UInt32,
    `ClickMarketPP` UInt32,
    `ClickMarketCategoryName` String,
    `ClickMarketPPName` String,
    `ClickAWAPSCampaignName` String,
    `ClickPageName` String,
    `ClickTargetType` UInt16,
    `ClickTargetPhraseID` UInt64,
    `ClickContextType` UInt8,
    `ClickSelectType` Int8,
    `ClickOptions` String,
    `ClickGroupBannerID` Int32,
    `OpenstatServiceName` String,
    `OpenstatCampaignID` String,
    `OpenstatAdID` String,
    `OpenstatSourceID` String,
    `UTMSource` String,
    `UTMMedium` String,
    `UTMCampaign` String,
    `UTMContent` String,
    `UTMTerm` String,
    `FromTag` String,
    `HasGCLID` UInt8,
    `FirstVisit` DateTime,
    `PredLastVisit` Date,
    `LastVisit` Date,
    `TotalVisits` UInt32,
    `TraficSource` Nested(
        ID Int8,
        SearchEngineID UInt16,
        AdvEngineID UInt8,
        PlaceID UInt16,
        SocialSourceNetworkID UInt8,
        Domain String,
        SearchPhrase String,
        SocialSourcePage String),
    `Attendance` FixedString(16),
    `CLID` UInt32,
    `YCLID` UInt64,
    `NormalizedRefererHash` UInt64,
    `SearchPhraseHash` UInt64,
    `RefererDomainHash` UInt64,
    `NormalizedStartURLHash` UInt64,
    `StartURLDomainHash` UInt64,
    `NormalizedEndURLHash` UInt64,
    `TopLevelDomain` UInt64,
    `URLScheme` UInt64,
    `OpenstatServiceNameHash` UInt64,
    `OpenstatCampaignIDHash` UInt64,
    `OpenstatAdIDHash` UInt64,
    `OpenstatSourceIDHash` UInt64,
    `UTMSourceHash` UInt64,
    `UTMMediumHash` UInt64,
    `UTMCampaignHash` UInt64,
    `UTMContentHash` UInt64,
    `UTMTermHash` UInt64,
    `FromHash` UInt64,
    `WebVisorEnabled` UInt8,
    `WebVisorActivity` UInt32,
    `ParsedParams` Nested(
        Key1 String,
        Key2 String,
        Key3 String,
        Key4 String,
        Key5 String,
        ValueDouble Float64),
    `Market` Nested(
        Type UInt8,
        GoalID UInt32,
        OrderID String,
        OrderPrice Int64,
        PP UInt32,
        DirectPlaceID UInt32,
        DirectOrderID UInt32,
        DirectBannerID UInt32,
        GoodID String,
        GoodName String,
        GoodQuantity Int32,
        GoodPrice Int64),
    `IslandID` FixedString(16)
)
ENGINE = CollapsingMergeTree(Sign)
PARTITION BY toYYYYMM(StartDate)
ORDER BY (CounterID, StartDate, intHash32(UserID), VisitID)
SAMPLE BY intHash32(UserID)

    Query id: 7f5ba6bb-b320-469f-87ec-40d508b8e2d8
    Ok.
    0 rows in set. Elapsed: 0.024 sec.
```

Импортируем данные из файлов hits_v1 и visits_v1 в базу Clickhouse через клиент:
```sql
clickhouse-client --query "INSERT INTO tutorial.hits_v1 FORMAT TSV" --max_insert_block_size=100000 < hits_v1.tsv --password "******************"
clickhouse-client --query "INSERT INTO tutorial.visits_v1 FORMAT TSV" --max_insert_block_size=100000 < visits_v1.tsv --password "******************"
```

Проверяем что все успешно загрузилось:
```sql
SELECT COUNT(*)
FROM tutorial.hits_v1

    Query id: 45261922-8de5-4e95-ab47-1b488616d9c3
    ┌─count()─┐
    │ 8873898 │
    └─────────┘
    1 row in set. Elapsed: 0.027 sec.
```
```sql
SELECT COUNT(*)
FROM tutorial.visits_v1

    Query id: 68ff9451-c001-40df-80e8-1329743d8709
    ┌─count()─┐
    │ 1680609 │
    └─────────┘
    1 row in set. Elapsed: 0.006 sec.
```


### 2. Задание 2 По данным из набора «visits_v1» составьте следующие запросы:

### 2.1 Запрос 1: Возвращает список, содержащий Ip адреса клиентов (поле ClientIP) и количество записей по каждому адресу. Список должен быть отсортирован по количеству по убыванию и содержать не более 10 строк.

```sql
SELECT ClientIP, COUNT(*) AS count
FROM tutorial.visits_v1
GROUP BY ClientIP
ORDER BY count DESC
LIMIT 10;

    Query id: b990b7e7-520c-4176-b74a-adb666da34b1
        `Title` String,
    ┌───ClientIP─┬─count─┐
    │ 2263631863 │  1310 │me,
    │ 1228771210 │   995 │
    │  118030920 │   913 │,
    │  466595077 │   872 │
    │ 3557957762 │   816 │tring(16),
    │ 4092462721 │   805 │
    │  370096382 │   798 │
    │ 2244345465 │   755 │8,
    │ 1810835888 │   722 │
    │ 3074919315 │   712 │
    └────────────┴───────┘
    10 rows in set. Elapsed: 0.059 sec. Processed 1.68 million rows, 6.72 MB (28.41 million rows/s., 113.65 MB/s.)
```

### 2.2 Запрос 2: Возвращает список доменов (поле RefererDomain) и количество записей по каждому домену. В список должны попасть только те домены, у которых количество записей превышает значение 3000. Список должен быть отсортирован по количеству по убыванию и содержать не более 10 строк.
```sql
SELECT
    RefererDomain,
    count(*) AS count
FROM tutorial.visits_v1
GROUP BY RefererDomain
HAVING count > 3000
ORDER BY count DESC
LIMIT 10

    Query id: 196adbfa-524e-4b59-9b6c-947ef39ee3d6
    ┌─RefererDomain──────┬──count─┐
    │                    │ 540266 │
    │ ukr.net.ru.perfb   │ 346237 │
    │ yandex.ru          │ 290505 │
    │ go.mail.ru         │  54953 │
    │ go.mail.yandex.ru  │  35573 │
    │ news.rostop        │  27633 │
    │ m.facebook.com     │  26512 │
    │ go.mail.ru.msn.com │  21371 │
    │ takep.ru           │  19483 │
    │ yandex.ua          │  16259 │
    └────────────────────┴────────┘
    10 rows in set. Elapsed: 0.040 sec. Processed 1.68 million rows, 29.94 MB (42.45 million rows/s., 756.40 MB/s.)
```

### 2.3 Запрос 3: Возвращает список с датами и количеством строк по каждой дате, где в поле RefererDomain указано значение: «yandex.com.tr». Список должен содержать данные за период с 18.03.2014 по 22.03.2014 и должен быть отсортирован по датам по возрастанию.

```sql
SELECT
    toDate(ClientEventTime) AS Date,
    COUNT(*) AS cnt
FROM tutorial.visits_v1
WHERE ((toDate(ClientEventTime) >= '2014-03-18') AND (toDate(ClientEventTime) <= '2014-03-22')) AND (RefererDomain = 'yandex.com.tr')
GROUP BY toDate(ClientEventTime)
ORDER BY toDate(ClientEventTime) ASC

    Query id: 3f7d6ec2-9406-4ed9-9a1a-96857a3341dd
    ┌───────Date─┬─cnt─┐
    │ 2014-03-18 │  34 │
    │ 2014-03-19 │  35 │
    │ 2014-03-20 │  27 │
    │ 2014-03-21 │  27 │
    │ 2014-03-22 │  13 │
    └────────────┴─────┘
    5 rows in set. Elapsed: 0.051 sec. Processed 1.68 million rows, 30.84 MB (32.70 million rows/s., 600.08 MB/s.)
```

### 3. Задание 3 (не обязательно к выполнению, но будет большим плюсом, если выполните) По условиям из задания 2 постройте диаграммы в Superset. В качестве решения предоставьте скриншоты диаграмм с параметрами построения и SQL запросом.