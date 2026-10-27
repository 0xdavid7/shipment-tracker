# Shipment status tracker for Codex on Windows

This folder is a starter workflow for a nontechnical user. Create
`shipments.json` with the browser form, then create a daily Scheduled task in
Codex Desktop with the prompt from `daily-check-prompt.md` and this folder as
its local project.

## Add a shipment

1. Open `shipment-entry.html` in Edge or Chrome.
2. Fill in the shipment form. Choose the tracking number type and enter that
   number. The form generates a unique Shipment ID if you leave it blank.
3. Click **Add shipment**. Repeat for each shipment.
4. Add the email addresses that should receive the daily digest.
5. Click **Download shipments.json** and copy the downloaded file into this
   Codex project folder.

To make later changes, open the form again, click **Import shipments.json**,
select the file from the Codex project, edit the shipment, and download the
updated file. The browser keeps a draft copy on that computer too. An inactive
example record is also included in `shipments.example.json`.

The original order fields are retained. `booking_number` and
`container_number` are added because several carriers accept those identifiers
when a B/L number is not available. `tracking_reference_type` tells the daily
workflow which identifier to use first.

Set `tracking_reference_type` to `booking`, `bill_of_lading`, or `container`.
This starter is designed for the ocean carriers in your list; air freight
tracking is not included.

## Set up the daily check

In Codex Desktop on the Windows computer:

1. Open this folder as a local project.
2. Create `shipments.json` before scheduling the first run.
3. Open **Scheduled** and create a daily task in this project's local
   environment. Choose the desired daily time and use the full text from
   `daily-check-prompt.md`.
4. Keep the computer on and Codex Desktop open when the task is due. Local
   scheduled tasks need the project files to remain available.
5. Enable Codex desktop notifications in Windows. Results and any failures
   should also be reviewed in **Scheduled**.
6. To receive email, connect an email app in Codex. The scheduled task uses
   the recipients from `shipments.json` and reports when it could not send
   email.

## What this version can and cannot do

The daily task searches public web pages and uses official carrier tracking
pages when it can. It must mark a shipment **Needs manual check** when a carrier
requires a form, sign-in, CAPTCHA, or other interaction the scheduled run cannot
complete. It must never infer a status from a search snippet or an unrelated
third-party page.

This is a best-effort, no-API-key starting point. It is not a guaranteed live
tracking feed. For dependable automatic status updates across all 12 carriers,
use a licensed multi-carrier tracking API or arrange API access with each line.
That option can incur fees and sends the selected tracking reference to the
tracking provider. No tracking provider has been configured or subscribed to
in this starter folder.

The daily task writes its latest verified results to `tracking-results.json`
and keeps prior status values there so it can report changes. Shipment details
are not copied into the result history unless needed to identify the shipment.
The `.gitignore` file excludes live shipment data and tracking history from Git
to reduce the chance of committing business-sensitive information.
