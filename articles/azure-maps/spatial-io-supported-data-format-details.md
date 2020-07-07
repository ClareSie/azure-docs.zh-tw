---
title: 支援的資料格式詳細資料 |Microsoft Azure 對應
description: 瞭解如何在空間 IO 模組中剖析分隔的空間資料。
author: philmea
ms.author: philmea
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3353620f1751e939a04543115fe704555fb3bc21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "80334089"
---
# <a name="supported-data-format-details"></a>支援的資料格式詳細資料

本文提供所有 XML 標記的讀取和寫入支援，以及已知的文字幾何類型的詳細資訊。 它也會詳細說明如何在空間 IO 模組中剖析分隔的空間資料。

## <a name="supported-xml-namespaces"></a>支援的 XML 命名空間

空間 IO 模組支援來自下列命名空間的 XML 標記。

| 命名空間前置詞 | 命名空間 URI   | 備忘稿                                                                    |
|:------------------|:-----------------|:----------------------------------------|
| `atom`           | `http://www.w3.org/2005/Atom`   |                                         |
| `geo`            | `http://www.w3.org/2003/01/geo/wgs84_pos#`  | GeoRSS 檔中的唯讀支援。           |
| `georss`         | `http://www.georss.org/georss`  |                                                |
| `geourl`         | `http://geourl.org/rss/module/` | GeoRSS 檔中的唯讀支援。                       |
| `gml`            | `http://www.opengis.net/gml`    |                                                        |
| `gpx`            | `http://www.topografix.com/GPX/1/1` |                                                   |
| `gpxx`           | `http://www.garmin.com/xmlschemas/GpxExtensions/v3` | .GPX 檔中的唯讀支援。 剖析並使用 DisplayColor。 新增至圖形中繼資料的所有其他屬性。 |
| `gpx_style`      | `http://www.topografix.com/GPX/gpx_style/0/2`      | 在 .GPX 檔中支援。 使用線條色彩。 |
| `gx`             | `http://www.google.com/kml/ext/2.2` |                                                      |
| `kml`            | `http://www.opengis.net/kml/2.2`    |                                                      |
| `rss`            |                                 | 唯讀。 使用 Atom 格式的 GeoRSS 寫入。              |

## <a name="supported-xml-elements"></a>支援的 XML 元素

空間 IO 模組支援下列 XML 元素。 任何不支援的 XML 標記都會轉換成 JSON 物件。 然後，每個標記會當做屬性加入 `properties` 父圖形或圖層的欄位中。

### <a name="kml-elements"></a>KML 元素

空間 IO 模組支援下列 KML 元素。

