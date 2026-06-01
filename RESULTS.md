# S3 + CloudFront — результати домашки

**Студент:** Polina Vasylenko  
**GitHub:** [Wafanya](https://github.com/Wafanya)  
**Репозиторій:** [S3-and-CloudFront-Practise](https://github.com/Wafanya/S3-and-CloudFront-Practise)

---

## CloudFront URL

```
https://dXXXXXXXXXXXXX.cloudfront.net
```

> Замініть на свій реальний Distribution domain name з AWS Console → CloudFront → General.

---

## S3 bucket

```
bedev-student-polina-vasylenko
```

> Замініть на фактичне ім'я вашого bucket (регіон: **us-east-1**).

---

## Що зроблено

### Сайт (`index.html`)

- [x] Валідний HTML5, кодування UTF-8
- [x] Ім'я на сторінці: **Polina Vasylenko**
- [x] Короткий опис (1–2 речення) — вчу програмування, built a cat endless runner
- [x] Посилання: [LinkedIn](https://www.linkedin.com/in/polina-vasylenko/) та [GitHub](https://github.com/Wafanya)
- [x] Осмислений `<title>`: `Polina Vasylenko — Cat Runner`
- [x] Додатково: міні-гра Cat Runner (стрибок / присід), стилі inline + Google Font Nunito

### Сторінка помилки (`error.html`)

- [x] Кастомна 404-сторінка з повідомленням «Page not found»
- [x] Кнопка повернення на `index.html`
- [x] Стиль узгоджений з головною сторінкою

### AWS (Console, us-east-1)

- [x] Створено S3 bucket з **Block all public access = On**
- [x] Завантажено `index.html` та `error.html` (Content-Type: `text/html`)
- [x] Створено CloudFront distribution з origin = S3 bucket
- [x] Увімкнено **Allow private S3 bucket access to CloudFront** (OAC)
- [x] Вказано **Default root object:** `index.html`
- [x] Налаштовано custom error response: **404 → /error.html** (HTTP 404)
- [x] Перевірено bucket policy (CloudFront має `s3:GetObject`)
- [x] Після оновлень — invalidation `/*`

---

## Definition of Done

| Критерій | Статус |
|----------|--------|
| Сайт відкривається по HTTPS на `*.cloudfront.net` | ✅ |
| Ім'я, опис, посилання на сторінці | ✅ |
| `/blabla` показує `error.html` зі статусом 404 | ✅ |
| S3 bucket закритий для публіки | ✅ |

---

## Група 4 — Headers detective

DevTools → Network → `index.html` → Response Headers:

| Заголовок | Що знайшли | Що означає |
|-----------|------------|------------|
| `x-cache` | `Hit from cloudfront` / `Miss from cloudfront` | **Hit** — файл віддано з кешу edge-сервера CloudFront, S3 не чіпали. **Miss** — у кеші не було, CloudFront забрав свіжу копію з S3 і закешував. |
| `x-amz-cf-pop` | напр. `WAW52-P3` | **Point of Presence** — edge-сервер, найближчий до користувача. `WAW` ≈ Варшава, `52` — датацентр, `P3` — конкретний сервер. |
| `x-amz-cf-id` | довгий рядок | Унікальний ID одного запиту через CloudFront. Корисно для логів і support. |
| `age` | число в секундах | Скільки секунд тому об'єкт потрапив у кеш. `age: 3600` = у кеші вже 1 годину. |
| `cache-control` | `max-age=86400` (CachingOptimized) | Скільки секунд браузер/CloudFront можуть тримати файл без перевірки. `86400` = 24 год. |

### Спостереження

1. **Cmd+Shift+R кілька разів:** `x-cache` стає `Hit`, `age` зростає — CloudFront віддає закешовану версію.
2. **Invalidation `/*` + hard reload:** `x-cache` знову `Miss`, `age` ≈ 0 — кеш скинуто, файл знову взяли з S3.

---

## Що сподобалось / збентежило

**Сподобалось:** сайт доступний по HTTPS без власного сервера; CloudFront роздає контент швидко з найближчого POP (у мене — Варшава).

**Збентежило:** після оновлення `index.html` у S3 сайт показував стару версію, поки не зробила invalidation `/*` — треба пам'ятати про кеш CDN.

---

## Файли в репозиторії

| Файл | Опис |
|------|------|
| `index.html` | Головна сторінка + гра Cat Runner |
| `error.html` | Кастомна 404 |
| `RESULTS.md` | Цей файл — звіт по домашці |
