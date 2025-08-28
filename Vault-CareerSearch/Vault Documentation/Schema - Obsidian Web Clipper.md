
`"[Company Name] - [Role Title] ([Note Type])"`
Ex. "Airbnb - Manager, Learning Performance (Listing)"
## Company Name
`{{selector:.job-details-jobs-unified-top-card__company-name a}}`

![[LinkedIn Company Name Example.png]]

## Role Title
`{{selector:h1.t-24.t-bold.inline}}`

![[LinkedIn Company Role Title Example.png]]


## Job Description
`{{selectorHtml:.jobs-description__container|markdown}}`

![[LinkedIn Job Description Example.png]]

## App Note Name

`[[{{selector:.job-details-jobs-unified-top-card__company-name a}} - {{selector:h1.t-24.t-bold.inline}} (App)]]`
