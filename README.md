# SRPAL AMC Portal

SRPAL AMC Portal is a browser-based internal form for submitting station AMC maintenance checklists. It collects engineer and station details, validates the maintenance period and completion date, checks for duplicate submissions, and uploads a PDF checklist through Google Apps Script.

## Project files

```text
AMC-main/
|-- index.html    # Portal markup and client-side JavaScript
|-- style.css     # Portal styling and responsive layout
`-- README.md     # Project documentation
```

## Run locally

The portal should be opened through a local web server rather than directly with the `file://` protocol.

From the project directory, run:

```powershell
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/index.html
```

## Main features

- Engineer name and email validation
- Zonal railway, division, station, and category selection
- Dynamic maintenance year selection around the current system year
- Month display with backend values in `YYYY_Month` format
- Duplicate submission checking by station and maintenance period
- Maintenance date validation that prevents future dates
- PDF-only checklist upload with a 10 MB maximum size
- Optional remarks with a 500-character limit
- Submission acknowledgement generation
- Responsive desktop, tablet, and mobile layout

## Maintenance period format

The user selects a year and month separately. The existing `month` field continues to send the combined backend value.

Examples:

```text
Visible selections: 2026 and July
Submitted value:    2026_July

Visible selections: 2027 and March
Submitted value:    2027_March
```

The year list is generated dynamically from the current system year.

## Google Apps Script configuration

The Apps Script endpoint is configured in `index.html` using the `APPS_SCRIPT_URL` constant. The endpoint is used for:

- Loading station master data
- Checking existing station-period submissions
- Uploading checklist data and the PDF file

When deploying to another environment, update only the endpoint value and confirm that the Apps Script response format remains compatible with the existing callbacks.

## Upload rules

- Accepted format: PDF
- Maximum file size: 10 MB
- A station and maintenance period must pass the duplicate check before submission
- Required fields must pass client-side validation
- Maintenance Done Date cannot be later than the current date

## Deployment notes

- Serve `index.html` and `style.css` from the same directory.
- Keep the stylesheet path in `index.html` as `style.css`.
- Use HTTPS in production.
- Confirm the Apps Script deployment permits requests from the portal's deployed origin.
- Test station loading, duplicate checking, PDF upload, and acknowledgement display after deployment.

## Change safety

The form depends on its current element IDs, field names, Apps Script callbacks, and payload keys. Avoid renaming them unless the corresponding JavaScript and backend integration are updated together.
