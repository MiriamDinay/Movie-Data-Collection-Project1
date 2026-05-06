# Movie Data Collection

A project for the courses **Advanced Data Mining and Analysis in Python** and **Machine Learning**.

## Team Members

| Name | ID |
|---|---|
| Miriam Dinay | 212296099 |
| Hila Halberstadt | 032540668 |

## Collection Method

We filtered the data to movies released up to 2024 with a runtime between 60 and 300 minutes. We then filtered to movies starting with the letter **L** (per our assigned letter), and finally selected **5,000 movies** with the highest number of votes among those with no missing values in any column.

The data was collected from two sources:

- **IMDb Non-Commercial Datasets** — Public TSV files (`title.basics`, `title.ratings`, `title.principals`, `name.basics`) loaded with `pandas`. Source for: `tconst`, `primaryTitle`, `startYear`, `genres`, `runtimeMinutes`, `averageRating`, `numVotes`, `lead_actors_ids`.
- **Wikipedia** — Web scraping of movie pages using `requests` and `BeautifulSoup`, with a local cache. Source for: `Language`, `Country`, `budget`, `BoxOffice`, `plot`.

> For full details on decisions, methodology and missing values — see `report.pdf`.

## How to Run

### 1. Requirements

- Python 3.8+
- Jupyter Notebook

### 2. Install Dependencies

```bash
pip install pandas requests beautifulsoup4 tqdm CurrencyConverter
```

### 3. Download IMDb Files

Download the following four files from https://datasets.imdbws.com/, extract them, and place them in the same folder as the notebook:

- `title.basics.tsv`
- `title.ratings.tsv`
- `title.principals.tsv`
- `name.basics.tsv`

### 4. Run

```bash
jupyter notebook Movie_Data_Collection_-_Part_1.ipynb
```

Run the cells in order. On completion, `dataset.csv` will be created with 5,000 movies.
