# Clean_AirBnB_Dataset

Data cleaning notebook for a raw Airbnb listings dataset in the San Francisco area, that was initially scraped from the Airbnb website (listings.csv). The notebook walks through inspecting, cleaning, and standardizing the messy scraped data into an analysis-ready dataset.

**Author:** Gracy Sutaria

## Objective

This notebook explores the Airbnb dataset and focuses on cleaning up messy, scraped data, including missing values, incorrectly scraped entries, inconsistent types, and redundant columns, to produce a dataset ready for further analysis and modeling.

## Notebook Structure

### A. Preprocessing
- **A.1** Import Python libraries (`pandas`, `numpy`, `seaborn`, `matplotlib`)
- **A.2** Load the raw CSV file
- **A.3** Display overall data structure

### B. Exploratory Data Analysis & Cleaning
- **B.1** Function to identify columns with missing values
- **B.2** Remove columns that are entirely missing (dropped columns with all 7,332 values null — reduced from 90 to 77 columns)
- **B.3** Check for duplicate rows (none found)
- **B.4–B.5** Identify incorrectly scraped values using a custom function for inspecting unique values per column
- **B.6** Remove accidentally scraped HTML/markdown artifacts (e.g. stray `<br /><br />` tags and `**` in the `description` field)
- **B.7** Convert columns to proper types (e.g. years and other trailing-zero floats → integers); clean the `about` field of stray `\r\n` characters and placeholder junk values (e.g. `.`, `..`, `"This field is required."`) by converting them to `NaN`; map boolean-style fields to `0`/`1`
- **B.8** Type casting for URLs, host response fields, and additional boolean-style columns converted to `0`/`1`; replace empty list values (`[]`) with `NaN`
- **B.9** Identify and fix invalid values: e.g. strip `$` signs from price fields and cast to float; drop redundant min/max nightly-stay columns in favor of the `_avg_ntm` equivalents (which have no missing values)
- **B.10** Identify and drop redundant/highly-correlated columns (e.g. `number_of_reviews_l30d` and `number_of_reviews_ly`, correlated sub-rating scores, and `host_listings_count`, which is self-reported and inconsistent with the scraped `calculated_host_listings_count`)

### C. Summary
- **Original shape:** (7332, 90)
- **Final shape:** (7332, 65)

### D. Export
- Cleaned dataset exported to `airbnb_cleaned.json` using `df.to_json(orient='table', indent=2)`

## Requirements

```
pandas
numpy
seaborn
matplotlib
beautifulsoup4
```

## Usage

1. Place the raw Airbnb CSV file in the project directory.
2. Open `CleanMessyData.ipynb` in Jupyter.
3. Run all cells in order — later steps depend on transformations made earlier in the notebook.
4. The cleaned dataset will be saved as `airbnb_cleaned.json`.

## Output

- **File:** `airbnb_cleaned.json`
- **Format:** JSON (`orient='table'`), which includes a schema block describing each column's data type alongside the cleaned records.

## Notes

- This dataset is specific to San Francisco listings, so latitude/longitude values are expected to cluster tightly around that area.
- Some columns with unique identifiers (e.g. `host_profile_id`) were intentionally left with null values unmodified, since IDs shouldn't be altered or imputed.
