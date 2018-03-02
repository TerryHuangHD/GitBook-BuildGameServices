# Hosting 服務

> Hosting 提供了檔案形式的資源取得方式，在 Game Services 中最常見用途有：遊戲通用的資源，遊戲的更新檔案、使用者上傳的照片...等等。依照需求有各種實作和優化方式

Hosting 服務，可依照檔案變動性分為「動態檔案託管」與「靜態檔案託管」。

「動態檔案託管」用於存取使用者在使用過程中產生之檔案，具有較高的變動性。在 Parse 服務中，設計有 File 類型的資料格式，可透過 Adapter 來串接檔案服務，目前支援的有 MongoDB GridStore 服務, Amazon S3 服務, Google Cloud Storage(GCS) 服務, 本機端 File 儲存...等等。在 Firebase 服務中則提供了 Cloud Storage 服務

「靜態檔案託管」用於存取不會動態改變的檔案資源。在 Parse 中，需使用外部服務（如：GCS/S3 + CDN），或是透過設定讓伺服器直接提供 Hosting。在 Firebase 服務中則提供了 Hosting 服務，完整地提供了安全（SSL）、快速（CDN）、版本控管的服務

### 目錄

* [Parse File Adapter 設定](#adapter)
* [透過 Express 直接在 Parse Server 提供 Hosting 服務](#host)

### Parse File Adapter 設定 {#adapter}

* MongoDB GridStore：若您使用 MongoDB 作為 Parse 資料庫，您可以不用而外的設定，便可以用 MongoDB GridStore 作為檔案伺服

* Google Cloud Storage：使用 Google Cloud Storage 來存放檔案，請使用 [parse-server-gcs-adapter](https://github.com/parse-community/parse-server-gcs-adapter)

```
{
  "appId": 'my_app_id',
  "masterKey": 'my_master_key',
  // ...
  "filesAdapter": {
    "module": "@parse/gcs-files-adapter",
    "options": {
      "projectId": "projectId",
      "keyFilename": "path/to/keyfile",
      "bucket": "my_bucket",
      // optional:
      "bucketPrefix": '', // default value
      "directAccess": false // default value
    } 
  }
}
```

* Amazon S3：使用 Amazon S3 來存放檔案，請使用  [parse-server-s3-adapter](https://github.com/parse-community/parse-server-s3-adapter)

```
{
  "appId": 'my_app_id',
  "masterKey": 'my_master_key',
  // ...
  "filesAdapter": {
    "module": "@parse/s3-files-adapter",
    "options": {
      "bucket": "my_bucket",
      // optional:
      "region": 'us-east-1', // default value
      "bucketPrefix": '', // default value
      "directAccess": false, // default value
      "baseUrl": null, // default value
      "baseUrlDirect": false, // default value
      "signatureVersion": 'v4', // default value
      "globalCacheControl": null, // default value. Or 'public, max-age=86400' for 24 hrs Cache-Control
      "ServerSideEncryption": 'AES256|aws:kms' //AES256 or aws:kms, or if you do not pass this, encryption won't be done
    }
  }
}
```

* File Adapter：使用 Parse 主機端的空間來存放檔案，請使用  [parse-server-fs-adapter](https://github.com/parse-community/parse-server-fs-adapter)

```
{
  "appId": 'my_app_id',
  "masterKey": 'my_master_key',
  // ...
  "filesAdapter": {
    "module": "@parse/fs-files-adapter",
    "options": {
      "filesSubDirectory": "my/files/folder" // optional
    } 
  }
}
```

### 透過 Express 直接在 Parse Server 提供 Hosting 服務 {#host}

可在掛載 parse 的 express 中以 express.static 掛載靜態檔案，比如以下範例，將 /public route 至 public 資料夾

```
var app = express();
// ...
app.use('/public', express.static('public'));
```

除此之外，也可參考 [主題：在 Parse 服務架構簡易的 Cloud Code 部署機制](service-api/parse-cloud-deploy.md) 的方法，實作一個簡易 Deploy 的架構