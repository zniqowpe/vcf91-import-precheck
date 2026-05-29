# VCF 9.1 部署 Pre-check 報告

> **環境：** VCF 9.1 Deploy  
> **Domain：** Management Domain  
> **檢查時間：** *(請填入實際執行時間)*

---

## 摘要

| 狀態 | 數量 |
|------|------|
| ✅ Success | 69 |
| ⚠️ Warning | 1 |
| ❌ Failure | 0 |

---

## ⚠️ 警告項目（需人工確認）

| # | 對象類型 | 檢查項目 | 問題說明 | 建議處置 |
|---|----------|----------|----------|----------|
| 1 | Cluster | ESXi 升級政策不符 | 叢集的「Evacuate Offline VMs」升級政策在 vCenter 中設為 `false`，與 SDDC Manager 預設值 `true` 不符 | 在「ESX Configure Update」流程中，於「**Upgrade Options**」頁面確認並套用所需的升級政策 |

---

## ✅ 成功項目

### 1. 全域 vSphere

| # | 檢查項目 | 結果 |
|---|----------|------|
| 1 | vCenter 數量驗證 | ✅ 符合，偵測到 1 台 vCenter |

---

### 2. vCenter / SDDC Manager

#### 2.1 基礎連線與時間同步

| # | 檢查項目 | 結果 |
|---|----------|------|
| 1 | vCenter 與 SDDC Manager 時間差 | ✅ 時間差 2 秒，在允許上限 45 秒內 |
| 2 | SDDC Manager DNS 設定 | ✅ DNS 已正確設定 |
| 3 | SDDC Manager NTP 設定 | ✅ NTP 已正確設定 |
| 4 | vCenter NTP 可達性 | ✅ NTP 已設定且可連線 |
| 5 | vCenter 連線延遲 | ✅ 延遲在允許範圍內（上限：110 ms） |
| 6 | Lookup Service 狀態 | ✅ 服務已啟動（STARTED） |
| 7 | vpxd 服務狀態 | ✅ 服務已啟動（STARTED） |

#### 2.2 版本與相容性

| # | 檢查項目 | 版本需求 | 結果 |
|---|----------|----------|------|
| 8 | vCenter 最低版本需求 | **CONVERT 模式：≥ 8.0.3**<br>**IMPORT 模式：≥ 8.0.1.00000-21815093** | ✅ 符合最低版本要求 |
| 9 | vCenter 與 NSX 版本相容性 | vCenter：9.1.0.0.x<br>NSX：9.1.0.0.x（須互相相容） | ✅ 兩者版本相容 |
| 10 | ESXi / vCenter / NSX 與 VCF 版本相容性 | 三者版本須均與 VCF 相容 | ✅ 全部相容 |
| 11 | vCenter 更新狀態 | Appliance 無待套用更新 | ✅ 狀態正常 |

#### 2.3 網路與 DNS

| # | 檢查項目 | 結果 |
|---|----------|------|
| 12 | vCenter IP 位址族系 | ✅ 與 SDDC Manager IP 位址族系相容 |
| 13 | vCenter FQDN 可解析性 | ✅ FQDN 可正常解析 |
| 14 | vCenter FQDN DNS 別名 | ✅ 無 DNS 別名（不得有 DNS alias） |
| 15 | vCenter VM 網路連線（管理流量 DVPortGroup） | ✅ vCenter VM 已連接至管理流量類型的 DVPortGroup |

#### 2.4 服務與功能狀態

| # | 檢查項目 | 結果 |
|---|----------|------|
| 16 | SSH 登入啟用狀態 | ✅ SSH 登入已啟用 |
| 17 | SDDC Manager 延伸套件（Extension）衝突 | ✅ 無 SDDC Manager 延伸套件衝突 |
| 18 | 舊版 VxRail 延伸套件 | ✅ 無不支援的 Legacy VxRail 延伸套件 |
| 19 | VxRail Next 延伸套件 | ✅ 符合規範 |
| 20 | NSX Manager 與 vCenter 重複連線 | ✅ vCenter 未與 NSX Manager 重複連線 |
| 21 | vCenter HA 停用確認 | ✅ vCenter HA 已停用（部署期間須停用） |
| 22 | vCenter ELM 模式確認 | ✅ 非 ELM 模式（只有 1 個 vCenter 節點） |
| 23 | CEIP（客戶體驗改善計畫）設定 | ✅ CEIP 已正確設定 |
| 24 | vCenter root 密碼驗證 | ✅ root 帳號可正常存取 |

#### 2.5 規模限制

