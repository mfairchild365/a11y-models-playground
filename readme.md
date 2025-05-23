# Accessibility of AI Models

This repo is an effort to determine how well different AI models generate, test, and fix accessible code.

## Methodology

For each test, two models were compared. Sentiment was defined based on the output of the prompt. The sentiment is pass/fail for WCAG compliance when the prompt generates code. Otherwise, it reflect the level of helpfulness of the prompt. The code output was analyzed by a subject matter expert in accessibility, but a full WCAG audit was not performed.

This is NOT an exhaustive list of accessibility tests. These prompts do not cover all scenarios or accessibility requirements. However, they were chosen to reflect some common scenarios.

## Summary

AI models are getting better at identifying and generating accessible code, but they still fail at creating compliant code even when asked to do so. This being said, it may still be possible to provide instructions to have AI generate more accessible code more reliably. However, AI can be helpful in identifying and fixing many bugs, but the limitations of the models should be clearly conveyed to the user, as it can't identify and fix all issues.

## Prompt 1: [basic-form.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/basic-form.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful registration form in HTML and JavaScript. The registration form is for a conference workshop.
```

Goal: Have the AI create a basic form. The AI is specifically NOT prompted about accessibility. This was done intentionally, because most developers are unlikely to include accessibility as a specific instruction.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| GPT 4.1 | 🚫 Fail | 0 | [outout](/results/2025-05-23/basic-form.gpt4.1.html) | No h1 (but this doesn't fail WCAG). No focus indicator for checkboxes. Checkboxes are misaligned (not a WCAG failure). Error text is not programmatically associated with form controls. Etc. |
| o4-mini | 🚫 Fail | 1 | [outout](/results/2025-05-23/basic-form.gpt-o4-mini.html) | 1 color contrast issue found for the submit button.  No h1 (but this doesn't fail WCAG). No focus indicator for checkboxes or radio buttons. Error text is not programmatically associated with form controls. Etc. |


## Prompt 2: [custom-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/custom-datepicker.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful HTML / JavaScript user registration form. The form must include a custom JavaScript Date picker. Output this as a single file.
```

Goal: Have the AI create a complex custom date picker. This will help to test the limits and shortcomings of AI when generating code for more dynamic functionality. The AI is specifically NOT prompted about accessibility. This was done intentionally, because most developers are unlikely to include accessibility as a specific instruction.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| GPT 4.1 | 🚫 Fail | 0 | [outout](/results/2025-05-23/custom-datepicker.gpt-4.1.html) | No h1 (but this doesn't fail WCAG). Date picker table/grid is missing table/grid semantics (no header/row associations). The date picker is technically keyboard accessible, but is far from optimal. For example, it doesn't support arrow navigation, it doesn't close on escape, etc. |
| o4-mini | 🚫 Fail | 1 | [outout](/results/2025-05-23/custom-datepicker.o4-mini.html) |  1 color contrast issue found for the submit button. No h1 (but this doesn't fail WCAG). Date picker doesn't work for anyone - it's not even mouse accessible. |


## Prompt 3: [analyze-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/analyze-datepicker.prompt.yml)

Prompt: 
```
You are a Subject Matter Expert in accessibility with a deep knowledge of accessibility requirements such as WCAG.
```
[File](/code-samples/datepicker.html)

Goal: Have the AI identify all WCAG issues. This will help to show how capable the AI is at identifying accessibility bugs.

| Model | sentiment | output | notes |
| --- | --- | --- | --- |
| GPT 4.1 | 👍 mostly helpful | [outout](/results/2025-05-23/analyze-datepicker.gpt-4.1.md) | Most bugs were identified. It however incorrectly identified some SCs as failing. For example, it said that the user name missing a label fails 1.3.1 and 3.3.2. However, it's 4.1.2 that fails since the input doesn't have an acc name. There is a label - it's just not programmatically associated. It also called out some coding and usability best practices which don't technically fail WCAG. It also says that disabled dates fail contrast when no disabled dates are actually shown. Etc. |
| o4-mini | 😐 somewhat helpful | [outout](/results/2025-05-23/analyze-datepicker.o4-mini.md) | Many bugs were identified. However, it called out the missing h1 as failing both 2.4.6 and 2.4.10 which is not accurate. It also says that disabled dates fail contrast when no disabled dates are actually shown. Etc. |


## Prompt 4: [site-with-nav.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/site-with-nav.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful home page for a retail website in HTML and Javascript as a single file. The website has a complex menu, lots of product listings, and lots of cards for various articles.
```

Goal: Have the identify and fix accessibility bugs. This will help to show how capable the AI is at such tasks.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| GPT 4.1 | 🚫 Fail | 55 | [outout](/results/2025-05-23/site-with-nav.gpt-4.1.html) | Many issues were found, including contrast issues and color alone to identify links. Search button is missing an acc name. Not all menu items are keyboard navigable. Decorative images are given alt text. Read more links are not clear out of context. No way to bypass blocks. Etc.  |
| o4-mini | 🚫 Fail | --- | [outout](/results/2025-05-23/site-with-nav.o4-mini.html) | Many issues were found, including contrast issues and color alone to identify links. Search button is missing an understandable acc name (icon is used). Some menu items have two tab stops. Decorative images are given alt text. Read more links are not clear out of context. No way to bypass blocks.  Etc. |


## Prompt 5: [fix-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/fix-datepicker.prompt.yml)

Prompt: 
```
You are a Subject Matter Expert and developer in accessibility with a deep knowledge of accessibility requirements such as WCAG. Find all WCAG issues in this file and fix them. Generate the fixed code as a single HTML file.
```
[File](/code-samples/datepicker.html)

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| GPT 4.1 | 🚫 Fail, but it did improve results | 3 | [outout](/results/2025-05-23/fix-datepicker.gpt-4.1.md) | aria-expanded is not allowed on input. Submit button fails contrast. Date picker is keyboard accessible and supports esc, arrow keys. But the date picker table/grid is still missing table/grid structure. Etc. |
| o4-mini | 🚫 Fail | 9 | [outout](/results/2025-05-23/fix-datepicker.o4-mini.md) | aria-expanded is not allowed on input. Submit button fails contrast. Can't open datepicker with keyboard. Date picker table/grid is still missing table/grid structure. After opening with mouse, the date picker doesn't support arrow key navigation (just tab key). Missing landmarks. Etc. |


