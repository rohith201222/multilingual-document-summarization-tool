# Knowledge Base & Prompt Library
**Project:** Multilingual Document Summarization Tool (n8n Automation Pipeline)

---

## 1. Language Detection Prompt
**Node:** Basic LLM Chain (`Language Detection`)  
**Purpose:** Identifies the primary language of the extracted PDF text so downstream nodes handle formatting accurately.

```text
Detect the primary language of the document.
Return only the language name.
Document content:
{{ $json.text }}
```

---

## 2. Core Summarization Prompt
**Node:** Basic LLM Chain (`Summarization`)  
**Purpose:** Produces a clean, non-redundant, multi-paragraph summary in the detected source language.

```text
You are a multilingual expert summarizer.
Summarize the content below into a concise, accurate multi-paragraph summary in the below language.
Remove redundancies and highlight the most important points.

Document:
{{ $('Extract from File').item.json.text }}

Language:
{{ $json.text }}

Return only the summary.
```

---

## 3. Multilingual Translation Prompts
**Nodes:** Basic LLM Chains (`English Translation`, `Hindi Translation`, `Spanish Translation`, `French Translation`, `German Translation`, `Japanese Translation`)

### English Translation
```text
Translate the summary below into ENGLISH.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

### Hindi Translation
```text
Translate the summary below into Hindi.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

### Spanish Translation
```text
Translate the summary below into Spanish.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

### French Translation
```text
Translate the summary below into French.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

### German Translation
```text
Translate the summary below into German.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

### Japanese Translation
```text
Translate the summary below into Japanese.
Ensure the translation is accurate, professional, and maintains the original meaning.

Summary:
{{ $('Summarization').item.json.text }}
```

---

## 4. Quality Control & Enhancement Instructions
To maintain high output accuracy across global teams:
1. **Noise Removal:** Strip recurring headers, page numbers, and legal footers prior to passing text into the Summarization chain.
2. **Terminology Consistency:** Keep proper nouns, product names, and financial figures untranslated or standardized across all target languages.
3. **LLM Quality Check Agent (Optional Extension):** Append a verification LLM node comparing the English back-translation against the source summary to flag omissions or hallucinations before Google Docs export.