| 元素名稱         | 讀取    | 寫入   | 備忘稿                                                                                                                      |
|----------------------|---------|---------|----------------------------------------------------------------------------------------------------------------------------|
| `address`            | partial | 是     | 物件已剖析，但未用於定點陣圖形。                                                                    |
| `AddressDetails`     | partial | 不可以      | 物件已剖析，但未用於定點陣圖形。                                                                    |
| `atom:author`        | 可以     | 可以     |                                                                                                                            |
| `atom:link`          | 可以     | 可以     |                                                                                                                            |
| `atom:name`          | 可以     | 是     |                                                                                                                            |
| `BalloonStyle`       | partial | partial | 不支援 `displayMode`。 已轉換成 `PopupTemplate` 。 若要寫入，請將 `popupTemplate` 屬性新增為您想要為其撰寫之功能的屬性。 |
| `begin`              | 可以     | 可以     |                                                                                                                            |
| `color`              | 可以     | 是     | 包含 `#AABBGGRR` 和 `#BBGGRR` 。 剖析成 CSS 色彩字串                                                           |
| `colorMode`          | 是     | 不可以      |                                                                                                                            |
| `coordinates`        | 是     | 可以     |                                                                                                                            |
| `Data`               | 可以     | 可以     |                                                                                                                            |
| `description`        | 可以     | 可以     |                                                                                                                            |
| `displayName`        | 可以     | 可以     |                                                                                                                            |
| `Document`           | 可以     | 是     |                                                                                                                            |
| `drawOrder`          | partial | 不可以      | 閱讀地面重迭並用來排序。 
| `east`               | 可以     | 可以     |                                                                                                                            |
| `end`                | 可以     | 可以     |                                                                                                                            |
| `ExtendedData`       | 可以     | 是     | 支援表單的不具類型 `Data` 、 `SimpleData` 或 `Schema` 和實體取代 `$[dataName]` 。                      |
| `extrude`            | partial | partial | 僅支援多邊形。 具有不同高度之多邊形的 MultiGeometry 將會細分為個別的功能。 不支援線條樣式。 高度為0的多邊形會轉譯為平面多邊形。 讀取時，會將外部環中第一個座標的高度新增為多邊形的 height 屬性。 然後，第一個座標的高度將用來呈現地圖上的多邊形。 |
| `fill`               | 可以     | 可以     |                                                                                                                            |
| `Folder`             | 可以     | 可以     |                                                                                                                            |
| `GroundOverlay`      | 可以     | 是     | `color`不支援                                                                                                   |
| `heading`            | partial | 不可以      | 已剖析，但不是由呈現 `SimpleDataLayer` 。 只有在將資料儲存在圖形的屬性中時，才會寫入。                 |
| `hotSpot`            | 是     | partial | 只有在將資料儲存在圖形的屬性中時，才會寫入。 單位只會輸出為「圖元」。                         |
| `href`               | 可以     | 是     |                                                                                                                            |
| `Icon`               | partial | partial | 已剖析，但不是由呈現 `SimpleDataLayer` 。 只有在包含 URI 資料時，才寫入圖形的 icon 屬性。 只支援 `href`。 |
| `IconStyle`          | partial | partial | `icon`、 `heading` 、 `colorMode` 和 `hotspots` 值會進行剖析，但不會呈現`SimpleDataLayer`         |
| `innerBoundaryIs`    | 可以     | 可以     |                                                                                                                            |
| `kml`                | 可以     | 是     |                                                                                                                            |
| `LabelStyle`         | 不可以      | 否      |                                                                                                                            |
| `LatLonBox`          | 是     | 可以     |                                                                                                                            |
| `gx:LatLonQuad`      | 可以     | 可以     |                                                                                                                            |
| `LinearRing`         | 可以     | 可以     |                                                                                                                            |
| `LineString`         | 可以     | 可以     |                                                                                                                            |
| `LineStyle`          | 可以     | 是     | 不支援 `colorMode`。                                                                                         |
| `Link`               | 是     | 不可以      | `href`網路連結只支援屬性。                                                                   |
| `MultiGeometry`      | partial | partial | 可能會在讀取時細分為個別功能。                                                                     |
| `name`               | 可以     | 可以     |                                                                                                                            |
| `NetworkLink`        | 是     | 不可以      | 連結必須與檔位於相同的網域。                                                                  |
| `NetworkLinkControl` | 不可以      | 否      |                                                                                                                            |
| `north`              | 是     | 可以     |                                                                                                                            |
| `open`               | 可以     | 可以     |                                                                                                                            |
| `outerBoundaryIs`    | 可以     | 可以     |                                                                                                                            |
| `outline`            | 可以     | 是     |                                                                                                                            |
| `overlayXY`          | 不可以      | 否      |                                                                                                                            |
| `Pair`               | partial | 不可以      | 僅 `normal` 支援中的樣式 `StyleMap` 。 不支援 `highlight`。                                   |
| `phoneNumber`        | 可以     | 是     |                                                                                                                            |
| `PhotoOverlay`       | 不可以      | 否      |                                                                                                                            |
| `Placemark`          | 是     | 可以     |                                                                                                                            |
| `Point`              | 可以     | 可以     |                                                                                                                            |
| `Polygon`            | 可以     | 可以     |                                                                                                                            |
| `PolyStyle`          | 可以     | 是     |                                                                                                                            |
| `Region`             | partial | partial | `LatLongBox`檔層級支援。                                                                      |
| `rotation`           | 不可以      | 否      |                                                                                                                            |
| `rotationXY`         | 否      | 否      |                                                                                                                            |
| `scale`              | 否      | 否      |                                                                                                                            |
| `Schema`             | 是     | 可以     |                                                                                                                            |
| `SchemaData`         | 可以     | 是     |                                                                                                                            |
| `schemaUrl`          | partial | 是     | 不支援從不包含在 KMZ 中的外部檔載入樣式。                             |
| `ScreenOverlay`      | 不可以      | 否      |                                                                                                                            |
| `screenXY`           | 否      | 否      |                                                                                                                            |
| `SimpleData`         | 是     | 可以     |                                                                                                                            |
| `SimpleField`        | 可以     | 是     |                                                                                                                            |
| `size`               | 不可以      | 否      |                                                                                                                            |
| `Snippet`            | partial | partial | `maxLines`已忽略屬性。                                                                                  |
| `south`              | 可以     | 可以     |                                                                                                                            |
| `Style`              | 可以     | 是     |                                                                                                                            |
| `StyleMap`           | partial | 不可以      | 僅支援中的一般樣式 `StyleMap` 。                                                                        |
| `styleUrl`           | partial | 是     | 不支援外部樣式 Url。                                                                         |
| `text`               | 可以     | 是     | `$[geDirections]`不支援取代                                                                          |
| `textColor`          | 可以     | 是     |                                                                                                                            |
| `TimeSpan`           | 是     | 是     |                                                                                                                            |
| `TimeStamp`          | 是     | 是     |                                                                                                                            |
| `value`              | 是     | 是     |                                                                                                                            |
| `viewRefreshMode`    | partial | 不可以      |  如果指向 WMS 服務，則只 `onStop` 支援基礎重迭。 會 `BBOX=[bboxWest],[bboxSouth],[bboxEast],[bboxNorth]` 在地圖移動時附加至 URL 和更新。  |
| `visibility`         | 可以     | 是     |                                                                                                                            |
| `west`               | 是     | 是     |                                                                                                                            |
| `when`               | 是     | 是     |                                                                                                                            |
| `width`              | 是     | 是     |                                                                                                                            |

