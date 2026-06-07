# GoTTY Web 終端

基於 [GoTTY](https://github.com/sorenisanerd/gotty) 的輕量 Web 終端應用，整合 [mulin/gotty-tmux](https://hub.docker.com/r/mulin/gotty-tmux) Docker 映像，支援 tmux 會話持久化。

## 特色

- **超輕量**：Docker 映像僅 ~16.9 MB
- **會話持久化**：內建 tmux，關瀏覽器再開可重新接回原會話
- **密碼認證**：Basic Auth 保護，避免未授權存取
- **即開即用**：點桌面圖示 → 瀏覽器開啟 → 直接操作 Shell

## 安裝

透過 FnDepot 應用商店一鍵安裝，或手動下載 `.fpk` 使用 `appcenter-cli` 安裝：

```bash
appcenter-cli install gotty.fpk
```

## 預設資訊

- 服務埠口：8080（安裝精靈可自訂）
- 存取帳號：admin
- 存取密碼：安裝精靈中設定（留空則不啟用認證）

## 技術架構

```
FnOS 桌面 → browser → Docker (mulin/gotty-tmux)
                         ├── GoTTY (WebSocket 終端)
                         └── tmux (會話持久化)
```

## 原始碼

- Docker 映像：https://github.com/MuLinForest/gotty-tmux
- FnDepot 商店：https://github.com/MuLinForest/FnDepot
