---
name: stock-expert-email
description: Run a full US and Taiwan stock market analysis and send the result as a rich Traditional Chinese HTML email via Gmail SMTP (no Chrome required). Use this skill when the user says "send me the stock report by email", "stock expert email", "撟急??蟡典????唬縑蝞?, or any variation of wanting stock analysis delivered to their email.
---

# Stock Expert Email

Fetch the latest US and Taiwan stock market data, build a full Traditional Chinese HTML analysis report, and send it to jimmy6409444@gmail.com, applerain152@gmail.com via Gmail SMTP ??no Chrome required.

---

## Step 1 ??Fetch latest data (run all searches in parallel)

- `US stock market S&P 500 Nasdaq latest news today [current date]`
- `?啗 ??? ?啁??????[current date]`
- `Fed interest rate CPI inflation latest [current month year]`
- `TSMC TSM earnings revenue outlook latest [current month year]`
- `NVIDIA MSFT AI capex spending latest news [current month year]`

Extract: Fed funds rate, US CPI, S&P 500 & Nasdaq levels, TAIEX level, TSMC stock price + revenue/net income YoY, AI capex totals, 3?? major market-moving headlines.

---

## Step 2 ??Build HTML email body (蝜?銝剜?, inline styles only)

Compose a complete HTML string in `$htmlBody` with ALL text in Traditional Chinese. Use only inline CSS styles (no `<style>` blocks). Include these sections:

1. **Header** ??gradient blue background, date + "蝢?啗???勗?" title + subtitle
2. **?之??唬?隞?* ??blue left-border strip, 3?? bullet headlines
3. **?蝮賡?蝬???** ??8 metric cards in a 4-column table:
   ?舀???? 蝢?CPI撟游?, ????, ???, ?啁??餉?? ?啁??餅楊?坡oY, TSMC?YoY, AI鞈?臬
4. **撣閮?** ??2-column table side by side:
   - ?? 蝢嚗I鞈?臬, ?, ?舀??蝑? ?啁楠?踵祥
   - ?? ?啗嚗?甈??貉隅?? ?啁??餌??? AI?瘙? ?葉摨阡◢??   - Badge colors: ?? bg #d1fae5 text #065f46 | ??/曋寞晷 bg #fef3c7 text #92400e | 憸券 bg #fee2e2 text #991b1b
5. **蝢蝎暸** ??6 cards (2-col): NVDA, MSFT, AVGO, NOW, VST, AMD
   Each card: ticker | sector, company name, 2-sentence thesis, rating (?脤?暺?瘜?#059669 / ??撖?#b45309 / ?潸牲??#dc2626)
6. **?啗蝎暸** ??6 cards (2-col): ?啁???2330), ?舐蝘?2454), 暾餅絲(2317), ?交???3711), ?圈???2308), ?舫(2303)
7. **銝餉?憸券** ??3 amber cards: 鞎典馳?輻?, 撣?葉摨? AI?望?
8. **銝?撟游???* ??3 border cards: AI鞈?臬, ?拍??啣?, ?啁楠?踵祥
9. **?痊?脫?** ??gray footer: ?砍??靘???銝??遙雿?鞈遣霅啜?
---

## Step 3 ??Send via .NET SmtpClient with UTF-8

**CRITICAL: Use `System.Net.Mail.SmtpClient` ??NOT `Send-MailMessage`.**
`Send-MailMessage` uses Windows-1252 encoding and will turn all Chinese characters into question marks.

```powershell
$cfg = Get-Content "C:\Users\Jimmy\.claude\email-config\gmail.json" | ConvertFrom-Json
$pass = ConvertTo-SecureString $cfg.appPassword -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential($cfg.from, $pass)
$today = Get-Date -Format 'yyyy/MM/dd HH:mm'
$subject = "?? 蝢?啗???勗? ??$today"

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
- Email delivered to jimmy6409444@gmail.com, applerain152@gmail.com
- Subject contains today's date
- All Chinese characters display correctly (no question marks)
- All 9 sections present in Traditional Chinese with latest search data

---

## Trigger phrases
- "send me the stock report by email"
- "stock expert email"
- "撟急??蟡典????唬縑蝞?
- "email me the stock analysis"
- "撖蟡典?策??

