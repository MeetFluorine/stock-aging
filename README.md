# Substore Aging – Auto Cleanup

A single-file HTML tool that turns the raw **Report_Substore_PAN_India_Aging** download into the cleaned Excel report, without the manual filtering and formatting.

**Live site:** http://meetfluorine.github.io/stock-aging

Everything runs in your browser. The raw file is never uploaded anywhere.

## How to use

1. Open the live site (or `Substore_Aging_Automation.html` locally) in Chrome or Edge.
2. Drop the raw `.zip` or `.xlsx` into the box. A large file (about 140 MB, 1.8M rows) takes a minute or two. A progress bar shows the stages.
3. Check the counts and the preview tabs.
4. Click **Download Excel**. The file is saved as `<raw file name>_Processed.xlsx`.

## What it does

| Step | Rule |
|---|---|
| 1. Consolidate | Reads every sheet and keeps rows where `substore_locator` contains `sdfx`, combined into one list |
| 2. Split blank SN | Rows with no `item_sno` go to **Sheet2** |
| 3. Remove duplicates | Rows with a repeated `item_sno` are removed, keeping the first row |
| 4. Date format | `stock_in_date` becomes a real Excel date shown as `dd-mm-yyyy h:mm`, for example `10-07-2026 17:50` |
| 5. Rename (serialized) | A description containing `iptv`, `modem outdoor`, `wifi router` or `ONT` becomes `Iptv`, `Modem Outdoor`, `Wifi Router` or `Ont` |
| 6. Rename (non-serialized) | Descriptions are renamed by `item_code` using the mapping box |

**Output workbook**

- **Sheet1:** rows with SN, after de-duplication and renaming.
- **Sheet2:** rows with blank SN.
- **Duplicates removed** (optional): tick the box before downloading to add it.

The check line under the counts confirms that Sheet1 + duplicates + blank SN equals the consolidated total.

## Editing the rules

Both boxes are on the left of the page and are remembered in your browser.

- **Serialized rules:** one per line, `pattern => New name`. The pattern is a case-insensitive regex and the first match wins. `\bont\b` matches the word ONT only, so words like "Front" are not touched.
- **Non-serialized mapping:** one per line, `item_code => New name`. To rename a new item code, add a line here.

Click **Apply rules** to re-run instantly. The raw file is not read again.

## Requirements

- A current Chrome or Edge. Firefox and Safari 16.4+ also work.
- Internet access once per session, to load the Excel library from cdnjs.
- A few GB of free RAM for the largest files.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Could not read the file" | Drop the original `.zip` or `.xlsx` from the download, not a re-saved copy |
| Download button is greyed out | The Excel library did not load. Check your internet and reload |
| A description was not renamed | Add or adjust the line in the rules box, then click **Apply rules** |
| Counts look different from your manual result | Compare the check line first, then the Duplicates removed sheet |

## Files

- `Substore_Aging_Automation.html`: the tool (one file, no install).
- `README.md`: this guide.
