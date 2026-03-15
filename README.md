# Google Workspace Resume Automation

A Google Workspace-based automation designed to help recruiters generate branded, client-ready presentation résumés faster, more consistently, and with significantly less manual work.

## Table of Contents

- [Original Scenario](#original-scenario)
- [Pain Points](#pain-points)
- [Mapping Out Requirements](#mapping-out-requirements)
- [Design of the New Solution](#design-of-the-new-solution)
- [Google Form](#google-form)
- [Google Docs](#google-docs)
- [Google Sheet](#google-sheet)
- [Apps Script \& ChatGPT API](#apps-script--chatgpt-api)
- [Final Result](#final-result)
- [Business Impact](#business-impact)
- [Video of the Solution Working](#video-of-the-solution-working)

## Original Scenario

This solution was created for a small recruiting company with a team of approximately 8 to 10 recruiters.

When recruiters needed to present candidates to clients, they typically prepared a customized presentation résumé. This document combined information from the candidate’s original résumé with notes gathered during the recruiter’s interview process. The final output emphasized the candidate’s strengths for a specific role, including relevant experience, education, and skills, while excluding personal contact information.

The purpose of the document was to present the candidate in a professional, client-facing format that aligned with the recruiting company’s process and brand.

At the time, this work was being done entirely manually. Recruiters had to gather the information, rewrite it into the required format, and adjust the document before sending it to the client. As a result, a significant amount of recruiter time was being spent on administrative work instead of sourcing, evaluating, and presenting more candidates.

## Pain Points

After interviewing team leads and recruiters, several operational issues became clear.

Although some recruiters had already started using AI tools on their own, the process was still inconsistent and highly manual. In many cases, recruiters copied information from their notes and the candidate’s résumé into different AI tools, then copied the response back into a Google Doc and manually adjusted the final formatting.

The main pain points were:

- **High administrative workload:** Recruiters were still spending too much time collecting, organizing, and rewriting candidate information.
- **Lack of process standardization:** Some recruiters used one AI tool, others used a different one, and some continued to do the work entirely manually.
- **Manual formatting against brand guidelines:** Even when there was a preferred brand format, recruiters still had to manually adjust typography, layout, spacing, and document structure.
- **Reduced recruiter output:** Because résumé preparation took so much time, recruiters had less capacity to present additional candidates to clients.

## Mapping Out Requirements

The requirements for the solution were clear and practical.

First, recruiters needed a structured way to capture candidate information in an orderly format. The process had to be simple enough to use consistently while still collecting the detailed inputs required to produce a strong presentation résumé.

Second, the solution needed to remain inside the company’s existing ecosystem: **Google Workspace**. This was important both for ease of adoption and for cost efficiency.

Finally, the workflow needed to be scalable. As the recruiting team grew, new recruiters should be able to learn the process quickly and use the same tool without requiring major operational changes or additional administrative support.

## Design of the New Solution

The final solution was built entirely within Google Workspace.

Recruiters submitted candidate information through a **Google Form**, and those responses were automatically logged into a **Google Sheet**. From there, **Google Apps Script** processed the submission, built the appropriate prompt, and sent the information to the **ChatGPT API**.

Because the company already had a paid ChatGPT account, the only additional setup required was enabling API credits. Since the requests were relatively lightweight, each call was highly cost-efficient and remained below one cent per generated document.

Once the API returned the structured résumé content, the script inserted the output into a **Google Docs template** designed around the company’s preferred client-facing format. The completed résumé was then saved in a dedicated **Google Drive folder** and automatically shared with the recruiter who submitted the form.

To complete the workflow, the recruiter received both an **email notification** and a **Google Chat notification** confirming that the document had been created and was ready for review.

![alt text](https://github.com/silviogs86/ResumeFormating/blob/main/Pictures/Design.png)<br>

## Google Form

The Google Form was designed to capture all the information required to generate the final presentation résumé.

Each recruiter’s email address was recorded as part of the submission so the generated document could be shared with the correct person. The recruiter also selected the desired output language for the résumé, either **English** or **Spanish**. The input itself did not need to be written in the same language as the output, but the selected language determined which API prompt would be used.

The form collected the following fields:

- Recruiter email
- Output language
- Client
- Job title
- Candidate name
- Experience
- Education
- Skills

The most important sections were **experience**, **education**, and **skills**, since these fields contained the source material used to build the final résumé.

## Google Docs

A standardized Google Docs template was created to ensure every generated résumé followed the same visual and structural format.

The template included placeholders for all dynamic content, as well as the company’s preferred layout, typography, alignment rules, emphasis styles, and brand elements such as logo placement. This allowed the automation to generate documents that were not only structured correctly, but also presentation-ready from a branding perspective.

Once the ChatGPT API returned the processed content, the script mapped each section into its corresponding placeholder inside the template.

The structure of the template was defined together with the client. For example, each experience entry was expected to include detailed supporting content such as:

- Company name
- Location
- Dates
- Position title
- Key bullet points

After the document was generated, it was automatically shared with the recruiter so they could review it, make any necessary edits, and send it to the client.

Doc template in Spanish
![alt text](https://github.com/silviogs86/ResumeFormating/blob/main/Pictures/Doc%201%20spanish.png)<br>


Doc template in English
![alt text](https://github.com/silviogs86/ResumeFormating/blob/main/Pictures/Doc%201%20english.png)<br>


## Google Sheet

The Google Sheet served as both the default response log for the Google Form and a lightweight operational control panel for the solution.

All form responses were automatically stored in the main response tab. In addition, a second tab called **Resumes** was added for tracking and troubleshooting purposes.

This tab stored key information such as:

- Candidate name
- Company name
- Processed API output
- Link to the generated résumé

This made it easier for whoever managed the workflow to identify failures or inconsistencies. For example, if a résumé was not created correctly or the API output came back blank, the issue could be quickly traced by reviewing the corresponding row in the tracking tab.

## Apps Script & ChatGPT API

**Apps Script** acted as the core orchestration layer of the solution.

Within the script, we configured the required IDs and references as script properties, including the Google Docs template ID, the destination Google Drive folder ID, and other reusable configuration values. We also added supporting logic to improve output consistency, such as a reference list of commonly used cities when location data was entered inconsistently.

The script handled the full automation flow:

1. Read the Google Form submission from the Google Sheet  
2. Map the input into the correct API prompt  
3. Send the request to the ChatGPT API  
4. Receive and structure the output  
5. Insert the processed content into the Google Docs template  
6. Save, share, and notify the recruiter  

On the API side, the prompt was intentionally designed to prioritize accuracy, consistency, and control.

The model was instructed to:

- Use only the information explicitly provided in the input
- Avoid generating filler or generic content
- Leave information blank when there was not enough supporting detail
- Make only reasonable, limited inferences based on the source material
- Follow defined length and structure rules for each résumé section

For example, if a candidate had worked as a cashier, it was reasonable to infer experience in customer service. However, it would not be acceptable to infer inventory management unless that responsibility was explicitly supported by the input.

This prompt design helped ensure that the final résumé remained aligned with the recruiter’s notes, the candidate’s actual background, and the company’s preferred presentation style.

## Final Result

The final result was a scalable, low-cost résumé generation workflow built entirely inside Google Workspace.

Instead of manually rewriting and formatting each candidate presentation résumé, recruiters could now submit structured information through a form and receive a branded, editable Google Docs file with much less effort.

This produced three major improvements:

- Reduced administrative work for recruiters
- Greater consistency across recruiter output
- Faster turnaround for client-ready candidate presentations

In practical terms, the solution helped shift recruiter time away from repetitive formatting work and back toward higher-value recruiting activity.

![Final result placeholder](YOUR_IMAGE_LINK_HERE)

## Business Impact

The solution delivered measurable operational value while maintaining an extremely low running cost.

Over a one-year period, the workflow processed **2,185 résumés**. Of those, **44** (roughly **2%**) were created for testing purposes, confirming that nearly all usage came from real recruiter activity.

Across that same period, the total API cost was only **$10 USD**, which translates to an average cost of approximately **$0.0046 per résumé**.

These results showed that the solution was both operationally scalable and financially sustainable, giving the team a standardized process at a negligible per-document cost.

## Video of the Solution Working

A demo video can be added here to show the end-to-end workflow, from form submission to document creation and recruiter notification.

[Video link placeholder](YOUR_VIDEO_LINK_HERE)
