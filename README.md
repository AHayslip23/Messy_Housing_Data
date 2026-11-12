For further reference, this project is designed to test my ability to clean a messy dataset and to teach someone without a data science background how to visualize it in Power BI.

raw dataset - messy_housing.csv
python data cleaning pipeline - data cleaning for Messy Housing Data.ipynb
cleaned dataset - cleaned_housing_data.csv
Power BI dashboard - Messy_Housing_Data_Dashboard.pbix

## Changelog

Changes are listed oldest to newest, by commit ID.

### `0bc85d1`: Initial commit (2026-06-16)
**Added**
- `.gitattributes` to set up the repository.

### `e165dd3`: Commit #1, data import and cleaning (2026-06-16)
**Added**
- `messy_housing_data.csv`: raw dataset (420 homes; columns `area_sqft`, `bedrooms`, `bathrooms`, `year_built`, `price`, `location`).
- `Data Cleaning for Messy Housing Data.ipynb`: Python cleaning pipeline that:
  - **Profiles missing data:** null counts and percentages per column (`price` 11.9%, `year_built` 8.6%, `bedrooms` 5.0%).
  - **Tests missingness:**
    - whether nulls are randomly spread or clustered in certain rows
    - whether nulls in one column are connected to nulls in another
  - **Fixes types:** converts `price`, `year_built` and `bedrooms` to numbers (text values like `"unavailable"` in `price` become nulls), then casts them to int/float.
  - **Imputes missing values:**
    - `price`: median
    - `year_built`: median
    - `bedrooms`: mode
  - **Standardizes `location`:** trims and lowercases the text, then maps variants and typos to 7 clean city names (e.g. `sf` → San Francisco, `bostan` → Boston, `chicgo` → Chicago, `l.a.`/`la` → Los Angeles, `nyc` → New York City).
  - **Engineers features:**
    - `Historical_Period`: era the home was built in, from Pre-WWI onward
    - `Age_of_House`, `Price_Per_Sqft`, `Total_Rooms` and `Decade_Built`
    - `size_category`: Small / Medium / Large / Luxury
    - `price_category`: Budget / Mid-Range / Premium / Luxury
    - `Region`: West Coast / East Coast / Midwest / South
    - `Home_Classification`: Modern / Established / Vintage / Historic
  - Drops the helper column `total_nulls_in_row`.

### `c3c1b51`: Commit #2, export cleaned data and add README (2026-06-16)
**Added**
- An export step at the end of the notebook: `clean_data.to_csv("cleaned_housing_data.csv")`.
- `cleaned_housing_data.csv`: cleaned dataset (420 rows, 15 columns).
- `README.md`.

### `881e2fe`: Commit #3, data pipeline change (2026-06-17)
**Changed**
- `bedrooms` imputation switched from **mode** to **median**. Rows with a missing bedroom count changed from 1 bedroom to 3.
- Rows with a missing `year_built` are now **dropped** instead of filled with the median.

**Result**
- `cleaned_housing_data.csv` went from **420 to 384 rows**, because the 36 homes with a missing `year_built` were removed.

> The commit message notes this change introduced a mistake in the pipeline. Dropping the 36 rows removes about 8.6% of the dataset rather than imputing the missing value as in Commit #1.

### `c4c3d36`: README edit (2026-10-29)
**Changed**
- Removed a personal note from the project description in `README.md`.

### `9f27213`: Commit #4, Power BI dashboard (2026-11-11)
**Added**
- `Messy_Housing_Data_Dashboard.pbix`: Power BI dashboard built on the cleaned dataset.

**Changed**
- Notebook metadata only: the Python version recorded in the notebook changed from 3.12.13 to 3.12.16. No code changes.
