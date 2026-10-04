# toolvik-data

Public holiday data for the **Toolvik** app (Global Working Days Calculator).
This repository holds **data only** — no app code, no keys, no personal data.

The app works fully offline with the data bundled in it. When the phone is online it checks
`holidays/v1/index.json` at most once a day and downloads a list only if its version is newer.
Every downloaded file is checked first (SHA-256 from the index, then every record); if anything
fails, the app keeps its own data.

## Files

| File | Content |
|---|---|
| `holidays/v1/index.json` | dataset version, and for each list: file name, SHA-256, number of records |
| `holidays/v1/<KEY>.json` | the **official** year lists of one holiday list: `IN` (Central Government, Delhi / New Delhi), `IN-WB` (Government of West Bengal), `JP`, `AE`, `US`, `GB` |

Years without an official list are not in these files; the app calculates them from the law and marks
them `CALCULATED` or `TENTATIVE`.

## Sources

Only official government documents, each record carrying its own `official_source` and `source_url`:

- India: DoPT Office Memorandum F.No.12/2/2023-JCA dated 3 July 2025 (holidays 2026)
- West Bengal: Finance (Audit) Department Notification No. 4188-F(P2) dated 27/11/2025 (holidays 2026)
- Japan: Cabinet Office national holidays list (syukujitsu.csv), 2024–2027

## Record

`date` (YYYY-MM-DD), `holiday_name` (`en`, `bn`, `hi`, …), `holiday_type`, `official_source`, `source_url`,
`publication_date`, `effective_year`, `last_verified`, `verification_status`, `confidence`, `notes`.

`verification_status`: `OFFICIAL_CONFIRMED` (read from the government document), `CALCULATED` (fixed rule
in law), `TENTATIVE` (expected date, not confirmed — e.g. moon-dependent days), `UNVERIFIED` (never counted).

The files are generated from the app's data with `npm run export-holiday-data` in the app repository.
Dates can change when the authorities issue new notifications.
