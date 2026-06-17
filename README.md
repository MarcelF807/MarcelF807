- 👋 Hi, I’m @MarcelF807
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
MarcelF807/MarcelF807 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

import requests
import csv
from datetime import datetime

# Deine Coinbase API-Daten
API_KEY = "HIER_DEIN_API_KEY"
API_SECRET = "HIER_DEIN_API_SECRET"

# Coinbase API-Endpunkt
url = "https://api.exchange.coinbase.com"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "CB-VERSION": "2021-08-01"
}

# CSV-Datei vorbereiten
csv_file = "coinbase_report.csv"
headers_csv = ["Zeitstempel", "Währung", "Balance"]

# CSV-Datei erstellen (nur beim ersten Lauf)
with open(csv_file, mode="w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerow(headers_csv)

# API-Abfrage
try:
    r = requests.get(url, headers=headers, timeout=10)
    data = r.json()

    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    for account in data["data"]:
        currency = account["currency"]["code"]
        balance = account["balance"]["amount"]

        # Nur speichern, wenn Guthaben vorhanden
        if float(balance) > 0:
            with open(csv_file, mode="a", newline="", encoding="utf-8") as file:
                writer = csv.writer(file)
                writer.writerow([timestamp, currency, balance])

    print(f"\n✅ Coinbase-Bericht gespeichert in: {csv_file}")

except Exception as e:
    print(f"\n⚠️ Fehler beim Abrufen der Coinbase-Daten: {e}")
