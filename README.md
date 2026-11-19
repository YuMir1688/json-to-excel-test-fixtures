# json-to-excel-test-fixtures

Sample nested JSON, UTF-8 BOM CSV, and multi-line statement fixtures for testing client-side Excel (`.xlsx` / BIFF8 `.xls`) converters.

## Nested Orders JSON Fixture

Client-side spreadsheet exporters must decide between **dot-notation flattening** (preserving one row per order, e.g. `customer.name`, `customer.tier`) and **row-expansion** (unwinding `line_items` into normalized child rows while repeating parent keys):

```json
[
  {
    "order_id": "ORD-1042",
    "customer": { "name": "田中 太郎", "tier": "Enterprise", "country": "JP" },
    "line_items": [
      { "sku": "XLS-PRO", "qty": 2, "unit_price": 49.0 },
      { "sku": "CSV-ADDON", "qty": 1, "unit_price": 19.0 }
    ]
  }
]
```

When targeting legacy **BIFF8 `.xls`** workbooks instead of OOXML `.xlsx`, row-expansion pipelines must guard against the **65,536-row** (`65,536` rows × `256` columns) sheet ceiling by splitting overflow batches across worksheets or warning before truncation.

## UTF-8 BOM (`EF BB BF`) for Desktop Excel CSVs

Desktop Microsoft Excel on Windows assumes a local ANSI code page when opening raw UTF-8 `.csv` files, corrupting CJK text (such as `田中 太郎`) and currency symbols. Prepending the three-byte UTF-8 Byte Order Mark (`0xEF 0xBB 0xBF` / `\uFEFF`) guarantees clean decoding on double-click without manual import wizards.

## Browser Test Workbenches

- [JSON to XLS & XLSX Converter](https://jsontosheet.com/) — Inspect dot-notation flattening, nested array expansion, and BIFF8 `.xls` vs `.xlsx` output directly in the browser.
- [PDF Bank Statement to Excel](https://jsontosheet.com/pdf-bank-statement-to-excel) — Test multi-line transaction wrapping and tabular column extraction.
- [Receipt to Excel](https://jsontosheet.com/receipt-to-excel) — Verify line-item parsing from structured expense OCR outputs.

## License

MIT
