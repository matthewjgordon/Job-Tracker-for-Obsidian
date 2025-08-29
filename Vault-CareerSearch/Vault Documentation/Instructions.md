- Clip the information from LinkedIn via Obsidian Web Clipper. Various variables grab the company name, the role's title, and the job description/requirements. Additional YAML is supplied. The clipper places the note in the "Job Listings" folder. 
- A `[[WikiLink]]` is generated in the YAML block of the Listing note for the "(App)" note.
- A new (App) note is automatically created. Both the (Listing) note and the (App) note will have a `[[WikiLink]]` to the other.
- The (App) Note's "Resume" and "Cover Letter" properties will automatically include a `[[WikiLink]]` to the appropriately named resume and cover letter files (PDF). 
> Note that you will need to update the resume and cover letter file naming scheme inside the `template - startup-listing-to-app`