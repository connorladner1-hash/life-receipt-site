# life-receipt.com

Static marketing + support site for LifeReceipt (Paper Street Holdings LLC).
No build step. Plain HTML/CSS, deployed on Vercel.

## Pages

| Page | URL | Used for |
|---|---|---|
| `index.html` | `https://life-receipt.com/` | **App Store Marketing URL** |
| `support.html` | `https://life-receipt.com/support` | **App Store Support URL** |
| `privacy.html` | `https://life-receipt.com/privacy` | **App Store Privacy Policy URL** (required) |
| `terms.html` | `https://life-receipt.com/terms` | EULA — must be linked on the paywall |

`cleanUrls` is on in `vercel.json`, so `/support` works without `.html`.

## 1. Push to GitHub (PowerShell)

```powershell
cd C:\path\to\life-receipt
git init
git add .
git commit -m "LifeReceipt site: marketing, support, privacy, terms"
git branch -M main
git remote add origin https://github.com/connorladner1-hash/life-receipt-site.git
git push -u origin main
```

Create the empty repo on GitHub first (no README, no .gitignore) so the push isn't rejected.

## 2. Deploy on Vercel

1. vercel.com → **Add New → Project** → import `life-receipt-site`.
2. Framework preset: **Other**. Build command: blank. Output directory: blank.
3. Deploy. You'll get a `*.vercel.app` URL — check all four pages load.

## 3. Point life-receipt.com at it — WITHOUT breaking email

Your four mailboxes (`connor@`, `info@`, `ryan@`, `support@`) run on MX records at your
current DNS host. **Do not change the nameservers to Vercel** unless you first copy every
MX and TXT record across, or mail stops arriving.

Safe route: leave DNS where it is and add two records.

1. In Vercel: Project → **Settings → Domains** → add `life-receipt.com`, then `www.life-receipt.com`.
2. Vercel shows the exact records it wants. Add them at your registrar:
   - **A** · host `@` · value → the IP Vercel displays (typically `76.76.21.21`)
   - **CNAME** · host `www` · value `cname.vercel-dns.com`
3. Leave every existing **MX** record and any mail-related **TXT** (SPF/DKIM/DMARC) untouched.
4. Wait for Vercel to show "Valid Configuration" (usually minutes, up to a few hours).
5. Set `life-receipt.com` as the primary domain so `www` redirects to it.

**Verify after DNS propagates:** send a test email to `support@life-receipt.com` and confirm
it lands. Do this before submitting to Apple — the reviewer may email that address.

## 4. Before App Store submission

- [ ] All four URLs load over HTTPS with no mixed content
- [ ] `support@life-receipt.com` receiving mail
- [ ] Privacy Policy URL entered in App Store Connect **and** in the app listing
- [ ] Paywall screen links to `/terms` and `/privacy` (Apple checks this)
- [ ] Auto-renewal terms on the paywall match section 3 of `/terms`
- [ ] In-app **Delete account** path exists and matches what `/support` promises
- [ ] Privacy Nutrition Labels in ASC match `/privacy` (Health, email, purchases, diagnostics)

## Editing

Everything is hand-written HTML. Shared styling lives in `styles.css`.
Push to `main` and Vercel redeploys automatically.
