# Raspberry-pi-3B-Grafana-Study
這是一份樹莓派3B+建置Grafana，並在電腦端建立可視化儀表板的入門學習過程。

# Raspberry Pi Hardware Monitoring Dashboard with Grafana & Prometheus

這份文檔記錄了如何在 Raspberry Pi 3B+ 上，使用 Grafana、Prometheus 和 Node Exporter 建置一個即時硬體監控儀表板（CPU、RAM）。

## 系統架構
* **硬體**: Raspberry Pi 3B+
* **OS**: Raspberry Pi OS (64-bit full / Ubuntu Server)
* **Grafana**: 負責數據視覺化（儀表板）
* **Prometheus**: 時序資料庫，負責儲存數據
* **Node Exporter**: 負責採集樹莓派的硬體數據

---

## 1. 安裝步驟 (Installation)

### 第一步：安裝採集器與資料庫
首先安裝 `prometheus` (資料庫) 與 `prometheus-node-exporter` (硬體數據採集者)。

```bash
sudo apt update
sudo apt install prometheus
<img width="437" height="73" alt="image" src="https://github.com/user-attachments/assets/96b2252d-350f-433f-a606-ad0594744532" />

檢查是否正常啟動，看到綠字active即可
systemctl status prometheus
<img width="1286" height="116" alt="image" src="https://github.com/user-attachments/assets/47ae5f21-cccb-4a54-8558-de708d8acdc7" />
