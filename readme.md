# Accessibility of AI Models

This repo is an effort to determine how well different AI models generate, test, and fix accessible code.

## Methodology

For each test, two models were compared. Sentiment was defined based on the output of the prompt. The sentiment is pass/fail for WCAG compliance when the prompt generates code. Otherwise, it reflect the level of helpfulness of the prompt. The code output was automatically analyzed by [axe devtools](https://github.com/dequelabs/axe-core) and manually by at lease one subject matter expert in accessibility, but a full WCAG audit was not performed.

This is NOT an exhaustive list of accessibility tests. These prompts do not cover all scenarios or accessibility requirements. However, they were chosen to reflect some common scenarios.

## Summary

AI models are getting better at identifying and generating accessible code, but they still fail at creating compliant code even when asked to do so. This being said, it may still be possible to provide instructions to have AI generate more accessible code more reliably. AI can be helpful in identifying and fixing many bugs, but the limitations of the models should be clearly conveyed to developers, as it can't identify and fix all issues. In other words, AI can be helpful as one of many tools to generate accessible code, but it can't yet be used as the only tool.

## Prompt 1: [basic-form.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/basic-form.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful registration form in HTML and JavaScript as a single file. The registration form is for a conference workshop.
```

Goal: Have the AI create a basic form. The AI is specifically NOT prompted about accessibility. This was done intentionally, because most developers are unlikely to include accessibility as a specific instruction.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| Codestral 25.01 | 🚫 Fail | 1 | [output](/results/codestral-25.01/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/codestral-25.01/basic-form.html) | Contrast issue on register button. Otherwise, it seems pretty accessible. It is using browser based form validation which may have accessibility limitations. Etc. |
| Claud Opus 4 | 🚫 Fail | 3 | [output](/results/claud-opus-4/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-opus-4/basic-form.html)| Radio buttons are missing acc names. Group label is not programmatically defined for the radio buttons. The success message is not announced automatically (fails 4.1.3 Status Message) .The "C" in the "complete registration" button just barely fails contrast. Help text is provided as placeholders (thus disappearing after entering a value) for the phone number and additional comments fields (this one is debatable as a WCAG fail). Etc. |
| Claud Sonnet 3.7 | 🚫 Fail | 1 | [output](/results/claud-sonnet-3.7/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-3.7/basic-form.html) | color contrast failure for submit button. Heading structure is okay. Color alone used to convey the current step. Error messages are not programmatically associated with form controls. Checkboxes on second set are missing a programmatic group label association. Focus lost when moving between steps. Etc. |
| Claud Sonnet 4.0 | 🚫 Fail | 3 (for error messages) | [output](/results/claud-sonnet-4.0/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-4.0/basic-form.html) | 2 h1s (but this isn't a WCAG failure). Keyboard focus lost when navigating between steps. "Select your workshop track" radio buttons/cards are missing a visual focus indicator. Error messages are not programmatically associated with inputs. Wizard progress bar is missing alt text. "Additional Information" checkboxes lack proper contrast for their focus indicators. Etc. |
| GPT 4.1 | 🚫 Fail | 0 | [output](/results/gpt4.1/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt4.1/basic-form.html) | No h1 (but this doesn't fail WCAG). No focus indicator for checkboxes. Checkboxes are misaligned (not a WCAG failure). Error text is not programmatically associated with form controls. Etc. |
| GPT o4-mini | 🚫 Fail | 1 | [output](/results/gpt-o4-mini/basic-form.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt-o4-mini/basic-form.html) | 1 color contrast issue found for the submit button.  No h1 (but this doesn't fail WCAG). No focus indicator for checkboxes or radio buttons. Error text is not programmatically associated with form controls. Etc. |

## Prompt 2: [custom-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/custom-datepicker.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful HTML / JavaScript user registration form. The form must include a custom JavaScript Date picker. Output this as a single file.
```

Goal: Have the AI create a complex custom date picker. This will help to test the limits and shortcomings of AI when generating code for more dynamic functionality. The AI is specifically NOT prompted about accessibility. This was done intentionally, because most developers are unlikely to include accessibility as a specific instruction.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| Codestral 25.01 | 🚫 Fail | 1 | [output](/results/codestral-25.01/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/codestral-25.01/custom-datepicker.html)  | Registration button fails contrast. Date picker is not keyboard accessible and is missing correct semantics. Etc. |
| Claud Opus 4 | 🚫 Fail | 11 | [output](/results/claud-opus-4/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-opus-4/custom-datepicker.html) | Not keyboard accessible at all - not possible to open the date picker via keyboard or otherwise provide a value and is missing correct semantics. Contrast fails for column headers and 'disabled' dates (date appear disabled but not not programmatically disabled). Etc. |
| Claud Sonnet 3.7 | 🚫 Fail | 2 | [output](/results/claud-sonnet-3.7/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-3.7/custom-datepicker.html) | 2 color contrast failures. Error messages are not programmatically associated with form controls. Cannot open date picker with keyboard or input a date manually. Checkboxes fail focus visible. Next/previous month buttons are missing acc names. Date picker grid is missing table/grid semantics. Date picker is missing correct semantics in general. All dates appear to be disabled for some some reason (this is more of a functional issue). Etc. |
| Claud Sonnet 4.0 | - | 18 | [output](/results/claud-sonnet-4.0/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-4.0/custom-datepicker.html) | Many color contrast issues. Datepicker is not keyboard accessible at all, and a date cannot be entered manually. Error text is not associated with inputs. Datepicker is missing correct table/grid and other semantics. Etc. |
| GPT 4.1 | 🚫 Fail | 0 | [output](/results/gpt4.1/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt4.1/custom-datepicker.html)  | No h1 (but this doesn't fail WCAG). Date picker table/grid is missing table/grid semantics (no header/row associations). The date picker is technically keyboard accessible, but is far from optimal. For example, it doesn't support arrow navigation, it doesn't close on escape, etc. |
| GPT o4-mini | 🚫 Fail | 1 | [output](/results/gpt-o4-mini/custom-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt-o4-mini/custom-datepicker.html)  |  1 color contrast issue found for the submit button. No h1 (but this doesn't fail WCAG). Date picker doesn't work for anyone - it's not even mouse accessible. Etc. |

## Prompt 3: [analyze-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/analyze-datepicker.prompt.yml)

Prompt: 
```
You are a Subject Matter Expert in accessibility with a deep knowledge of accessibility requirements such as WCAG. Find all WCAG issues in this file.
```
[File](/code-samples/datepicker.html)

Goal: Have the AI identify all WCAG issues. This will help to show how capable the AI is at identifying accessibility bugs.

| Model | sentiment | output | notes |
| --- | --- | --- | --- |
| Codestral 25.01 | 😐 somewhat helpful | [output](/results/codestral-25.01/analyze-datepicker.md) | Identified many of the accessibility bugs. However, it did have some inaccuracies. For example, it states that using a `span` instead of a `label` fails WCAG 4.1.2, which is false. It says there is no visual feedback after submitting the form, and that fails 3.2.2 On Input, which is false. |
| Claud Opus 4 | 😐 somewhat helpful | [output](/results/claud-opus-4/analyze-datepicker.md) | Incorrectly states that the malformed HTML fails 1.3.1 and 4.1.2. It did identify most of the issues. Missing table/grid semantics are not mentioned. Oddly, as with most models, it suggests that the input for the date should be a readonly text field, and that attempting to interact with this field should open the datepicker. This wouldn't be expected by most screen reader users since the field is marked as readonly, hinting that it can't be interacted with. A separate button to open the date picker would be better (as would not marking the field as readonly so that a date could be entered manually id desired). The suggested fixes for the datepicker keyboard functionality are helpful as a starting point, but are incomplete, missing implementations for arrow keys. Additionally, the next/previous month buttons are not mentioned in the fixes section at all.|
| Claud Sonnet 3.7 | 😐 somewhat helpful | [output](/results/claud-sonnet-3.7/analyze-datepicker.md) | Finds many of the issues. Fails to mention the missing table/grid semantics. Says that explicit instructions for how to use the datepicker are required (not true). Suggests adding `aria-haspopup` to the text input, without adding `role=combobox` (this would be unexpected). |
| Claud Sonnet 4.0 | 😐 somewhat helpful | [output](/results/claud-sonnet-4.0/analyze-datepicker.md) | Provides a priority-based list of fixes, which is good in theory, but it defines them as "must fix", "should fix", and "may fix". This is misleading as all WCAG failures MUST be fixed. But some are likely more impactful than others and should be prioritized first. Many improper WCAG SC references. No code examples/suggestions. Many vague suggestions which may be unclear without further context.  |
| GPT 4.1 | 👍 mostly helpful | [output](/results/gpt4.1/analyze-datepicker.md) | Most bugs were identified. It however incorrectly identified some SCs as failing. For example, it said that the user name missing a label fails 1.3.1 and 3.3.2. However, it's 4.1.2 that fails since the input doesn't have an acc name. There is a label - it's just not programmatically associated. It also called out some coding and usability best practices which don't technically fail WCAG. It also says that disabled dates fail contrast when no disabled dates are actually shown. Etc. |
| GPT o4-mini | 😐 somewhat helpful | [output](/results/gpt-o4-mini/analyze-datepicker.md) | Many bugs were identified. However, it called out the missing h1 as failing both 2.4.6 and 2.4.10 which is not accurate. It also says that disabled dates fail contrast when no disabled dates are actually shown. Etc. |

## Prompt 4: [site-with-nav.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/site-with-nav.prompt.yml)

Prompt: 
```
You are a developer. Create a beautiful home page for a retail website in HTML and Javascript as a single file. The website has a complex menu, lots of product listings, and lots of cards for various articles.
```

Goal: See how well the AI does at creating a page with many different sections, including a complex menu (which often suffers from keyboard issues), product and article listings (which often suffer from alt text, keyboard, semantics, and link purpose issues).  The AI is specifically NOT prompted about accessibility. This was done intentionally, because most developers are unlikely to include accessibility as a specific instruction.

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| Codestral 25.01 | 🚫 Fail (but did pretty good) | 2 | [output](/results/codestral-25.01/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/codestral-25.01/site-with-nav.html) | axe error were best practices rather than real WCAG failures. These were related to landmarks. Decorative images were assigned alt text. The entire page is very basic compared to what other models created. Basic navigation, product and article cards are not linked, not many colors, etc.  |
| Claud Opus 4 | 🚫 Fail | 19 | [output](/results/claud-opus-4/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-opus-4/site-with-nav.html) | 19 color contrast failures. Mega nav is mouse operable, but not keyboard accessible. "shop by category" cards are not keyboard accessible, but are mouse accessible. "Summer collection 2024" is an H1 but functions as a H2 (I think). Featured Products tab panel is missing tab panel semantics. Read more links are not clear out of context. Etc. |
| Claud Sonnet 3.7 | 🚫 Fail | 44 | [output](/results/claud-sonnet-3.7/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-3.7/site-with-nav.html) | Account, Wishlist, and Cart buttons are not keyboard accessible and are missing correct semantics. Main nav displays sub-menus on hover, but these are not keyboard accessible. "Summer collection 2023" is marked up as a h1, but it doesn't function as an h1. "Shop by Category" cards are not keyboard accessible and are missing correct semantics. "Featured Products" tab panel is missing tab panel semantics. Many cards use buttons with icons, but those icons are missing alt text. "read more" link text is not clear out of context. Etc. |
| Claud Sonnet 4.0 | 🚫 Fail | 44 | [output](/results/claud-sonnet-4.0/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-4.0/site-with-nav.html) | Many color contrast issues. Hero background image has an animation which can't be paused, stopped, or hidden. Header action buttons (heart and shopping cart) are not keyboard accessible and are missing correct semantics. Main nav has sub-menus which appear on hover, but these are not keyboard accessible. "Shop by category" cards are not keyboard accessible. Decorative images/icons are not marked as decorative. Read more links are not clear out of context. Etc. |
| GPT 4.1 | 🚫 Fail | 55 | [output](/results/gpt4.1/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt4.1/site-with-nav.html)  | Many issues were found, including contrast issues and color alone to identify links. Search button is missing an acc name. Not all menu items are keyboard navigable. Decorative images are given alt text. Read more links are not clear out of context. No way to bypass blocks. Etc.  |
| GPT o4-mini | 🚫 Fail | 12 | [output](/results/gpt-o4-mini/site-with-nav.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt-o4-mini/site-with-nav.html)  | Many issues were found, including contrast issues and color alone to identify links. Search button is missing an understandable acc name (icon is used). Some menu items have two tab stops. Decorative images are given alt text. Read more links are not clear out of context. No way to bypass blocks.  Etc. |

## Prompt 5: [fix-datepicker.prompt.yml](https://github.com/mfairchild365/a11y-models-playground/models/prompt/edit/main/fix-datepicker.prompt.yml)

Prompt: 
```
You are a Subject Matter Expert and developer in accessibility with a deep knowledge of accessibility requirements such as WCAG. Find all WCAG issues in this file and fix them. Generate the fixed code as a single HTML file.
```
[File](/code-samples/datepicker.html)

| Model | sentiment | # axe errors | output | notes |
| --- | --- | --- | --- | --- |
| Codestral 25.01 | 🚫 Fail | 7 | [output](/results/codestral-25.01/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/codestral-25.01/fix-datepicker.html) | Contrast fails for the heading and button. The date picker is still not keyboard accessible. Etc. |
| Claud Opus 4 | 🚫 Fail | 44 | [output](/results/claud-opus-4/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-opus-4/fix-datepicker.html) | `aria-expanded` is not allowed on inputs. `row` roles are missing from the grid. Datepicker only displays on enter, which also tries to submit the form (see the problem there?). Next/previous month buttons are not functional (for anyone). `aria-selected` is missing from the grid. Etc. |
| Claud Sonnet 3.7 | 🚫 Fail | 44 | [output](/results/claud-sonnet-3.7/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-3.7/fix-datepicker.html) | Makes the date input directly editable (good). Can't delete characters when editing the date until the date is fully entered (usability issue). Button was added to launch the date picker (good). Launched datepicker is not keyboard accessible. Etc. |
| Claud Sonnet 4.0 | 🚫 Fail | 41 | [output](/results/claud-sonnet-4.0/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/claud-sonnet-4.0/fix-datepicker.html) | Color contrast issues. Incorrect use of ARIA. Grid is missing rows. When navigating the grid with arrow keys, if you advance to the next month where all dates are disabled, then keyboard focus becomes lost. Text input for datepicker is missing the combobox role. Etc. |
| GPT 4.1 | 🚫 Fail, but it did improve results | 3 | [output](/results/gpt4.1/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt4.1/fix-datepicker.html)  | aria-expanded is not allowed on input. Submit button fails contrast. Date picker is keyboard accessible and supports esc, arrow keys. But the date picker table/grid is still missing table/grid structure. Etc. |
| GPT o4-mini | 🚫 Fail | 9 | [output](/results/gpt-o4-mini/fix-datepicker.html) - [preview](https://html-preview.github.io/?url=https://github.com/mfairchild365/a11y-models-playground/blob/main/results/gpt-o4-mini/fix-datepicker.html)  | aria-expanded is not allowed on input. Submit button fails contrast. Can't open datepicker with keyboard. Date picker table/grid is still missing table/grid structure. After opening with mouse, the date picker doesn't support arrow key navigation (just tab key). Missing landmarks. Etc. |
