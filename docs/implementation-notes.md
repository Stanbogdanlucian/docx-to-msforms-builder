# Implementation Notes

## Local schema first

The most important implementation decision is to create a local structured schema before touching Microsoft Forms.

Recommended schema shape:

```json
{
  "title": "Questionnaire title",
  "description": ["Paragraph 1", "Paragraph 2"],
  "sections": [
    {
      "title": "Section title",
      "questions": [
        {
          "question": "Question text",
          "forms_type": "single_choice",
          "options": ["Yes", "No", "Needs review"]
        }
      ]
    }
  ]
}
```

Useful `forms_type` values:

- `text`
- `long_text`
- `single_choice`
- `multiple_choice`

## Microsoft Forms execution

Because there is no official authoring API, browser automation should be small and defensive:

- create or open a form in the Microsoft Forms editor;
- set the form title and description;
- insert sections;
- insert text or choice questions;
- paste choice options line-by-line so Forms expands them into separate options;
- turn on `Multiple answers` only for multiple-choice questions;
- wait for `Saved` before continuing.

## Copy strategy

When multiple forms share most of their structure, create and verify one base form first. Then use Microsoft Forms' native `Copy` action and edit only the differences in each copy.

This is usually more reliable than recreating every question repeatedly through the UI.

## Verification

Automated verification should compare the rendered editor state against the local schema:

- all expected section titles are present;
- all expected question texts are present;
- all expected choice options are present;
- multiple-choice questions have `Multiple answers` enabled;
- the editor reports `Saved`.
