---
name: stock-expert-email
description: Run a full US and Taiwan stock market analysis and send the result as a rich Traditional Chinese HTML email via Gmail SMTP (no Chrome required). Use this skill when the user says "send me the stock report by email", "stock expert email", "幫我把股票分析寄到信箱", or any variation of wanting stock analysis delivered to their email.
---

# Stock Expert Email

Fetch the latest US and Taiwan stock market data, build a full Traditional Chinese HTML analysis report, and send it to jimmy6409444@gmail.com via Gmail SMTP — no Chrome required.

---

## Step 1 — Fetch latest data (run all searches in parallel)

- `US stock market S&P 500 Nasdaq latest news today [current date]`
- `台股 加權指數 台積電 最新 [current date]`
- `Fed interest rate CPI inflation latest [current month year]`
- `TSMC TSM earnings revenue outlook latest [current month year]`
- `NVIDIA MSFT AI capex spending latest news [current month year]`

Extract: Fed funds rate, US CPI, S&P 500 & Nasdaq levels, TAIEX level, TSMC stock price + revenue/net income YoY, AI capex totals, 3–5 major market-moving headlines.

---

## Step 2 — Build HTML email body (繁體中文, inline styles only)

Compose a complete HTML string in `$htmlBody` with ALL text in Traditional Chinese. Use only inline CSS styles (no `<style>` blocks). Include these sections:

1. **Header** — gradient blue background, date + "美股台股分析報告" title + subtitle
2. **重大最新事件** — blue left-border strip, 3–5 bullet headlines
3. **關鍵總體經濟指標** — 8 metric cards in a 4-column table:
   聯準會利率, 美國CPI年增, 那斯達克, 加權指數, 台積電股價, 台積電淨利YoY, TSMC營收YoY, AI資本支出
4. **市場訊號** — 2-column table side by side:
   - 🇺🇸 美股：AI資本支出, 通膨, 聯準會政策, 地緣政治
   - 🇹🇼 台股：加權指數趨勢, 台積電營收, AI需求, 集中度風險
   - Badge colors: 看多 bg #d1fae5 text #065f46 | 偏高/鷹派 bg #fef3c7 text #92400e | 風險 bg #fee2e2 text #991b1b
5. **美股精選** — 6 cards (2-col): NVDA, MSFT, AVGO, NOW, VST, AMD
   Each card: ticker | sector, company name, 2-sentence thesis, rating (▲重點關注 #059669 / —觀察 #b45309 / ▼謹慎 #dc2626)
6. **台股精選** — 6 cards (2-col): 台積電(2330), 聯發科(2454), 鴻海(2317), 日月光(3711), 台達電(2308), 聯電(2303)
7. **主要風險** — 3 amber cards: 貨幣政策, 市場集中度, AI週期
8. **下半年展望** — 3 border cards: AI資本支出, 利率環境, 地緣政治
9. **免責聲明** — gray footer: 本報告僅供參考，不構成任何投資建議。

---

## Step 3 — Send via .NET SmtpClient with UTF-8

**CRITICAL: Use `System.Net.Mail.SmtpClient` — NOT `Send-MailMessage`.**
`Send-MailMessage` uses Windows-1252 encoding and will turn all Chinese characters into question marks.

```powershell
$cfg = Get-Content "C:\Users\Jimmy\.claude\email-config\gmail.json" | ConvertFrom-Json
$pass = ConvertTo-SecureString $cfg.appPassword -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential($cfg.from, $pass)
$today = Get-Date -Format 'yyyy/MM/dd HH:mm'
$subject = "📊 美股台股分析報告 — $today"

$msg = New-Object System.Net.Mail.MailMessage
$msg.From = $cfg.from
$msg.To.Add($cfg.to)
$msg.Subject = $subject
$msg.Body = $htmlBody
$msg.IsBodyHtml = $true
$msg.SubjectEncoding = [System.Text.Encoding]::UTF8
$msg.BodyEncoding = [System.Text.Encoding]::UTF8

$smtp = New-Object System.Net.Mail.SmtpClient($cfg.smtpServer, $cfg.smtpPort)
$smtp.EnableSsl = $true
$smtp.Credentials = $cred
$smtp.Send($msg)
$smtp.Dispose()
$msg.Dispose()
Write-Host "Email sent: $subject"
```

Config file: `C:\Users\Jimmy\.claude\email-config\gmail.json`
Reusable script: `C:\Users\Jimmy\.claude\email-config\Send-StockEmail.ps1` (already uses UTF-8)

---

## Success criteria
- Email delivered to jimmy6409444@gmail.com
- Subject contains today's date
- All Chinese characters display correctly (no question marks)
- All 9 sections present in Traditional Chinese with latest search data

---

## Trigger phrases
- "send me the stock report by email"
- "stock expert email"
- "幫我把股票分析寄到信箱"
- "email me the stock analysis"
- "寄股票報告給我"