| # | 檢查項目 | 限制條件 | 結果 |
|---|----------|----------|------|
| 25 | NSX Manager 規格（主機數量） | 超過 2,500 台主機須使用 XLarge 規格 | ✅ 目前主機數量符合 MEDIUM 規格 |
| 26 | VCF 主機總數規模限制 | 上限 20,000 台 | ✅ 未超過限制 |
| 27 | Workload Domain 數量限制 | 上限 40 個 | ✅ 未超過限制 |

#### 2.6 其他

| # | 檢查項目 | 結果 |
|---|----------|------|
| 28 | 叢集符合性 | ✅ 偵測到符合規範的叢集 |
| 29 | vCenter VM 共置確認 | ✅ VM 已共置於 vCenter 或由 Management Domain 管理 |
| 30 | Cross-vCenter vSAN 資料存放區來源 | ✅ 無跨 vCenter vSAN 資料存放區來源 |

---

### 3. Cluster

| # | 檢查項目 | 版本需求／條件 | 結果 |
|---|----------|----------------|------|
| 1 | 共用資料存放區驗證 | 叢集所有主機須有有效共用資料存放區 | ✅ 通過 |
| 2 | ESXi 主機數量 | vSAN 叢集最少 3 台；非 vSAN 最少 2 台 | ✅ 共 3 台主機，符合要求 |
| 3 | DRS 全自動模式 | 多主機叢集須啟用 DRS 且設為 Fully Automated | ✅ 已啟用 |
| 4 | vSAN VMkernel 介面卡 | 每台主機須有獨立 vSAN 流量 VMkernel 介面卡 | ✅ 每台主機均符合 |
| 5 | 管理流量 IP 族系一致性 | 叢集內所有主機管理流量 IP 族系須一致（IPv4-only 或 Dual Stack） | ✅ 一致 |
| 6 | vMotion 流量 IP 族系一致性 | 叢集內所有主機 vMotion 流量 IP 族系須一致（IPv4-only 或 IPv6-only） | ✅ 一致 |
| 7 | vLCM Image-based 管理確認 | 叢集須採用 vSphere Lifecycle Manager image-based 管理 | ✅ 已採用 |
| 8 | DVS 使用確認 | 叢集須至少使用一個 Distributed Virtual Switch | ✅ 已使用 |

---

### 4. DVS（Distributed Virtual Switch）

| # | 檢查項目 | 版本需求 | 結果 |
|---|----------|----------|------|
| 1 | DVS 版本相容性（NSX 需求） | **DVS 最低版本：≥ 7.0.3** | ✅ 版本符合 NSX 設定需求 |

---

### 5. ESXi 主機（共 3 台）

以下檢查項目適用於所有主機，三台均通過。

#### 5.1 版本需求

| # | 檢查項目 | 版本需求 | 結果 |
|---|----------|----------|------|
| 1 | 主機最低版本需求 | **CONVERT 模式（叢集含 vCenter VM）：≥ 8.0.1**<br>**IMPORT 模式：≥ 8.0.1-21813344** | ✅ 三台主機均符合 |

#### 5.2 網路與憑證

| # | 檢查項目 | 結果 |
|---|----------|------|
| 2 | 主機憑證 CN / SANs 有效性 | ✅ 三台主機憑證均有效 |
| 3 | 主機名稱為 FQDN 格式 | ✅ 三台主機均符合 |
| 4 | VMkernel 介面卡流量類型唯一性 | ✅ 每台主機各流量類型各自對應唯一 VMkernel 介面卡 |

#### 5.3 服務與狀態

| # | 檢查項目 | 結果 |
|---|----------|------|
| 5 | NTP 服務運作狀態 | ✅ 三台主機 NTP 服務均運作中 |
| 6 | 主機模式（非 Stateless） | ✅ 三台主機均為 Stateful（非 PXE boot / stateless） |
| 7 | 主機與 vCenter 連線狀態 | ✅ 三台主機均為 Active |
| 8 | VM 未固定至主機（VM-Host Affinity） | ✅ 三台主機均無 VM Host Affinity 限制 |

---

## 結論與後續行動

| 項目 | 狀態 | 行動 |
|------|------|------|
| 整體 Pre-check | ⚠️ 有警告 | 處理下方警告後可繼續 |
| **叢集升級政策（Evacuate Offline VMs）** | ⚠️ 需確認 | 在「ESX Configure Update」→「**Upgrade Options**」頁面中，確認並設定所需的升級政策（預設應為 `true`） |

> 所有 Failure 項目：**無**  
> 所有 Warning 項目已處理後，即可繼續進行 VCF 9.1 部署作業。

---

*本報告由 VCF Pre-check Validation 結果自動整理，已完成去識別化處理。*
