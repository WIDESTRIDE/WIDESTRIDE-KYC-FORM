# Widestride Account Opening Form: Setup Guide

One link handles **Individual, Joint, Minor and Corporate** accounts. Every submission is saved to Google Sheets.

## Files
| File | What it is |
|---|---|
| `Index.html` | The form itself. It's multi-step, works on mobile and uses Widestride branding. |
| `Code.gs` | The Apps Script backend. It saves each application to the right sheet and stores photos, signatures and documents in Google Drive. |

## Steps
1. **Create a Google Sheet.** Name it something like *Widestride – Account Opening Applications*.
2. Go to **Extensions → Apps Script**.
3. Replace everything in `Code.gs` with the contents of **Code.gs** from this folder.
4. Click **+ → HTML** and name the file exactly **`Index`**. Paste in the contents of **Index.html**.
5. At the top of `Code.gs`, edit `CONFIG` if you need to:
   - `NOTIFY_EMAIL`: back-office emails that should get an alert for each new application (separate them with commas)
   - `SEND_APPLICANT_CONFIRMATION`: set to `true` to email the applicant a reference number. Corporate applications send it to both the official email and the primary contact.
6. Choose **setup** in the function dropdown, click **Run**, then approve the permissions (Sheets, Drive, Mail).
   This creates two tabs, **Applications** and **Corporate Applications**, plus the Drive folder *Widestride Account Opening Uploads*.
7. Go to **Deploy → New deployment → Web app**. Set Execute as to **Me** and Who has access to **Anyone**, then click **Deploy** and copy the **Web app URL**.

> **Already deployed the earlier version?** Paste in both new files, run **setup** again, then go to **Deploy → Manage deployments → Edit → Version: New version → Deploy**. The URL stays the same, and your existing individual rows are not changed.

## How the form flows
| Individual / Joint / Minor | Corporate |
|---|---|
| A Personal details (or guardian's details for a minor) | A Company details |
| A1 Minor details (minor only) | B Annual turnover & investment purpose |
| A2 Joint holder (joint only) | C Product |
| B Employment | D Bank account |
| C Product & funding | F Authorised signatories (1–4), each with photo, BVN, class and specimen signature, plus G Signing mandate |
| D Bank details | H Primary contact (can be copied from Signatory 1 with one click) |
| E Next of kin | I Board resolution. This is completed and e-signed in the form, and skipped for sole proprietorships. |
| F Declarations & signature | Documents checklist uploads |
| Review & submit | E Email indemnity, T&Cs & signatures, then Review & submit |

## What gets recorded
- **Individual, joint and minor applications** go to the *Applications* tab with references like `WS-260924-0001`. Duplicates are checked by **BVN**.
- **Corporate applications** go to the *Corporate Applications* tab with references like `WSC-260924-0001`. Duplicates are checked by **RC number**.
- **Uploads** are saved in one Drive sub-folder per application. This covers passport photos, signatures, specimen signatures, board resolution signatures and checklist documents (Certificate of Incorporation, CAC forms, MEMART, IDs, proofs of address, deposit evidence). The sheet stores the links.
- **Back-office columns** on the corporate tab are highlighted in green. They come from the *For Internal Use Only* and *AML Risk Categorisation* pages: KYC verified, deferred documents, client file number, relationship manager, operations officer, AML risk rating (1–5), customer category (Low to High), justification, compliance officer and date. Clients never see these fields.
- The **Status** dropdown (Pending Review → Approved/Rejected) and colour coding are on both tabs.

## Notes
- If you open `Index.html` directly in a browser, it runs in **preview mode** and nothing is saved.
- Each document upload is limited to 5MB. Images are compressed automatically.
- Products B, C and D were blank on both paper forms. To add products, edit this line in `Index.html`:
  `C('product', 'Product', ['Widestride Balanced Product', 'Others'], …)`
