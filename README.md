# DOCX to Microsoft Forms Builder

This repository documents a practical workflow for turning a structured Word questionnaire into Microsoft Forms.

Microsoft Forms currently does not provide a stable public authoring API for creating and editing forms programmatically. The reliable pattern is therefore:

1. extract a local questionnaire schema from a `.docx` file;
2. normalize questions, sections, options, and answer types;
3. use Microsoft Forms UI automation only as the final execution layer;
4. verify the created forms against the local schema.

The key idea is to keep the local schema as the source of truth. Microsoft Forms is treated as a target editor, not as the place where structure is inferred.

## Workflow

1. Parse the Word document.
2. Produce a JSON schema with:
   - form title and description;
   - sections;
   - questions;
   - question type: short text, long text, single choice, multiple choice;
   - options for choice questions.
3. Optionally expand the schema into several supplier/customer/category-specific forms.
4. Create the first form in Microsoft Forms.
5. Use Microsoft Forms' own `Copy` action when several forms share the same structure.
6. Edit only the fields that differ between copies.
7. Verify every final form:
   - expected title;
   - expected section count;
   - expected question count;
   - expected options;
   - expected single-choice vs multiple-choice behavior;
   - saved state.

## Why not Quick Import?

Microsoft Forms Quick Import can be useful for simple documents, but it is not reliable enough for complex questionnaires. In particular, it may flatten predefined answers into free-text questions or fail to preserve single-choice versus multiple-choice intent.

## Repository Scope

This repository intentionally contains no customer data, private form links, real questionnaires, account identifiers, or production Microsoft Forms URLs.
