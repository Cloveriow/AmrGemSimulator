# AMR SECS/GEM Navigation Simulator

基於 **C# / Blazor** 開發的 AMR（Autonomous Mobile Robot）導航與 SECS/GEM 控制模擬系統，模擬半導體自動化設備中 AMR 的感知、路徑規劃、導航控制與設備通訊狀態。

## Features

* **SEMI E30 SECS/GEM**

  * GEM Control State / Processing State
  * CEID Event 模擬
  * S2F41 Remote Command 模擬
  * S6F11 Event Report 模擬
  * Alarm / Recovery State 管理

* **AMR Navigation**

  * 360° LiDAR Ray Casting
  * 局部感知地圖（Known Grid）
  * ESDF（Euclidean Signed Distance Field）計算
  * 基於 ESDF Cost 的路徑規劃
  * 8-direction A* Search
  * Path Hysteresis，降低路徑左右反覆切換
  * Loop Detection 與 Deadlock Detection
  * Reverse / Recovery 機制

* **Simulation & Visualization**

  * 隨機生成非規則障礙物與牆體
  * 即時 LiDAR 感知視覺化
  * 全域 ESDF / 局部 ESDF 顯示
  * AMR 位置、速度與航向角監控
  * 即時 GEM 狀態與 CEID 顯示
  * 支援手動建立、刪除障礙物及修改起終點

## Navigation Pipeline

```text
Environment
    ↓
LiDAR Ray Casting
    ↓
Known Grid / Local Map
    ↓
ESDF Cost Map
    ↓
A* Path Planning
    ↓
Path Hysteresis / Loop Detection
    ↓
Target Point Selection
    ↓
Velocity & Angular Velocity Control
    ↓
AMR Simulation
```

## Technical Highlights

### ESDF-based Path Planning

使用距離轉換計算障礙物距離，將障礙物距離轉換為路徑成本，使 AMR 在可通行區域中優先選擇具有安全距離的路徑。

同時使用預先配置的陣列 Pool 儲存 A* 搜尋資料，降低導航迴圈中的記憶體配置。

### Dynamic Replanning

導航過程持續更新 LiDAR 感知結果並重新規劃路徑，使 AMR 能根據局部環境變化調整導航方向。

### Deadlock Recovery

針對狹窄通道與反覆繞行情況加入：

* ESDF Cost Saturation
* Path Hysteresis
* Recent Position History
* Loop Penalty
* Reverse Escape
* Deadlock Detection

透過狀態機切換 Normal / Recovery / Paused 等處理狀態。

## Technology

* **C#**
* **Blazor / Razor**
* **SEMI E30 SECS/GEM**
* **A* Path Planning**
* **ESDF**
* **LiDAR Simulation**
* **AMR Navigation**
* **State Machine**
* **SVG Visualization**

## Project Purpose

本專案用於模擬半導體自動化環境中的 AMR 控制流程，將 **機器人導航演算法與 SECS/GEM 設備通訊概念整合**，並透過即時視覺化呈現感知、路徑規劃與設備狀態。

> Note: 本專案為 SECS/GEM 與 AMR Navigation 的軟體模擬，並非實際 SEMI E30 通訊設備或 ROS 2 Nav2 的完整實作。
