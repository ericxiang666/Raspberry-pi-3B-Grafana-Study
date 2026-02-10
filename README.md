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
首先安裝 `prometheus` (資料庫)。

```bash
sudo apt update
sudo apt install prometheus
```
<img width="437" height="70" alt="image" src="https://github.com/user-attachments/assets/4d5203d6-8c34-4942-a566-f67cc42f252d" />

檢查prometheus是否正常啟動，看到綠字active即可

```bash
systemctl status prometheus
```
<img width="1286" height="116" alt="image" src="https://github.com/user-attachments/assets/47ae5f21-cccb-4a54-8558-de708d8acdc7" />

換安裝 `prometheus-node-exporter` (硬體數據採集者)
```bash
sudo apt update
sudo apt install prometheus-node-exporter
```

<img width="587" height="59" alt="image" src="https://github.com/user-attachments/assets/178fd2cc-654f-460f-bfff-875c9da2eded" />

檢查prometheus-node-exporter是否正常啟動

```bash
systemctl status prometheus-node-exporter
```

<img width="1476" height="117" alt="image" src="https://github.com/user-attachments/assets/ae9ce797-772f-479b-b465-9b64e9fbcf42" />

---

### 第二步：安裝 Grafana
1.安裝必要的下載工具
```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
```

2.加入 Grafana 的 GPG 金鑰 (Key)
```bash
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

3.加入 Grafana 軟體源庫
```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

4.再次更新並安裝 Grafana
```bash
sudo apt-get update
sudo apt-get install grafana
```

5.啟動 Grafana 伺服器  
安裝完成後，Grafana 不會自動開啟，你需要手動啟動它，並設定開機自動啟動。
```bash
# 啟動 Grafana
sudo systemctl start grafana-server

# 設定開機自動啟動 (這樣重開機後不用重敲指令)
sudo systemctl enable grafana-server

# 檢查狀態 (看到綠色的 active (running) 代表成功)
sudo systemctl status grafana-server
```
<img width="857" height="483" alt="image" src="https://github.com/user-attachments/assets/e3e9184d-f065-4dc3-84dd-4e779fa1312d" />

---

### 第三步：配置 Grafana (Configuration)
1.登入介面：打開瀏覽器，輸入 http://<樹莓派IP>:3000 (預設帳密: admin/admin)。
<img width="1500" height="499" alt="image" src="https://github.com/user-attachments/assets/db1f47f3-ac5b-431f-a812-2bc228c113a0" />

2.新增數據源 (Data Source)：  
路徑：Connections -> Data sources -> Add new data source -> 選擇 Prometheus。
<img width="1917" height="658" alt="image" src="https://github.com/user-attachments/assets/952aa8f8-8d81-4e2e-8a89-d298591ca7b4" />  
<img width="1888" height="541" alt="image" src="https://github.com/user-attachments/assets/04e9435a-ad98-4260-84f6-3ac96ef8f9be" />  

3.設定連線 URL：  
在 URL 欄位輸入：http://localhost:9090  
原本Prometheus 預設端口是 9090，而非 9000，小心不要看錯。
<img width="1595" height="561" alt="image" src="https://github.com/user-attachments/assets/259f40fd-7fc9-4b21-9161-f1d8562ba489" />  
完成後點擊最下方的 Save & test。成功訊息：Successfully queried the Prometheus API.

---

### 第四步：建立儀表板 (Visualization)
<img width="1914" height="687" alt="image" src="https://github.com/user-attachments/assets/eb76e101-c021-4db4-ae47-bb656a604584" />  
選擇prometheus  
<img width="1190" height="730" alt="image" src="https://github.com/user-attachments/assets/f437c710-972d-474b-8a93-b12cdbfd0814" />  

**1.Metric (查詢指令): node_load1(Builder 模式)**    
**2.Run queries:亮起藍色按下去**  
**3.Visualization: Gauge (儀表盤)，可選擇Gauge模式**  
**4.title改CPU Load，監控CPU使用情況**  
**5.Save dashboards**  
<img width="1922" height="905" alt="image" src="https://github.com/user-attachments/assets/692c3754-abd9-4e0e-b119-ce0cc33663ba" />

**完成後圖示，該圖程式百分比數值**  
**注意1.紅框必須切換至 Code 模式 輸入，不可使用 Builder 模式，有數學公式就要改**  
**2.紅框選percent顯示%**  
<img width="1910" height="910" alt="image" src="https://github.com/user-attachments/assets/397b8c28-e3c9-4d99-99d0-0827a964d9fd" />  

---

### 初步成品  
**按下add新增更多儀表,範例目前呈現樹莓派CPU和RAM的使用率**  
<img width="1916" height="742" alt="image" src="https://github.com/user-attachments/assets/ee7771a7-dcce-4712-af68-075ae04e0fc3" />
