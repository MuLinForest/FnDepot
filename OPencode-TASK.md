# OpenCode 任務：製作 GoTTY Web 終端 FPK

## 目標

為 FnOS 製作一個「Web 終端」FPK 應用包，基於 GoTTY Docker image，讓使用者在飛牛桌面點圖示就能打開瀏覽器終端。

## FPK 結構（Docker 型）

參考範例：https://github.com/kci-lnk/fn-knock-turborepo/tree/main/apps/fn-knock-docker

```
gotty/
├── manifest              ← 應用中繼資料
├── cmd/main              ← start/stop/status 腳本
├── config/
│   ├── privilege         ← 權限設定
│   └── resource          ← Docker 專案定義 + 資料共享
├── wizard/
│   └── install           ← 安裝精靈（首次設定埠口、密碼等）
├── app/
│   └── docker/
│       └── docker-compose.yaml  ← Docker Compose
├── ICON.PNG              ← 256x256 圖示
├── ICON_256.PNG
└── ICON_64.PNG
```

## GoTTY Docker 資訊

- Image: `sorenisanerd/gotty:latest`（31MB）
- 指令: `gotty -w -c <user:pass> tmux new -A -s main`
- 預設埠口: 8080（容器內）
- GitHub: https://github.com/sorenisanerd/gotty
- Docker Hub: https://hub.docker.com/r/sorenisanerd/gotty

## 要點

1. **manifest** 欄位：`appname=gotty`, `version=1.0.0`, `display_name=Web 終端`, `platform=x86`, `service_port=8080`, `install_type=root`
2. **cmd/main** 支援 `start`/`stop`/`status`，對 Docker 型就是 `docker compose up -d` / `down` / `ps`
3. **config/resource** 要宣告這是 docker-project，讓飛牛知道要幫管 docker-compose
4. **wizard/install** 提供安裝精靈，讓使用者設定埠口和終端密碼
5. **docker-compose.yaml** 把 GoTTY 跑起來，port mapping、環境變數用 `wizard_` 前綴變數（飛牛會在安裝時替換）
6. **ICON**：需要一個終端機圖示，256x256 PNG

## FnDepot 商店規範（之後要上架）

- GitHub repo 名必須叫 `FnDepot`，Public
- 根目錄放 `fnpack.json`（全局索引）
- 每個 app 一個子目錄，裡面放 `ICON.PNG`、`*.fpk`、`README.md`
- 規範詳見：https://github.com/EWEDLCM/FnDepot

## 產出

1. `/mnt/aivault/projects/FnDepot/gotty/` 完整目錄
2. 打包成 `gotty.fpk`（`tar czf gotty.fpk -C gotty .`）
3. 放到 FnOS 上用 `appcenter-cli install gotty.fpk` 測試
