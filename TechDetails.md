# YAML/JINJA Notes 
### General

**ONE YAML FILE** → **Prompt + Template + Model**
> fields.yaml  →  prompt.txt (for llm) + base.yaml (for drafter) + model.py (pydantic model)

Each "0-module" should have its own YAML file with fields that are module-specific. These additional YAML files are merged into the base YAML file to create the final set of fields for that module.
> Only 0.2 has been completed so far (stone extraction)

`insert_after` in the metadata section indicates where the module's fields should be inserted into the base template. e.g. `insert_after: cholangiogram` means the module's fields will be inserted after the "cholangiogram" section in the base template.

### Field Groups + Definitions
```yaml
field_groups:
  my_section:
    report_section: "section_name"
    report_subsection: "subsection_name"  # for overrides; these must be unique
    template: "Text with {{ field_name }}."
    fields:
      - name: field_name
        type: string              # string, boolean, integer, enum. USUALLY STRING BECAUSE THIS IS MOST PERMISSIVE
        prompt_instruction: "..." # instructions for LLM
```

### Fields and Prompt Instructions
Fields usually correspond to the individual pieces of information that have multiple options in the Word Doc templates. 

- Fields are usually specific, e.g. `pd_stent_placed`, `pd_stent_size_cm`, `pd_stent_length_cm`, `pd_stent_type`. Each field captures one piece of information, and this will allow for automatic evaluation in the future.
- However, a field can be more general and instruct the llm to generate a longer (1-2ish sentences) "narrative" text that covers multiple aspects (e.g. `pd_stent_therapy_narrative`). This is appropriate when the information is often presented together in the transcript and we don't need to separately capture each piece of information (ie. for use elsewhere).

Ideally, the prompt instruction for each field is short but specific. 

The system instruction (specified in fields_base.yaml) already provide overall guidelines, e.g. handling unknowns. These do not need to be repeated in each field's prompt instruction.


### Template Patterns
```yaml
# Skip unknown/none
{{ field | sku }}

# Conditional section - skips section if field is unknown/none
{% if field | sku %}Text here.{% endif %}

# Multi-choice
{% if status == 'normal' %}
  Normal.
{% elif status == 'abnormal' %}
  Abnormal: {{ findings }}.
{% endif %}

# Set variable
{% set var = field | default('') | sku %}

# Make a paragraph without newlines (note: run replace('  ', ' ') repeatedly to catch double spaces)
{% set _section_name %}
text
more text
{% endset %}
{{ _section_name | replace('\n', ' ') | replace('  ', ' ')  | replace('  ', ' ') | trim}}
```

### Jinja Filters

| Filter | Use | Example |
|--------|-----|---------|
| `sku` | Skip "unknown"/"none" |
| `sent` | Normalize spacing, capitalize first word (unless begins with a number), and ensure a trailing period | {{ field | sent }}
| `capfirst` | Capitalize first letter |
| `default_if_unknown` | Fallback value |
| `default_if_unknown_sentence` | Fallback as sentence, fallback presented as a properly capitalized sentence with punctuation. |
| `join_nonempty` | Join list, skip empty |
