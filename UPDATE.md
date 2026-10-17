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
