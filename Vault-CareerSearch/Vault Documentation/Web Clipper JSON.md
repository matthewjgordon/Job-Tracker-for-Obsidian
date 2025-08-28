```
{
  "schemaVersion": "0.1.0",
  "name": "Job Listing (LinkedIn)",
  "behavior": "create",
  "noteContentFormat": "{{selectorHtml:.jobs-description__container|markdown}}",
  "properties": [
    {
      "name": "Type",
      "value": "Listing",
      "type": "text"
    },
    {
      "name": "date created",
      "value": "{{date|date:\\\"YYYY-MM-DD\\\"}}",
      "type": "text"
    },
    {
      "name": "Company",
      "value": "{{selector:.job-details-jobs-unified-top-card__company-name a}}",
      "type": "text"
    },
    {
      "name": "Role",
      "value": "{{selector:h1.t-24.t-bold.inline}}",
      "type": "text"
    },
    {
      "name": "URL",
      "value": "{{url}}",
      "type": "text"
    },
    {
      "name": "App",
      "value": "[[{{selector:.job-details-jobs-unified-top-card__company-name a}} - {{selector:h1.t-24.t-bold.inline}} (App)]]",
      "type": "text"
    }
  ],
  "triggers": [],
  "noteNameFormat": "{{selector:.job-details-jobs-unified-top-card__company-name a}} - {{selector:h1.t-24.t-bold.inline}} (Listing)",
  "path": "Job Listings"
}
```