### <a name="georss-elements"></a>GeoRSS 元素

空間 IO 模組支援下列 GeoRSS 元素。

| 元素名稱             | 讀取    | 寫入 | 備忘稿                                                                                          |
|--------------------------|---------|-------|------------------------------------------------------------------------------------------------|
| `atom:author`            | 可以     | 是   |                                                                                                |
| `atom:category`          | 是     | 是   |                                                                                                |
| `atom:content`           | 是     | 是   |                                                                                                |
| `atom:contributor`       | 是     | 是   |                                                                                                |
| `atom:email`             | 是     | 是   |                                                                                                |
| `atom:entry`             | 是     | 是   |                                                                                                |
| `atom:feed`              | 是     | 是   |                                                                                                |
| `atom:icon`              | 是     | 是   |                                                                                                |
| `atom:id`                | 是     | 是   |                                                                                                |
| `atom:link`              | 是     | 是   |                                                                                                |
| `atom:logo`              | 是     | 是   |                                                                                                |
| `atom:name`              | 是     | 是   |                                                                                                |
| `atom:published`         | 是     | 是   |                                                                                                |
| `atom:rights`            | 是     | 是   |                                                                                                |
| `atom:source`            | 是     | 是   |                                                                                                |
| `atom:subtitle`          | 是     | 是   |                                                                                                |
| `atom:summary`           | 是     | 是   |                                                                                                |
| `atom:title`             | 是     | 是   |                                                                                                |
| `atom:updated`           | 是     | 是   |                                                                                                |
| `atom:uri`               | 是     | 是   |                                                                                                |
| `geo:lat`                | 是     | 不可以    | 撰寫為 `georss:point` 。                                                                   |
| `geo:lon`                | 是     | 不可以    | 撰寫為 `georss:point` 。                                                                   |
| `geo:long`               | 是     | 不可以    | 撰寫為 `georss:point` 。                                                                   |
| `georss:box`             | 是     | 不可以    | 讀取為多邊形，並指定「 `subType` 矩形」的屬性                                |
| `georss:circle`          | 可以     | 是   |                                                                                                |
| `georss:elev`            | 是     | 是   |                                                                                                |
| `georss:featurename`     | 是     | 是   |                                                                                                |
| `georss:featuretypetag`  | 是     | 是   |                                                                                                |
| `georss:floor`           | 是     | 是   |                                                                                                |
| `georss:line`            | 是     | 是   |                                                                                                |
| `georss:point`           | 是     | 是   |                                                                                                |
| `georss:polygon`         | 是     | 是   |                                                                                                |
| `georss:radius`          | 是     | 可以   |                                                                                                |
| `georss:relationshiptag` | 可以     | 可以   |                                                                                                |
| `georss:where`           | 可以     | 可以   |                                                                                                |
| `geourl:latitude`        | 是     | 不可以    | 撰寫為 `georss:point` 。                                                                   |
| `geourl:longitude`       | 是     | 不可以    | 撰寫為 `georss:point` 。                                                                   |
| `position`               | 是     | 不可以    | 有些 XML 摘要會將 GML 包裝成位置標記，而不是使用標記來包裝它 `georss:where` 。 將會讀取此標記，但會使用 `georss:where` 標記寫入。 |
| `rss`                    | 是     | 不可以    | 以 ATOM 格式撰寫的 GeoRSS。                                                                 |
| `rss:author`             | 是     | partial | 撰寫為 `atom:author` 。                                                                 |
| `rss:category`           | 是     | partial | 撰寫為 `atom:category` 。                                                               |
| `rss:channel`            | 是     | 不可以    |                                                                                                |
| `rss:cloud`              | 是     | 不可以    |                                                                                                |
| `rss:comments`           | 是     | 不可以    |                                                                                                |
| `rss:copyright`          | 是     | partial | 以 `atom:rights` if 圖形撰寫的 `rights` `properties` 尚未擁有屬性。       |
| `rss:description`        | 是     | partial | 以 `atom:content` if 圖形撰寫的 `content` `properties` 尚未擁有屬性。      |
| `rss:docs`               | 是     | 不可以    |                                                                                                |
| `rss:enclosure`          | 是     | 不可以    |                                                                                                |
| `rss:generator`          | 是     | 不可以    |                                                                                                |
| `rss:guid`               | 是     | partial | 以 `atom:id` if 圖形撰寫的 `id` `properties` 尚未擁有屬性。         |
| `rss:image`              | 是     | partial | 以 `atom:logo` if 圖形撰寫的 `logo` `properties` 尚未擁有屬性。      |
| `rss:item`               | 是     | partial | 撰寫為 `atom:entry` 。                                                                  |
| `rss:language`           | 是     | 不可以    |                                                                                                |
| `rss:lastBuildDate`      | 是     | partial | 以 `atom:updated` if 圖形撰寫的 `updated` `properties` 尚未擁有屬性。     |
| `rss:link`               | 是     | partial | 撰寫為 `atom:link` 。                                                                   |
| `rss:managingEditor`     | 是     | partial | 撰寫為 `atom:contributor` 。                                                            |
| `rss:pubDate`            | 是     | partial | 以 `atom:published` if 圖形撰寫的 `published` `properties` 尚未擁有屬性。  |
| `rss:rating`             | 是     | 不可以    |                                                                                                |
| `rss:skipDays`           | 是     | 不可以    |                                                                                                |
| `rss:skipHours`          | 是     | 不可以    |                                                                                                |
| `rss:source`             | 是     | partial | 撰寫為 `atom:source` 包含的 `atom:link` 。                                       |
| `rss:textInput`          | 是     | 不可以    |                                                                                                |
| `rss:title`              | 是     | partial | 撰寫為 `atom:title` 。                                                                  |
| `rss:ttl`                | 是     | 不可以    |                                                                                                |
| `rss:webMaster`          | 是     | 不可以    |                                                                                                |

