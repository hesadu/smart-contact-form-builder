# Smart Contact Form Builder

A WordPress contact form plugin built for real-world lead generation. Features a drag-and-drop form builder, live admin preview, AJAX submission, advanced spam protection, and a full submissions dashboard — all without any third-party dependencies.

---

## Features

- **Multi-form support** — create and manage unlimited forms, each with its own settings
- **Drag & drop builder** — add, reorder, and configure fields visually
- **Live preview** — see changes in real time inside the admin panel
- **AJAX submission** — no page reload, works with page caching plugins
- **Spam protection** — honeypot, time-on-page check, disposable email blocking, keyword scoring
- **German validation** — PLZ (postcode) and address validation on both client and server side
- **reCAPTCHA v3** — optional, uses your own Google API key
- **Rate limiting** — configurable attempt limits, no API key required
- **Submissions dashboard** — all entries saved to a custom DB table, viewable in admin
- **Email notifications** — with Reply-To automatically set to the submitter
- **Layout editor** — control form width, padding, background (color / gradient / image)
- **Shortcode based** — `[smart_contact_form id=1]`

---

## Installation

1. Download the latest zip from [Releases](../../releases)
2. Go to **WordPress Admin → Plugins → Add New → Upload Plugin**
3. Upload the zip and click **Activate**
4. Navigate to **Contact Form → All Forms** to create your first form
5. Copy the shortcode and paste it into any page

---

## Requirements

| | |
|---|---|
| WordPress | 5.8 or higher |
| PHP | 7.2 or higher |
| Tested up to | WordPress 6.9 |

---

## File Structure

```
smart-contact-form/
├── smart-contact-form.php        ← Main plugin file
├── admin/
│   ├── class-scf-admin.php       ← Admin AJAX handlers, settings save
│   ├── css/admin.css
│   ├── js/admin.js
│   └── views/
│       ├── forms-list.php        ← All Forms page
│       ├── settings.php          ← Form Builder (Design, Info, Builder, Email, Layout tabs)
│       ├── layout.php            ← Dedicated Layout Editor page
│       └── submissions.php       ← Submissions viewer
├── frontend/
│   ├── class-scf-frontend.php    ← AJAX submit handler, enqueue scripts
│   ├── css/frontend.css
│   ├── js/frontend.js
│   └── views/form.php            ← Form HTML + inline submit JS
└── includes/
    ├── class-scf-database.php    ← Custom DB tables, CRUD
    ├── class-scf-mailer.php      ← Email notifications
    ├── class-scf-spam-guard.php  ← Spam protection logic
    ├── class-scf-rate-limiter.php
    ├── class-scf-recaptcha.php
    └── class-scf-shortcode.php
```

---

## Database Tables

The plugin creates two custom tables on activation:

- **`wp_scf_forms`** — stores form names and settings (JSON)
- **`wp_scf_submissions`** — stores all form submissions with IP, user agent, status, and timestamp

---

## Spam Protection

Submissions go through multiple layers before being saved:

1. **Honeypot field** — invisible field that bots fill in
2. **Time-on-page check** — blocks submissions faster than a human can type
3. **Email validation** — format check + disposable domain blocklist
4. **Phone validation** — blocks all-same-digit and sequential numbers
5. **Name validation** — blocks keyboard mash, fake names
6. **PLZ validation** — German postcode format (5 digits, no invalid prefixes)
7. **Address validation** — blocks test/muster/asdf patterns
8. **Message scoring** — weighted keyword scoring for financial spam, broker language
9. **Cross-field check** — first name must differ from last name

---

## External Services

| Service | When used | Data sent |
|---|---|---|
| Google Fonts | Always (for font loading) | Font name only |
| Google reCAPTCHA v3 | Only if enabled by admin | User interaction data |

reCAPTCHA is **disabled by default**. It only activates when you provide your own API keys in the form settings.

---

## Shortcode Usage

```
[smart_contact_form id=1]
```

Each form has its own `id`. You can place multiple different forms on the same page.

---

## Debug Log

When submissions fail silently, check:

```
wp-content/scf-debug.log
```

Every submission attempt is logged with the reason it passed or was blocked.

---

## License

GPLv2 or later — see [LICENSE.txt](LICENSE.txt)

---

## Author

Built by [Hesadu Creatives](https://hesaducreatives.com)
