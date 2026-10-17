# Güncelleme / Update

## Not ekleme
`data/notes.json` dosyasına yeni bir nesne ekleyin (sıra önemli değil; sayfa tarihe göre en yeniyi üstte gösterir):
```json
{ "date": "2026-10-18", "title": "Günlük özet", "items": ["Madde 1", "Madde 2"] }
```
Kontrol: `python3 -m json.tool data/notes.json >/dev/null && echo OK`

Yerel önizleme: `cd /workspace/nasdaq-panel && python3 -m http.server 8000` → http://localhost:8000

## Yayınlama (GitHub Pages)
İlk kurulum (bir kez, `gh auth login` sonrası):
```bash
cd /workspace/nasdaq-panel
git init -b main && git add . && git commit -m "Initial"
gh repo create EnginDenizTangut/nasdaq-panel --public --source=. --push
gh api -X POST repos/EnginDenizTangut/nasdaq-panel/pages -f "source[branch]=main" -f "source[path]=/"
```
Sonraki her not:
```bash
cd /workspace/nasdaq-panel && git add data/notes.json && git commit -m "Not: $(date +%F)" && git push
```
URL: https://engindeniztangut.github.io/nasdaq-panel/ (1–2 dk içinde güncellenir)

## Pozisyon / işlem / fiyat ekleme
- **Pozisyon** (`data/positions.json`, açık pozisyonlar): `{"ticker":"AAPL","qty":0.1,"buyPrice":336.5,"date":"2026-10-20","audUsd":0.65}` — kapanınca bu kaydı silin.
- **İşlem** (`data/trades.json`): alış `{"date":"2026-10-20","ticker":"AAPL","side":"buy","qty":0.1,"price":336.5,"resultAud":null,"note":""}`; satış aynı biçimde `"side":"sell"` ve gerçekleşen `"resultAud": 4.20` (masraflar dahil). Toplam getiri = resultAud toplamı / AU$100.
- **Fiyat** (`data/prices.json`): `"prices": {"AAPL": 337.1, ...}` ve `"updated": "2026-10-20 07:00 Brisbane"`. Bilinmeyen fiyatı `null` bırakın (uydurmayın).
- **Alarm** (`data/watch.json`): `lastClose` ve `lastCloseDate` güncelleyin.

Not: Yahoo/Stooq tarayıcıdan CORS nedeniyle okunamıyor; fiyatlar bu yüzden elle güncellenir.

Yayınla: `python3 -c "import json,glob;[json.load(open(f)) for f in glob.glob('data/*.json')]" && git add -A && git commit -m "Veri güncelleme" && git push`
