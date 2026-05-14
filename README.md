# 💸 MoneyLaundry — FiveM ESX Script

A simple and clean money laundering script for FiveM servers running the **ESX** framework. Players can convert black money into clean cash through an NPC interaction, with a configurable wash time and tax rate.

---

## 📋 Features

- NPC-based interaction with dialogue menu
- Configurable wash time (progress shown in real-time)
- Configurable tax/cut on laundered money
- Server-side validation (distance check, balance check)
- Black money is deducted before the wash begins — no exploits

---

## 🔧 Dependencies

- [es_extended (ESX)](https://github.com/esx-framework/esx_core)
- textui *(or compatible export: `exports.textui:Draw3DUI`)*
- NpcDialogue *(export: `exports['NpcDialogue']:StartNPCCameraMenu`)*
- dialog *(export: `exports['dialog']:Create`)*

> ⚠️ Make sure all dependencies are started **before** this resource in your `server.cfg`.

---

## 📦 Installation

1. Download or clone this repository into your FiveM `resources` folder:
   ```
   resources/
   └── moneylaundry/
       ├── client.lua
       ├── server.lua
       ├── config.lua
       └── fxmanifest.lua
   ```

2. Add the resource to your `server.cfg`:
   ```
   ensure fLaundry
   ```

3. Configure the script to your liking in `config.lua` (see below).

---

## ⚙️ Configuration

All settings are found in `config.lua`:

```lua
Config = {}

Config.NPC = {
    coords  = vector3(1116.66, -3193.36, -40.39), -- NPC spawn location
    heading = 225.94,                              -- NPC facing direction
    model   = "s_m_y_factory_01",                  -- NPC ped model
}

Config.Tax      = 0.30   -- Tax rate (0.30 = 30% cut taken)
Config.WashTime = 180    -- Time in seconds to complete a wash
```

| Option | Type | Description |
|---|---|---|
| `Config.NPC.coords` | `vector3` | World coordinates where the NPC spawns |
| `Config.NPC.heading` | `float` | Direction the NPC faces |
| `Config.NPC.model` | `string` | Ped model hash name |
| `Config.Tax` | `float` | Percentage deducted from the laundered amount (e.g. `0.30` = 30%) |
| `Config.WashTime` | `integer` | Duration of the laundry process in seconds |

---

## 🎮 How It Works

1. Player walks up to the NPC (within **2.5 units**).
2. A `[E] Money Laundry` prompt appears via textui.
3. Player presses **E** to open the NPC dialogue.
4. Player selects **"I want to wash money"** and enters an amount.
5. The server checks:
   - The player is within range of the NPC.
   - The player has enough **black money**.
6. Black money is deducted immediately.
7. A progress indicator counts down the wash timer.
8. On completion, the player receives **clean cash** minus the configured tax:
   ```
   cash = amount × (1 - Config.Tax)
   ```

---

## 🗂️ File Structure

```
fLaundry/
├── client.lua     — NPC creation, interaction, wash progress loop
├── server.lua     — Callbacks, validation, money transactions
├── config.lua     — NPC coords, tax rate, wash time
└── fxmanifest.lua — Resource manifest (you must create this)
```

---

## 🛡️ Security Notes

- The amount is validated **server-side** before any money is removed.
- Distance to the NPC is verified on the server using `GetEntityCoords` — clients cannot spoof proximity.
- `CompleteLaundry` recalculates the cash amount server-side using `Config.Tax`, so clients cannot manipulate the payout.

---

## 📜 License

This project is open source. Feel free to use, modify, and redistribute. A credit is appreciated but not required.
