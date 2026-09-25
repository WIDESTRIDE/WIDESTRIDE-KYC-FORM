# Widestride Account Opening: Setup Guide

## How it works
1. **An admin creates a client** in the Admin Portal. The client record holds the account type, name, BVN, email and phone.
2. **The client gets the form link** by email automatically. The admin can also copy the link or a ready-to-send message.
3. **The client verifies** on the form with their BVN plus their registered email *or* phone number. If there's no matching invitation, the form stays locked.
4. **The form unlocks.** The account type is locked to the one the admin chose, and the BVN, email and phone are pre-filled. The client completes the form and submits it.
5. **The application is saved** to Google Sheets and Drive. The client's status changes to **Submitted**, and the confirmation email goes out with legal@, chinemerem.n@ and munachi.b@ copied.

## Files
| File | Where it goes |
|---|---|
| `Code.gs` | Apps Script (the backend) |
| `github-pages/index.html` | GitHub: the customer form |
| `github-pages/admin.html` | GitHub: the Admin Portal |
| `Index.html`, `Admin.html` | The same two pages, if you prefer to host them inside Apps Script instead of GitHub |

## Setup (GitHub hosting)
1. **Apps Script**
   - Replace everything in Code.gs with the new **Code.gs**.
   - In `CONFIG` at the top, set:
     - `FORM_URL`: the address of your customer form, e.g. `https://yourname.github.io/yourrepo/`
     - `FIRST_ADMIN_EMAIL`: the email of the first Super Admin. If you leave it blank, your Google account email is used.
   - Save, choose **setup** from the function dropdown, and click **Run**. Approve the permissions.
   - Open **Execution log**. It shows the first Super Admin's email and **temporary password**. Copy it, because no email is sent.
   - Go to **Deploy → Manage deployments → Edit → New version → Deploy**. For a first deployment, use **New deployment → Web app** with Execute as **Me** and access **Anyone**. Copy the Web App URL.
2. **Both HTML files:** open `index.html` and `admin.html`, search for `PASTE_YOUR_WEB_APP_URL_HERE`, and paste your Web App URL in both.
3. **GitHub:** upload `index.html` and `admin.html` to the root of your repo.
   - Customer form: `https://yourname.github.io/yourrepo/`
   - Admin Portal: `https://yourname.github.io/yourrepo/admin.html`
4. **First login:** open the Admin Portal and sign in with the temporary password. You'll be asked to set your own password straight away.

> If every Super Admin is ever locked out, run `emergencyResetSuperAdmin` from the Apps Script editor. It prints a new temporary password for `FIRST_ADMIN_EMAIL` in the Execution log.

## Admin Portal features
| | Super Admin | Admin |
|---|:-:|:-:|
| Create client and send invite; copy link or message | ✓ | ✓ |
| View and update a client's profile; resend invite; disable or re-enable access | ✓ | ✓ |
| See a client's submitted application and activity | ✓ | ✓ |
| Change own password | ✓ | ✓ |
| Create admins (auto-generated temporary password, no email) | ✓ | — |
| Reset an admin's password, suspend, reactivate, remove, restore, change role | ✓ | — |
| Audit log | ✓ | — |

**Password rules**
- New admins and reset passwords get a 12-character temporary password. It's shown once, on screen only.
- Admins must change the temporary password at first sign-in, before they can do anything else.
- New passwords need 8 or more characters, with an uppercase letter, a lowercase letter and a number.
- Passwords are stored as salted hashes. Nobody, including Super Admins, can see an admin's password.
- After 5 wrong attempts, the account is locked for 15 minutes.
- Resetting a password, or suspending or removing an admin, signs that admin out immediately.
- Sessions last up to 6 hours.
- You can't suspend or remove yourself, and at least one active Super Admin must always remain.

## Client statuses
| Status | Meaning |
|---|---|
| **Invited** | Created. The client hasn't opened the form yet. |
| **In Progress** | The client has verified and is filling the form. |
| **Submitted** | The application was received. The form can't be submitted again. |
| **Disabled** | Access is blocked. |

To let a client resubmit, for example to correct something, open their profile and set the status back to **Invited**.

**Corporate clients:** enter the BVN of the principal signatory. It's used to verify access and must match **Signatory 1's BVN** on the form.

## Google Sheet tabs
| Tab | Contents |
|---|---|
| Applications | Individual, joint and minor applications. The **Client ID** column is the last column. |
| Corporate Applications | Corporate applications. The **Client ID** column is the last column. |
| Clients | One row per invited client, with status and application ref |
| Admins | Admin accounts. The password hash, salt and session columns are hidden. **Don't edit this tab by hand.** |
| Audit Log | Every admin and client action, with time and who did it |

## Notes
- Only share edit access to the Google Sheet with people you trust. The Admins and Clients tabs hold sensitive data.
- Each document upload is limited to 5MB. Images are compressed automatically.
- Apps Script's daily email quota applies to invitations and confirmations: about 100 recipients a day on free Gmail, or 1,500 a day on Google Workspace.
- After any change to Code.gs, publish a **new version** of the same deployment so the URL stays the same.
