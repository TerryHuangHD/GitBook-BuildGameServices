# Hosting 服務

> Hosting 提供了檔案形式的資源取得方式，在 Game Services 中最常見用途有：遊戲通用的資源，遊戲的更新檔案、使用者上傳的照片...等等。依照需求有各種實作和優化方式

Hosting 服務，可依照檔案變動性分為「動態檔案託管」與「靜態檔案託管」。

「動態檔案託管」用於存取使用者在使用過程中產生之檔案，具有較高的變動性。在 Parse 服務中，設計有 File 類型的資料格式，可透過 Adapter 來串接檔案服務，目前支援的有 MongoDB GridStore 服務, Amazon S3 服務, Google Cloud Storage(GCS) 服務, 本機端 File 儲存...等等。在 Firebase 服務中則提供了 Cloud Storage 服務

「靜態檔案託管」用於存取不會動態改變的檔案資源。在 Parse 中，需使用外部服務（如：GCS/S3 + CDN），或是透過設定讓伺服器直接提供 Hosting。在 Firebase 服務中則提供了 Hosting 服務，完整地提供了安全（SSL）、快速（CDN）、版本控管的服務

### 目錄

* [Parse File Adapter 設定](#adapter)
* [透過 Express 直接在 Parse Server 提供 Hosting 服務](#host)

### Parse File Adapter 設定 {#adapter}


### 透過 Express 直接在 Parse Server 提供 Hosting 服務 {#host}