### <a name="gml-elements"></a>GML 元素

空間 IO 模組支援下列 GML 元素。 

| 元素名稱            | 讀取 | 寫入 | 備忘稿                                                                                  |
|-------------------------|------|-------|----------------------------------------------------------------------------------------|
| `gml:coordinates`       | 是  | 不可以    | 撰寫為 `gml:posList` 。                                                              |
| `gml:curveMember`       | 是  | 不可以    |                                                                                        |
| `gml:curveMembers`      | 是  | 不可以    |                                                                                        |
| `gml:Box`               | 是  | 不可以    | 撰寫為 `gml:Envelope` 。                                                             |
| `gml:description`       | 可以  | 可以   |                                                                                        |
| `gml:Envelope`          | 可以  | 可以   |                                                                                        |
| `gml:exterior`          | 可以  | 可以   |                                                                                        |
| `gml:Feature`           | 是  | 不可以    | 撰寫成圖形。                                                                    |
| `gml:FeatureCollection` | 是  | 不可以    | 寫成 geometry 集合。                                                      |
| `gml:featureMember`     | 是  | 不可以    | 寫成 geometry 集合。                                                      |
| `gml:geometry`          | 是  | 不可以    | 撰寫成圖形。                                                                    |
| `gml:geometryMember`    | 可以  | 可以   |                                                                                        |
| `gml:geometryMembers`   | 可以  | 可以   |                                                                                        |
| `gml:identifier`        | 可以  | 可以   |                                                                                        |
| `gml:innerBoundaryIs`   | 是  | 不可以    | 使用寫入 `gml.interior` 。                                                          |
| `gml:interior`          | 可以  | 可以   |                                                                                        |
| `gml:LinearRing`        | 可以  | 可以   |                                                                                        |
| `gml:LineString`        | 可以  | 可以   |                                                                                        |
| `gml:lineStringMember`  | 可以  | 可以   |                                                                                        |
| `gml:lineStringMembers` | 是  | 不可以    |                                                                                        |
| `gml:MultiCurve`        | 是  | 不可以    | 只會讀取 `gml:LineString` 成員。 撰寫為`gml.MultiLineString`                  |
| `gml:MultiGeometry`     | partial  | partial   | 僅讀取為 FeatureCollection。                                              |
| `gml:MultiLineString`   | 可以  | 可以   |                                                                                        |
| `gml:MultiPoint`        | 可以  | 可以   |                                                                                        |
| `gml:MultiPolygon`      | 可以  | 可以   |                                                                                        |
| `gml:MultiSurface`      | 是  | 不可以    | 只會讀取 `gml:Polygon` 成員。 撰寫為`gml.MultiPolygon`                        |
| `gml:name`              | 可以  | 可以   |                                                                                        |
| `gml:outerBoundaryIs`   | 是  | 不可以    | 使用寫入 `gml.exterior` 。                                                          |
| `gml:Point`             | 可以  | 可以   |                                                                                        |
| `gml:pointMember`       | 可以  | 可以   |                                                                                        |
| `gml:pointMembers`      | 是  | 不可以    |                                                                                        |
| `gml:Polygon`           | 是  | 可以   |                                                                                        |
| `gml:polygonMember`     | 可以  | 可以   |                                                                                        |
| `gml:polygonMembers`    | 是  | 不可以    |                                                                                        |
| `gml:pos`               | 是  | 可以   |                                                                                        |
| `gml:posList`           | 可以  | 可以   |                                                                                        |
| `gml:surfaceMember`     | 可以  | 是   |                                                                                        |

