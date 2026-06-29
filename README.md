# Android POS App for Home Products

A point-of-sale (POS) Android application built with Kotlin for managing home product inventory and generating sales receipts.

---

## Features

- **Admin Login** — Secure login screen to restrict access to authorized users.
- **Inventory Management** — Add, update, and delete home products with name, price, and stock quantity.
- **Billing / Cart System** — Select products from a spinner, adjust quantities, and build a customer bill in real time.
- **Stock Tracking** — Inventory stock levels update automatically as items are added to or removed from a bill.
- **PDF Receipt Generation** — Confirms a sale and saves a styled PDF e-receipt to the device's Downloads folder.
- **Local SQLite Database** — All inventory data is persisted on-device using SQLite via a custom `InventoryDatabaseHelper`.

---

## Screenshots / App Flow

```
LoginActivity
    └── MainActivity (Dashboard)
            ├── InventoryActivity  →  Add / Edit / Delete products
            └── BillingActivity   →  Build bill → Confirm → Generate PDF receipt
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Kotlin |
| UI | XML Layouts, Material Design 3, ViewBinding, DataBinding |
| Database | SQLite (via `SQLiteOpenHelper`) |
| PDF Generation | Android `PdfDocument` API |
| Min SDK | 34 (Android 14) |
| Target / Compile SDK | 36 |
| Build System | Gradle with Kotlin DSL (`.kts`) |

---

## Project Structure

```
app/src/main/
├── java/com/posapp/
│   ├── data/
│   │   ├── BillItem.kt          # Data class for a bill line item
│   │   └── InventoryItem.kt     # Data class for an inventory product
│   ├── db/
│   │   └── InventoryDatabaseHelper.kt  # SQLite CRUD operations
│   └── ui/
│       ├── LoginActivity.kt     # Entry screen with credential validation
│       ├── MainActivity.kt      # Dashboard with navigation buttons
│       ├── InventoryActivity.kt # Inventory list + add/edit/delete dialogs
│       ├── InventoryAdapter.kt  # RecyclerView adapter for inventory
│       ├── BillingActivity.kt   # Cart, stock management, PDF export
│       └── BillAdapter.kt       # RecyclerView adapter for bill items
└── res/
    ├── layout/                  # XML UI layouts for all screens
    ├── drawable/                # Vector icons and gradient backgrounds
    └── values/                  # Colors, strings, and themes
```

---

## Getting Started

### Prerequisites

- Android Studio Hedgehog or newer
- Android device or emulator running **Android 14 (API 34)** or higher
- JDK 11

### Installation

1. Clone or download this repository and unzip it.
2. Open the project root (`Android-Pos-App-For-Home-Products-main/`) in Android Studio.
3. Let Gradle sync and download dependencies.
4. Connect a device or start an emulator (API 34+).
5. Click **Run ▶** or press `Shift+F10`.

> **Note:** PDF receipts are saved to the device's public **Downloads** folder. On Android 10+, the app uses `Environment.DIRECTORY_DOWNLOADS` for storage — no extra permissions are required for this path.

---

## Default Login Credentials

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `admin` |

> ⚠️ These credentials are hardcoded for demo purposes. For production use, replace the authentication logic in `LoginActivity.kt` with a secure backend or hashed credential store.

---

## Database Schema

**Table:** `inventory`

| Column | Type | Description |
|---|---|---|
| `id` | INTEGER (PK, AUTOINCREMENT) | Unique product ID |
| `name` | TEXT | Product name |
| `price` | REAL | Unit price (in Rs.) |
| `stock` | INTEGER | Current stock quantity |

---

## Key Screens

### Login
Validates username and password. On success, navigates to the main dashboard.

### Dashboard (`MainActivity`)
Two navigation buttons: **Inventory** and **Billing**.

### Inventory Management
- View all products in a scrollable list.
- Tap the **FAB (+)** to add a new item.
- Tap **Edit** on any item to update its name, price, or stock.
- Tap **Delete** to remove an item (with confirmation dialog).

### Billing
- Select a product from the dropdown spinner (shows live stock count).
- Tap **Add to Bill** to add it to the cart.
- Use **+** / **−** buttons on each bill row to adjust quantity; stock updates in real time.
- Tap the trash icon to remove an item entirely (stock is restored).
- Tap **Confirm Bill** to review and confirm the sale, then generate a PDF receipt.

### PDF Receipt
A styled e-receipt is generated using the Android `PdfDocument` API and saved as `Bill_<timestamp>.pdf` in the Downloads folder. The receipt includes item names, quantities, unit totals, a grand total, and the date/time of sale.

---

## Dependencies

All dependencies are managed via the version catalog (`libs.versions.toml`):

- `androidx.core:core-ktx`
- `androidx.appcompat:appcompat`
- `com.google.android.material:material`
- `androidx.activity:activity`
- `androidx.constraintlayout:constraintlayout`
- `junit` (unit tests)
- `androidx.test.espresso:espresso-core` (instrumented tests)

---

## Known Limitations

- Login credentials are hardcoded (`admin` / `admin`) and not suitable for production.
- No multi-user support or role-based access.
- PDF receipts are saved locally only; no print or share functionality.
- Data is stored on-device only; no cloud sync or backup.
- Currency is fixed to **Rs.** (Pakistani/Indian Rupees).

---

## License

This project was developed as an academic/OOP course project. No license is specified — please contact the author before reuse or distribution.
