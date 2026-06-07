# GoTTY Web 終端

基於 [GoTTY](https://github.com/sorenisanerd/gotty) 的輕量 Web 終端應用，整合 [mulin/gotty-tmux](https://hub.docker.com/r/mulin/gotty-tmux) Docker 映像，透過 tmux 實現會話持久化。

## 特色

- **會話持久化**：基於 tmux 實作，關閉瀏覽器再打開，同一個 shell session 繼續執行不中斷
- **TTY 標準登入**：使用 Linux `/bin/login` 標準 TTY 登入流程，輸入 FnOS 主機帳密即可操作 Shell
- **nsenter 主機穿透**：容器內透過 nsenter 直接操作 FnOS 主機，不是容器內部 shell
- **超輕量**：Docker 映像僅 ~17.5 MB

## 安裝

透過 FnDepot 應用商店一鍵安裝，或手動下載 `.fpk` 使用 `appcenter-cli` 安裝：

```bash
appcenter-cli install gotty.fpk
```

## 使用說明

### 會話持久化

每次開啟終端都會接回同一個 tmux 會話（名為 `main`）。在裡面跑的程式（編輯器、編譯、下載等）不會因為關閉瀏覽器而中斷，重新打開即可繼續操作。

### TTY 登入

不使用預先設定的使用者，而是以標準 `/bin/login` 呈現 TTY 登入畫面：

```
FnOS login: mulin
Password:
```

輸入你的 FnOS 主機帳號密碼即可登入。登入後的使用者權限與直接登入 FnOS 一致。

### 登出與切換使用者

在終端內執行 `exit` 或 `logout` 即可登出，再次回到 login prompt，可切換其他使用者登入。

## 預設資訊

- 服務埠口：8080（安裝精靈可自訂）
- 外層存取帳號：admin（密碼留空則跳過外層認證，直接顯示 TTY login）

## 技術架構

```
FnOS 桌面圖示
    │
    └── https://fnos/app/gotty/
        │
        ├── FnOS Gateway Socket → socat → GoTTY
        │                                   │
        │                                   └── tmux new -A -s main
        │                                       │
        │                                       └── nsenter → host PID 1
        │                                                       │
        │                                                       └── /bin/login → bash
        │
        └── 斷線重連 → tmux attach → 同一 session，程式不中斷
```

## 原始碼

- Docker 映像：https://github.com/MuLinForest/gotty-tmux
- FnDepot 商店：https://github.com/MuLinForest/FnDepot