#### <a name="additional-notes"></a>其他注意事項

- 會搜尋成員專案，以找出可能會隱藏在子專案中的 geometry。 這是必要的搜尋作業，因為延伸自 GML 的 XML 格式可能不會將幾何當做成員元素的直接子系。
- `srsName`部分支援 WGS84 座標和下列程式碼：[EPSG： 4326](https://epsg.io/4326)）和 web Mercator （[EPSG： 3857](https://epsg.io/3857)或其替代程式碼的其中之一。 任何其他座標系統都會依 WGS84 剖析為。
- 除非在讀取 XML 摘要時指定，否則會根據 XML 摘要中的提示來決定軸順序。 [緯度，經度] 軸順序會提供喜好設定。
- 除非在寫入 GML 檔案時，針對屬性指定了自訂 GML 命名空間，否則將不會新增其他屬性資訊。

### <a name="gpx-elements"></a>.GPX 元素

空間 IO 模組支援下列 .GPX 元素。

| 元素名稱             | 讀取    | 寫入   | 備忘稿                                                                                       |
|--------------------------|---------|---------|---------------------------------------------------------------------------------------------|
| `gpx:ageofdgpsdata`      | 可以     | 可以     |                                                                                             |
| `gpx:author`             | 可以     | 可以     |                                                                                             |
| `gpx:bounds`             | 可以     | 是     | 在讀取時轉換成 LocationRect。                                                    |
| `gpx:cmt`                | 可以     | 可以     |                                                                                             |
| `gpx:copyright`          | 可以     | 是     |                                                                                             |
| `gpx:desc`               | 是     | 是     | 在讀取時複製到 description 屬性，以配合其他 XML 格式。               |
| `gpx:dgpsid`             | 可以     | 是     |                                                                                             |
| `gpx:ele`                | 是     | 是     |                                                                                             |
| `gpx:extensions`         | partial | partial | 讀取時，會解壓縮樣式資訊。 所有其他延伸模組將會壓平合併為簡單的 JSON 物件。 只會寫入圖形樣式資訊。 |
| `gpx:geoidheight`        | 可以     | 是     |                                                                                             |
| `gpx:gpx`                | 是     | 是     |                                                                                             |
| `gpx:hdop`               | 是     | 是     |                                                                                             |
| `gpx:link`               | 是     | 是     |                                                                                             |
| `gpx:magvar`             | 是     | 是     |                                                                                             |
| `gpx:metadata`           | 是     | 是     |                                                                                             |
| `gpx:name`               | 是     | 是     |                                                                                             |
| `gpx:pdop`               | 是     | 是     |                                                                                             |
| `gpx:rte`                | 是     | 是     |                                                                                             |
| `gpx:rtept`              | 是     | 是     |                                                                                             |
| `gpx:sat`                | 是     | 是     |                                                                                             |
| `gpx:src`                | 是     | 是     |                                                                                             |
| `gpx:sym`                | 是     | 是     | 會捕捉值，但不會用來改變圖釘圖示。                               |
| `gpx:text`               | 可以     | 是     |                                                                                             |
| `gpx:time`               | 是     | 是     |                                                                                             |
| `gpx:trk`                | 是     | 是     |                                                                                             |
| `gpx:trkpt`              | 是     | 是     |                                                                                             |
| `gpx:trkseg`             | 是     | 是     |                                                                                             |
| `gpx:type`               | 是     | 是     |                                                                                             |
| `gpx:vdop`               | 是     | 是     |                                                                                             |
| `gpx:wpt`                | 是     | 是     |                                                                                             |
| `gpx_style:color`        | 是     | 是     |                                                                                             |
| `gpx_style:line`         | partial | partial | `color``opacity`支援、、、 `width` `lineCap` 。                                           |
| `gpx_style:opacity`      | 可以     | 是     |                                                                                             |
| `gpx_style:width`        | 是     | 是     |                                                                                             |
| `gpxx:DisplayColor`      | 是     | 不可以      | 用來指定圖形的色彩。 撰寫時， `gpx_style:line` 將改用色彩。  |
| `gpxx:RouteExtension`    | partial | 不可以      | 所有屬性都會讀入 `properties` 。 僅使用 `DisplayColor`。                     |
| `gpxx:TrackExtension`    | partial | 不可以      | 所有屬性都會讀入 `properties` 。 僅使用 `DisplayColor`。                     |
| `gpxx:WaypointExtension` | partial | 不可以      | 所有屬性都會讀入 `properties` 。 僅使用 `DisplayColor`。                     |
| `gpx:keywords`           | 可以     | 是     |                                                                                             |
| `gpx:fix`                | 是     | 是     |                                                                                             |

#### <a name="additional-notes"></a>其他注意事項

撰寫時;

- MultiPoints 將會細分為個別的之導航點。
- 多邊形和 MultiPolygons 會以追蹤的形式寫入。 
  
## <a name="supported-well-known-text-geometry-types"></a>支援的知名文字幾何類型

| Geometry 類型 | 讀取 | 寫入 |
|--------------|:----:|:-----:|
| 此處 | x | x |
| 點 Z | x | x | 
| 點 M | x | x<sup>[2]</sup> |
| 點 ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| LINESTRING | x | x |
| LINESTRING Z | x | x | 
| LINESTRING M | x | x<sup>[2]</sup> |
| LINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| 多邊 | x | x |
| 多邊形 Z | x | x |
| 多邊形 M | x | x<sup>[2]</sup> |
| 多邊形 ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTIPOINT | x | x |
| MULTIPOINT Z | x | x | 
| MULTIPOINT M | x | x<sup>[2]</sup> |
| POMULTIPOINTINT ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTILINESTRING | x | x |
| MULTILINESTRING Z | x | x | 
| MULTILINESTRING M | x | x<sup>[2]</sup> |
| MULTILINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTIPOLYGON | x | x |
| MULTIPOLYGON Z | x | x | 
| MULTIPOLYGON M | x | x<sup>[2]</sup> |
| MULTIPOLYGON ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| GEOMETRYCOLLECTION | x | x |
| GEOMETRYCOLLECTION Z | x | x | 
| GEOMETRYCOLLECTION M | x | x<sup>[2]</sup> | 
| GEOMETRYCOLLECTION ZM | x<sup>[1]</sup><sup>[2]</sup> | x | 

\[1 \] 只有 Z 參數會被捕捉並新增為位置值中的第三個值。

\[\]未捕捉到 2 M 參數。

## <a name="delimited-spatial-data-support"></a>分隔的空間資料支援

分隔的空間資料（例如逗號分隔值檔案（CSV））通常具有包含空間資料的資料行。 例如，可能有包含緯度和經度資訊的資料行。 在知名的文字格式中，可能會有一個包含空間幾何資料的資料行。

### <a name="spatial-data-column-detection"></a>空間資料行偵測

當讀取包含空間資料的分隔檔時，將會分析標頭，以判斷哪些資料行包含位置欄位。 如果標頭包含型別資訊，則會使用它將資料格值轉換成適當的型別。 如果未指定標頭，則會分析第一個資料列，並使用它來產生標頭。 分析第一個資料列時，會以不區分大小寫的方式執行檢查，以符合具有下列名稱的資料行名稱。 如果檔案中有兩個或多個名稱，則名稱的順序會是優先順序。

#### <a name="latitude"></a>緯度

- `latitude`
- `lat`
- `latdd`
- `lat_dd`
- `latitude83`
- `latdecdeg`
- `y`
- `ycenter`
- `point-y`

#### <a name="longitude"></a>經度

- `longitude`
- `lon`
- `lng`
- `long`
- `longdd`
- `long_dd`
- `longitude83`
- `longdecdeg`
- `x`
- `xcenter`
- `point-x`

#### <a name="elevation"></a>Elevation

- `elevation`
- `elv`
- `altitude`
- `alt`
- `z`

#### <a name="geography"></a>[地理位置]

資料的第一列將會掃描為已知文字格式的字串。 

### <a name="delimited-data-column-types"></a>分隔的資料行類型

掃描標題列時，資料行名稱中的任何型別資訊都會被解壓縮，並用來轉換該欄中的儲存格。 以下是具有類型值的資料行名稱範例： "ColumnName （typeName）"。 支援下列不區分大小寫的類型名稱：

#### <a name="numbers"></a>數字

- edm。 int64
- int
- long
- edm. double
- FLOAT
- double
- number

#### <a name="booleans"></a>布林值

- edm。布林值
- bool
- boolean

#### <a name="dates"></a>日期

- edm. datetime
- date
- Datetime

#### <a name="geography"></a>[地理位置]

- edm. geography
- geography

#### <a name="strings"></a>字串

- edm 字串
- varchar
- text
- 字串

如果沒有可從標頭解壓縮的類型資訊，而且在讀取時啟用動態輸入選項，則會個別分析每個資料格，以判斷哪一個最適合做為轉換的資料類型。

## <a name="next-steps"></a>後續步驟

請參閱下列文章，以取得更多可新增至地圖的程式碼範例：

> [!div class="nextstepaction"]
> [讀取和寫入空間資料](spatial-io-read-write-spatial-data.md)
