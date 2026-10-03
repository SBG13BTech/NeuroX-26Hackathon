# 🤖 Funobotz Product-Aligned Learning Assistance Chatbot Widget
### *Built with FastAPI, MySQL (via `pymysql`), Google Gemini API, and Vanilla HTML/CSS/JS*

> **Hackathon Solution**: A plug-and-play, embeddable chatbot widget for the Funobotz Store and learning ecosystem, strictly grounded in official paper robotics products with fine-grained customer entitlement gating, live MySQL data persistence, and Gemini AI reasoning.

---

## 🔑 Where to Add Your Credentials

Open the **[`.env`](file:///d:/Sanjay%20Balamurugan/NeuroX%20Hackathon/.env)** file in the root directory:

```env
# 1. Google Gemini API Key (Get from https://aistudio.google.com/app/apikey)
GEMINI_API_KEY=your_gemini_api_key_here

# 2. MySQL Database Credentials
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password_here
MYSQL_DB=funobotz_db
```

> 💡 **Auto-Setup & Fallback**: Once your password is added to `.env`, `database.py` will automatically connect to MySQL via `pymysql`, create the `funobotz_db` database, generate the 3 tables, and seed the customer and product records. If MySQL is not running or credentials are empty, it gracefully runs with an embedded SQLite database so you can test immediately without interruption!

---

## 🌟 Key System Capabilities

### 1. MySQL Database Architecture (`pymysql`)
* **`products`**: Stores official product knowledge (Petalo, Quacky, Tolly, Tiko, Lumina, and new Admin-added kits) with learning contexts, concepts, build steps, circuit components, and troubleshooting guides.
* **`customers`**: Stores test accounts (`FZ-HACK-001` through `FZ-HACK-005`).
* **`customer_purchases`**: Maps many-to-many relationship of customer purchases.

### 2. Google Gemini API System Instructions & Guardrails
* **Owned Products**: Delivers deep, step-by-step building assistance, origami creases, circuit wiring, mechanical dynamics, and troubleshooting.
* **Unowned Products**: Politely explains that the kit is not in their current collection, provides a high-level summary and features, and suggests checking it out in the Funobotz Store.
* **Strict Out-of-Context Guardrail**: If the user inputs vague, nonsensical, or unrelated prompts (e.g., *"what is day after tomorrow yesterday"*, random math, weather), Gemini strictly responds with:
  > *"Looks like the prompt is out of context... kindly try some other prompt."*

### 3. Admin Feature (Dynamic Product Management)
* Admins can click **"⚙️ Admin: + Add Product"** on the store page or call `POST /api/admin/products`.
* Products are saved directly to MySQL and **immediately recognized by Gemini AI** without server restarts.

### 4. Zero-Dependency Embeddable Widget (HTML/CSS/JS)
* Scoped, responsive floating chat interface matching Funobotz brand's yellow, white, and clean modern aesthetic.
* Embeds into any store page via a single `<script>` tag:
  ```html
  <script 
    src="http://localhost:8000/widget/funobotz-widget.js" 
    data-api="http://localhost:8000" 
    data-customer-id="FZ-HACK-001" 
    data-funobotz-widget>
  </script>
  ```

---

## 🧪 Customer Dataset Test Matrix

| Customer ID | Name | Registered Kits | Access Rules Enforced |
| :--- | :--- | :--- | :--- |
| `FZ-HACK-001` | Alex Chen | **Petalo, Quacky** | Full access for Petalo & Quacky; Boundary notice for Tiko/Tolly |
| `FZ-HACK-002` | Maya Sharma | **Tiko** | Full access for Tiko; Boundary notice for Petalo/Quacky/Tolly |
| `FZ-HACK-003` | Leo Patel | **Tolly, Petalo** | Full access for Tolly & Petalo; Boundary notice for Quacky/Tiko |
| `FZ-HACK-004` | Samira Khan | **Quacky, Tiko, Tolly** | Full access for Quacky, Tiko, Tolly; Boundary notice for Petalo |
| `FZ-HACK-005` | Jordan Taylor | *None (0 Kits)* | Friendly onboarding & store navigation overview |

---

## 🚀 How to Run

```bash
# 1. Start the FastAPI server
python -m uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --reload

# 2. Run all automated tests
python -m pytest backend/tests/ -v
```

* **Interactive Store Demo**: [http://localhost:8000/store](http://localhost:8000/store)
* **API Documentation**: [http://localhost:8000/docs](http://localhost:8000/docs)
