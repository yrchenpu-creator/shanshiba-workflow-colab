# Google Colab（山獅霸的多重宇宙：Colab 雲端工作流）

> 這是給 AI agent（AntiGravity / Gemini）讀的專案入口與藍圖。
> **最新決策寫在最上面**，每條一段：日期、做了什麼、結論、連到詳細文件。被取代的不刪，標「已被取代」。

> **2026-10-11 專案初始化完成**：已 Fork 至 `yrchenpu-creator/shanshiba-workflow-colab`，設定 upstream 為 `mathruffian-dot/shanshiba-workflow-colab`，安裝完成 `google-colab-cli`。硬體偵測判定採「本機劇本/剪接 + 雲端 Colab G4 運算（H3/配音）」架構。

## 專案簡介

本專案基於「山獅霸的多重宇宙：AI 短片製作工作流（Colab 雲端版）」，利用 Google Colab G4 GPU（RTX PRO 6000 Blackwell 96GB）處理 MiniMax H3 影片生成與配音，其餘劇本、剪接、字幕在本地環境執行。

## 影片規格

| 項目 | 設定 |
|---|---|
| 畫面比例 | 16:9 橫式（Shorts 用 9:16 重新生成，不裁切） |
| 解析度 | H3 1344×768 → 放大到 1920×1080 |
| 幀率 | 24 fps |
| 單鏡長度 | ≤12 秒 |

## 必讀（開工前）

- H3：一律寫分鏡 JSON，用 `tools/h3_shot.py`（官方格式＋格式檢查），不手寫提示詞。
- 做片前讀：`docs/03_規則與踩坑.md`、`docs/04_自審清單.md`、`cloud/README.md`。
- 本機環境：macOS 16GB RAM，不具本機 NVIDIA 顯卡，H3 與繁重語音工作流請一律派送至 Colab。

## 預設做法

- **首幀**：雲端 Image 2.5（需 OpenAI 金鑰）或在 Colab 雲端完整環境生成。
- **影片**：Google Colab G4 跑 MiniMax H3（`python cloud/colab_h3.py batch ...`），只給首幀；需要時加尾幀。`"draft": true`（DMAD 4 步）為定稿。
- **配音**：在 Colab 雲端環境（`cloud/colab_h3.py full-setup`）執行 BreezyVoice / VoxCPM2 / Whisper。
- **配樂**：MiniMax Music 3 純器樂。
- **放大**：全景 FlashVSR。

## 工作習慣與收工原則

- 啟動 Colab VM 跑完後**務必關閉執行個體**（`python cloud/colab_h3.py stop`），避免浪費運算單元額度。
- 金鑰與個人憑證絕對不印出、不提交至 Git。
- 收工時檢查 `git status`，確認敏感資訊已排除，並更新 `handoff.md`。
