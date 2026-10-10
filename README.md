# GitHub Pages + Google Apps Script email backend

## Files
- `index.html`: static assessment site, preconfigured with the supplied Apps Script Web App URL.
- `Code.gs`: Apps Script backend that receives POST submissions, generates a PDF and emails it.
- Deploy the backend update before using the GitHub Pages site.

## Critical setup: update your existing Apps Script deployment
The supplied URL is configured in `index.html`:
`https://script.google.com/macros/s/AKfycbzxYq0MDazYDtZtab4d_IxKoBb-7NPOfa6aUypT-UGjvFDyKv8xDa4FlKQRgqO2J_nn/exec`

1. Open https://script.google.com/ and open the project associated with that URL.
2. Replace the entire contents of `Code.gs` with the `Code.gs` in this package.
3. Confirm `RECIPIENT_EMAIL` is the intended address. Current default: `jontorapadang35@gmail.com` (verify it is correct).
4. Save.
5. Click **Deploy → Manage deployments → Edit (pencil)**.
6. For Version, choose **New version**, then click **Deploy**. The existing web app URL generally stays the same.
7. Ensure deployment executes as **Me** and has access settings suitable for your class. The first email send may require authorization.
8. Test backend URL by opening it in a browser. It should show “Backend is deployed.”
9. Use a test submission from GitHub Pages and check inbox/spam and Apps Script **Executions**.

## Publish the assessment on GitHub Pages
1. Create/open a GitHub repository.
2. Upload `index.html` to the repository root (you do not need to upload `Code.gs` to GitHub for hosting; keep it for reference if desired).
3. Go to **Settings → Pages**.
4. Choose **Deploy from a branch**, select `main` and `/(root)`, then Save.
5. Open the published Pages URL and test a submission.

## How the integration works
GitHub Pages cannot run Apps Script server functions directly. This version sends a hidden HTML form POST to the Apps Script `/exec` URL. The Apps Script `doPost(e)` receives the payload, builds the PDF and sends the email. The hidden iframe means the student stays on the result page; due to cross-origin browser limits, the page cannot reliably display the server's actual email-sending result. Confirm delivery via Gmail and Apps Script Executions.

## Security and reliability notes
- This is a starter assessment tool, not secure proctoring. Browser-supplied score/answers can be altered by a technically skilled user. For formal examinations, calculate scores on the server from canonical questions, use authentication, validate submissions and keep secure records.
- Anyone who can access the public web app endpoint may attempt to submit email payloads. Restrict access where possible and consider adding a shared secret/token, rate limiting, and validation before wider use.
- Keep the recipient address and student data private.
