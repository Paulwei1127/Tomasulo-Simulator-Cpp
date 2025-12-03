# Tomasulo Algorithm Simulator (C++)

## 專案簡介 (Introduction)

This project is a C++ simulator for the **Tomasulo Algorithm**. It
demonstrates how **Dynamic Scheduling** and **Out-of-Order Execution**
work in computer architecture.\
這是一個使用 C++ 實作的 **Tomasulo
演算法模擬器**。這個專案主要用來模擬計算機結構中的 **動態排程 (Dynamic
Scheduling)** 與 **亂序執行 (Out-of-Order Execution)** 機制。

The simulator processes assembly instructions and visualizes the state
of **Reservation Stations (RS)**, **Register File (RF)**, and **RAT** at
each cycle.\
模擬器會讀取組語指令，並將每個 Cycle 的 **保留站 (RS)**、**暫存器 (RF)**
與 **RAT** 的狀態印出來，方便觀察資料流向。

------------------------------------------------------------------------

## 功能與特色 (Features)

-   **Out-of-Order Execution:** Instructions are executed out-of-order
    to minimize stalls.
    -   **亂序執行**：指令依序發出 (Issue)，但可亂序執行以減少等待時間。
-   **Hardware Simulation:**
    -   **ADD/SUB Unit:** 3 Reservation Stations (RS1 - RS3).
    -   **MUL/DIV Unit:** 2 Reservation Stations (RS4 - RS5).
-   **Instruction Latency (指令延遲設定):**
    -   `ADD`, `SUB`, `ADDI`: **2 cycles**
    -   `MUL`: **10 cycles**
    -   `DIV`: **40 cycles**
-   **Visualization:** Prints the detailed status of every component
    cycle-by-cycle.
    -   **狀態視覺化**：詳細列印出每個 Cycle 的硬體狀態變化。

------------------------------------------------------------------------

## 系統架構 (System Architecture)

The simulation follows these three stages (模擬器主要包含三個階段):

1.  **Issue (發指令):**
    -   Fetch instruction and allocate a Reservation Station (RS).
    -   Update **RAT** to handle data dependency (Register Renaming).
    -   取得指令並分配 RS，同時更新 RAT 表格以處理資料相依性 (解決 WAW
        hazard)。
2.  **Dispatch / Execute (執行):**
    -   Monitor operands. When all operands are ready (resolved by CDB),
        execution starts.
    -   監控運算元 (Operands)。當所有資料準備就緒 (透過 CDB
        廣播取得)，即開始執行運算。
3.  **Write Back (寫回):**
    -   Broadcast result on **Common Data Bus (CDB)**.
    -   Update waiting RS and Register File.
    -   運算完成後，將結果廣播到 CDB，更新正在等待這個結果的 RS
        以及暫存器數值。

------------------------------------------------------------------------

## 檔案結構 (File Structure)

-   `TomasuloSimu.cpp`: Main source code. (核心程式碼)
-   `input.txt`: Assembly input file. (輸入的組語測試檔)
-   `output.txt`: Execution log. (執行結果紀錄)
-   `Project_Report.pdf`: Detailed documentation and logic explanation.
    (詳細的中文結案報告與邏輯說明)

------------------------------------------------------------------------

## 如何執行 (How to Run)

### 1. Compile (編譯)

``` bash
g++ TomasuloSimu.cpp -o tomasulo
```

### 2. Prepare Input (準備輸入檔)

Ensure `input.txt` is in the same directory. Format:\
請確保目錄下有 `input.txt`，格式如下：

    ADDI F1, F2, 1
    SUB  F1, F3, F4
    DIV  F1, F2, F3
    MUL  F2, F3, F4

### 3. Run (執行)

``` bash
./tomasulo
```

------------------------------------------------------------------------

## 輸出範例 (Output Example)

The simulator will print the state of every component:\
模擬器會印出每個 Cycle 的各元件狀態：

    Cycle: 5
        _ RF __
    F1 |   -1  |
    ...

       _ RAT __
    F1 |RS1 |
    F2 |RS4 |
    ...

        _ RS _________________
    RS1 |    - |    4 |  RS3 |
    ...
