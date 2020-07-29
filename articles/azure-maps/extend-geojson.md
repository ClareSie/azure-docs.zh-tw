---
title: 擴充的 GeoJSON 幾何 |Microsoft Azure 對應
description: 在本文中，您將瞭解 Microsoft Azure 對應如何擴充 GeoJSON 規格，以代表特定的幾何。
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84687492"
---
# <a name="extended-geojson-geometries"></a>擴充的 GeoJSON 幾何

Azure 地圖服務提供一份強大的 Api 清單，可供您在內和地理功能中進行搜尋。 這些 Api 會遵循代表地理功能的標準[GeoJSON 規格][1]。  

[GeoJSON 規格][1]僅支援下列幾何：

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Point
* 多邊形

某些 Azure 地圖服務 Api 會接受不屬於[GeoJSON 規格][1]的幾何。例如，Geometry API[內的搜尋](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)會接受圓形和多邊形。

本文詳細說明 Azure 地圖服務如何延伸 [GeoJSON 規格][1]來代表特定的幾何。

## <a name="circle"></a>Circle

`Circle` [GeoJSON 規格][1]不支援幾何。我們使用 `GeoJSON Point Feature` 物件來表示圓形。

`Circle`使用物件表示的 geometry `GeoJSON Feature` __必須__包含下列座標和屬性：

- 中心

    圓形的中心是使用物件來表示 `GeoJSON Point` 。

- 半徑

    圓形的 `radius` 是以 `GeoJSON Feature` 的屬性表示。 半徑值的單位是_公尺_，而且必須屬於型別 `double`。

- 子類型

    圓形幾何也必須包含 `subType` 屬性。 這個屬性必須是的屬性的一部分 `GeoJSON Feature` ，而且其值應該是_Circle_

#### <a name="example"></a>範例

以下是使用物件表示圓形的方式 `GeoJSON Feature` 。 讓我們將圓形放在緯度：47.639754 和經度：-122.126986，然後為它指派等於100計量的半徑：

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>矩形

`Rectangle` [GeoJSON 規格][1]不支援幾何。我們使用 `GeoJSON Polygon Feature` 物件來代表一個矩形。 矩形擴充功能主要是由 Web SDK 的「繪圖工具」模組所使用。

`Rectangle`使用物件表示的 geometry `GeoJSON Polygon Feature` __必須__包含下列座標和屬性：

- 轉角

    矩形的角落會使用物件的座標來表示 `GeoJSON Polygon` 。 應該有五個座標，每個角落各一個。 和，第五個座標與第一個座標相同，以關閉多邊形環形。 這會假設這些座標組齊，而開發人員可以視需要將它們旋轉。

- 子類型

    矩形 geometry 也必須包含 `subType` 屬性。 這個屬性必須是的屬性的一部分 `GeoJSON Feature` ，而且其值應該是_矩形_

### <a name="example"></a>範例

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>後續步驟

深入瞭解如何在 Azure 地圖服務中 GeoJSON 資料：

> [!div class="nextstepaction"]
> [地理柵欄 GeoJSON 格式](geofence-geojson.md)

請參閱與 Azure 地圖服務和位置智慧應用程式相關聯的常見技術詞彙詞彙：

> [!div class="nextstepaction"]
> [Azure 地圖服務字彙](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
