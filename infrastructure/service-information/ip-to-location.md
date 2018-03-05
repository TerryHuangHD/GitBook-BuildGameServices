# IP 能獲得什麼資料

### 免費 IP 取得地點資訊 API

> IP 資料庫準確度在各國有很大差異，使用上須納入考量

* freegeoip 是以 MaxMind GeoLite2 資料庫建立的免費平台，可獲得國家、城市、時區、經緯度、區碼

```
http://freegeoip.net/json/
```

```
{  
   "ip":"XXX.XXX.XXX.XXX",
   "country_code":"TW",
   "country_name":"Taiwan",
   "region_code":"TNN",
   "region_name":"Tainan",
   "city":"Tainan City",
   "zip_code":"",
   "time_zone":"Asia/Taipei",
   "latitude":22.9908,
   "longitude":120.2133,
   "metro_code":0
}
```

* ip-api 可獲得國家、城市、時區、經緯度、區碼、ISP、Autonomous System

```
http://ip-api.com/json
```

```
{  
   "as":"AS3462 Data Communication Business Group",
   "city":"Tainan City",
   "country":"Taiwan",
   "countryCode":"TW",
   "isp":"HiNet",
   "lat":22.9908,
   "lon":120.2133,
   "org":"HiNet",
   "query":"XXX.XXX.XXX.XXX",
   "region":"TNN",
   "regionName":"Tainan",
   "status":"success",
   "timezone":"Asia/Taipei",
   "zip":""
}
```

### MaxMind 資料庫

MaxMind 擁有數一數二的 IP 數據資料庫，除了常見的地理位置：大陸區塊、國家、城市、郵政資訊、座標位置、時區、網路資訊之外，還能取得像是人均收入、人口密度...等等資訊

```
{
  "city":  {
      "confidence":  25,
      "geoname_id": 54321,
      "names":  {
          "de":    "Los Angeles",
          "en":    "Los Angeles",
          "es":    "Los Ángeles",
          "fr":    "Los Angeles",
          "ja":    "ロサンゼルス市",
          "pt-BR":  "Los Angeles",
          "ru":    "Лос-Анджелес",
          "zh-CN": "洛杉矶"
      }
  },
  "continent":  {
      "code":       "NA",
      "geoname_id": 123456,
      "names":  {
          "de":    "Nordamerika",
          "en":    "North America",
          "es":    "América del Norte",
          "fr":    "Amérique du Nord",
          "ja":    "北アメリカ",
          "pt-BR": "América do Norte",
          "ru":    "Северная Америка",
          "zh-CN": "北美洲"
 
      }
  },
  "country":  {
      "confidence":  75,
      "geoname_id":  6252001,
      "iso_code":    "US",
      "names":  {
          "de":     "USA",
          "en":     "United States",
          "es":     "Estados Unidos",
          "fr":     "États-Unis",
          "ja":     "アメリカ合衆国",
          "pt-BR":  "Estados Unidos",
          "ru":     "США",
          "zh-CN":  "美国"
      }
  },
  "location":  {
      "accuracy_radius":     20,
      "average_income":      128321,
      "latitude":            37.6293,
      "longitude":           -122.1163,
      "metro_code":          807,
      "population_density":  7122,
      "time_zone":           "America/Los_Angeles"
  },
  "postal": {
      "code":       "90001",
      "confidence": 10
  },
  "registered_country":  {
      "geoname_id":  6252001,
      "iso_code":    "US",
      "names":  {
          "de":     "USA",
          "en":     "United States",
          "es":     "Estados Unidos",
          "fr":     "États-Unis",
          "ja":     "アメリカ合衆国",
          "pt-BR":  "Estados Unidos",
          "ru":     "США",
          "zh-CN":  "美国"
      }
  },
  "represented_country":  {
      "geoname_id":  6252001,
      "iso_code":    "US",
      "names":  {
          "de":     "USA",
          "en":     "United States",
          "es":     "Estados Unidos",
          "fr":     "États-Unis",
          "ja":     "アメリカ合衆国",
          "pt-BR":  "Estados Unidos",
          "ru":     "США",
          "zh-CN":  "美国"
      },
      "type": "military"
  },
  "subdivisions":  [
      {
          "confidence":  50,
          "geoname_id":  5332921,
          "iso_code":    "CA",
          "names":  {
              "de":    "Kalifornien",
              "en":    "California",
              "es":    "California",
              "fr":    "Californie",
              "ja":    "カリフォルニア",
              "ru":    "Калифорния",
              "zh-CN": "加州"
          }
      }
  ],
  "traits": {
      "autonomous_system_number":      1239,
      "autonomous_system_organization": "Linkem IR WiMax Network",
      "domain":                        "example.com",
      "is_anonymous_proxy":            true,
      "is_satellite_provider":         true,
      "isp":                           "Linkem spa",
      "ip_address":                    "1.2.3.4",
      "organization":                  "Linkem IR WiMax Network",
      "user_type":                     "traveler"
  },
}
```