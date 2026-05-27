# HardMTBench Data Format

## File

- **HardMTBench.jsonl** — One JSON object per line, 20,000 records in total (10,000 ZH→EN + 10,000 EN→ZH).

## Field Description

| Field | Type | Description |
|-------|------|-------------|
| `source_text` | `string` | The original text to be translated (source side). |
| `reference` | `string` | Human-verified reference translation for the source text. |
| `id` | `string` | Unique identifier for each test item. Format: `{pair_md5}_{direction}`, e.g. `abc123_zh2en`. Items sharing the same `pair_md5` prefix are bidirectional counterparts. |
| `source_language` | `string` | Language of `source_text`. Values: `"Chinese"` or `"English"`. |
| `target_language` | `string` | Language of `reference` / expected translation output. Values: `"Chinese"` or `"English"`. |
| `domain` | `string` | Top-level domain category (12 domains in total, in English). E.g. `"Finance"`, `"Medical"`, `"Legal"`. |
| `sub_domain` | `string` | Fine-grained sub-domain label (in English). E.g. `"Fiscal Policy / Local Government Debt"`, `"Capital Markets / Financing"`. |
| `domain_keywords` | `list[string]` | Key domain-specific terms/phrases extracted from the source text. |
| `terminology` | `list[object]` | Bilingual terminology pairs with category. Each object contains: |
| | | - `source` (`string`): term in source language |
| | | - `target` (`string`): term in target language |
| | | - `category` (`string`): terminology category label |
| `scores` | `object` | LLM-annotated quality and difficulty scores. Contains: |
| | | - `domain_knowledge_density` (`int`, 0–100): how dense the domain knowledge is |
| | | - `translation_difficulty` (`int`, 0–100): overall translation difficulty |
| | | - `translation_correctness` (`int`, 0–100): correctness of the reference translation |
| | | - `term_density` (`float`, 0–100): density of domain terminology |
| | | - `hardness` (`float`, 0–100): composite hardness score, computed as `H = 0.4×D + 0.4×F + 0.2×T` where D = domain_knowledge_density, F = translation_difficulty, T = term_density |

## Domain Coverage (12 domains)

| Domain | Records |
|--------|---------|
| Finance | ~1,666 |
| Medical | ~1,666 |
| Legal | ~1,666 |
| Technology | ~1,668 |
| Gaming | ~1,666 |
| History | ~1,668 |
| Media & Entertainment | ~1,668 |
| News | ~1,668 |
| Papers & Books | ~1,666 |
| Education | ~1,666 |
| Sports | ~1,666 |
| Military | ~1,666 |

## Example

```json
{
  "source_text": "加上此前地方平台大量使用私募等"表外"募资途径融资...",
  "reference": "In addition, the extensive use of off-balance-sheet financing channels...",
  "id": "c261cc10a2336fe64093373226f91284_zh2en",
  "source_language": "Chinese",
  "target_language": "English",
  "domain": "Finance",
  "sub_domain": "Fiscal Policy / Local Government Debt",
  "domain_keywords": ["私募", "表外", "隐性债务", "去杠杆", "城投公司", "借新还旧"],
  "terminology": [
    {"source": "私募", "target": "private placements", "category": "金融术语"},
    {"source": "隐性债务", "target": "implicit debt", "category": "金融术语"}
  ],
  "scores": {
    "domain_knowledge_density": 85,
    "translation_difficulty": 80,
    "translation_correctness": 95,
    "term_density": 100.0,
    "hardness": 86.0
  }
}
```
