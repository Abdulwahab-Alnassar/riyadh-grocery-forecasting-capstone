# Dataset provenance

- Source author/repository: MohammadYusif/time-series-forecasting-ai-systems.
- Immutable source revision: `0acb110628250c5efec9dd2b3d5c0b46401ebdee`.
- Original file: `data/retail_demand.csv` (6,576 rows; six series).
- Source URL: https://raw.githubusercontent.com/MohammadYusif/time-series-forecasting-ai-systems/0acb110628250c5efec9dd2b3d5c0b46401ebdee/data/retail_demand.csv
- Original CSV SHA-256: `08738f5573c1b85c5792ffc18f4308a91666e8bdb1a13e86f8fe898fa1f85362`.
- Selection: `region == "Riyadh" and category == "Grocery"` (1,096 rows).
- Local subset SHA-256: `b19ad57eebc2ee88fc1508d8bc5312a1346a16d1166ead5b91ad0bfe7a8347d3`.
- Changes: filter one series and serialize the original four columns to CSV with LF newlines.
  No values were imputed, rescaled, winsorized, regenerated, or removed from this series.
- The notebook embeds this same subset as gzip/base64 so the notebook is sufficient by itself.
- Snapshot inspected for this project on 2026-09-15.
- Data are synthetic educational data, not observations from a real retailer.
- No promotion or holiday flag is available in the supplied CSV; none is fabricated.

The upstream data and course materials remain attributed to their original author.
No ownership or additional license over upstream materials is claimed.
