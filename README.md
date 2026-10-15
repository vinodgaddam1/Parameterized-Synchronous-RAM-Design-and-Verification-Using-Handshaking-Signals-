# Parameterized-Synchronous-RAM-Design-and-Verification-Using-Handshaking-Signals-

## 📝 1. Project Title
✅ **Synchronous Single-Port RAM (Read/Write) with Valid/Ready Handshake**

---

## 📌 2. Description
This project implements a **parameterized synchronous RAM** in Verilog that supports **read** and **write** operations using a simple **handshake protocol**:

- 🟢 `valid` (input) = request is available  
- ✅ `ready` (output) = RAM accepted/processed the request  
- ✍️ `w_r` selects operation: **Write (1)** / **Read (0)**  

All operations occur on the **rising edge of `clk`** ⏱️, and `rst` clears the RAM to zero 🔄.

---

## ⭐ 3. Key Features
- ⏱️ **Synchronous (posedge clock)**
- 🤝 **Valid/Ready handshake interface**
- ⚙️ **Fully parameterized**
  - `WIDTH` = data width (default **8-bit**)  
  - `DEPTH` = number of memory locations (default **16**)  
  - `ADDR = $clog2(DEPTH)` = auto-calculated address width  
- 🧼 **Reset clears entire memory** to `0`

---

## 🔌 4. Module Interface

### 📄 Module Declaration
```verilog
module syn_RAM (clk,rst,w_r,wdata_in,rdata_out,addr,valid,ready);
```

### 🧾 Signals
| Signal       | Dir | Width | Meaning |
|-------------|-----|-------|--------|
| `clk`       | In  | 1     | ⏱️ Clock |
| `rst`       | In  | 1     | 🔄 Reset (clears memory) |
| `valid`     | In  | 1     | 🟢 Request present |
| `ready`     | Out | 1     | ✅ Request handled |
| `w_r`       | In  | 1     | ✍️ Write=1 / 📖 Read=0 |
| `addr`      | In  | ADDR  | 📍 Memory address |
| `wdata_in`  | In  | WIDTH | 📨 Data to write |
| `rdata_out` | Out | WIDTH | 📤 Data read |

---

## 🧠 5. Functional Behavior

### 🔄 Reset Behavior
- When `rst = 1`, on the next clock edge, RAM does:
  - 🧼 `mem[i] = 0` for all locations

### 🤝 Handshake + Operation
On **posedge clk**:
- If `valid == 1` 🟢:
  - `ready = 1` ✅
  - If `w_r == 1` ✍️ → **Write**: `mem[addr] = wdata_in`
  - Else `w_r == 0` 📖 → **Read**: `rdata_out = mem[addr]`
- If `valid == 0` 🔴:
  - `ready = 0` 🚫
  - No operation performed

---

## 🧪 6. Example Transactions (Conceptual)

### ✍️ Write Cycle
1. Set: `valid=1`, `w_r=1`, `addr`, `wdata_in`
2. On rising `clk` ⏱️ → memory stores data ✅

### 📖 Read Cycle
1. Set: `valid=1`, `w_r=0`, `addr`
2. On rising `clk` ⏱️ → `rdata_out` updates with stored data 📤✅

---

## ⚙️ 7. Parameter Example
To create **32 locations** with **16-bit data**:
```verilog
syn_RAM #(
  .WIDTH(16),
  .DEPTH(32)
) u_ram (
  .clk(clk),
  .rst(rst),
  .w_r(w_r),
  .wdata_in(wdata_in),
  .rdata_out(rdata_out),
  .addr(addr),
  .valid(valid),
  .ready(ready)
);
```

---

## 📁 8. Files
- 📄 `syn_RAM.v` — main synchronous RAM module

---

If you want, I can also add ✅:
- 🧪 a complete **testbench**
- 📊 expected waveform behavior
- 🛠️ improved RTL style version (using `<=` non-blocking assignments)
