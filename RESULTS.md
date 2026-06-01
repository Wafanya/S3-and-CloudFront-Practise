# S3 + CloudFront — результати домашки

**Студент:** Polina Vasylenko  
**GitHub:** [Wafanya](https://github.com/Wafanya)  
**Репозиторій:** [S3-and-CloudFront-Practise](https://github.com/Wafanya/S3-and-CloudFront-Practise)

---

## CloudFront URL

https://d2s5wljjyxb7gv.cloudfront.net/

## S3 bucket

`bedev-polina-vasylenko` · регіон **us-east-1**

---

## AWS (Console)

- S3 bucket, public access заблоковано
- Завантажено `index.html`, `error.html`
- CloudFront distribution + OAC (private S3 access)
- Default root object: `index.html`
- Custom error response: 404 → `/error.html`
- Invalidation `/*` після оновлень

---

## Definition of Done

| Критерій | Статус |
|----------|--------|
| HTTPS на `*.cloudfront.net` | ✅ |
| Вимоги до сторінки (див. `index.html`) | ✅ |
| `/blabla` → `error.html`, статус 404 | ✅ |
| S3 закритий для публіки | ✅ |

---

## Група 4 — Headers detective

| Заголовок | Що знайшли | Що означає |
|-----------|------------|------------|
| `x-cache` | `Hit from cloudfront` / `Miss from cloudfront` | **Hit** — з кешу edge-сервера. **Miss** — свіжа копія з S3. |
| `x-amz-cf-pop` | `WAW51-P3` | Edge-сервер CloudFront (WAW ≈ Варшава). |
| `x-amz-cf-id` | `7Sm1s4KC1ieJFoHbdU5FaqK8LyDGF1XR0RpagMbIlzaDlR2rHShybw==` | ID запиту для логів. |
| `age` | 803 | Скільки секунд об'єкт у кеші. |
| `cache-control` | `no-cache` | Клієнт має перевіряти актуальність перед використанням кешу (revalidate). |

**Спостереження:** після hard reload — `Hit`, `age` росте. Після invalidation `/*` — знову `Miss`, `age` ≈ 0.

---

## Рефлексія

**Сподобалось:** HTTPS і швидка роздача без свого сервера.

**Збентежило:** після оновлення файлу в S3 сайт показував стару версію, поки не зробила invalidation.
