To do:
Add descriptions to appendix
Proofread
Upload files/assets.

# Global Layoffs Analysis (2020-2024)

This project analyzes global employee layoffs from 2020 to 2024, showcasing advanced **SQL** skills for data cleaning and exploration, complemented by **Power BI** visualizations for insights. The dataset includes over 1,500 companies across 31 industries, highlighting trends in layoffs by company, industry, geography, and time. The analysis contextualizes layoff patterns within global economic events and demonstrates technical proficiency in data analytics.

---

## Table of Contents
- [Objective](#objective)
- [Technical Workflow](#technical-workflow)
  - [Data Cleaning](#data-cleaning)
  - [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
  - [Visualisations](#visualisations)
- [Key Findings](#key-findings)
- [References](#references)

---

## Objective
The aim of this project is to analyze global layoffs, identify key trends, and provide actionable insights into the factors driving workforce reductions. Real-world events, such as the COVID-19 pandemic, economic inflation, and sector-specific corrections, are considered to contextualize the analysis.

---

## Technical Workflow

### Data Cleaning
SQL was used extensively to clean the raw dataset:
- **Removed Duplicates**:
  ```sql
  DELETE
  FROM layoffs_staging2
  WHERE row_num > 1;

- **Standardised Columns** (e.g., ensuring industries, countries, etc are consistent)
  ```sql
  UPDATE layoffs_staging2
  SET industry = 'Crypto'
  WHERE industry LIKE 'Crypto%';
  
  UPDATE layoffs_staging2
  SET country = TRIM(TRAILING '.' FROM country)
  WHERE country LIKE 'United States%';
  
- **Handled Null Values**
  ```sql
  UPDATE layoffs_staging2 t1
  JOIN layoffs_staging2 t2
  ON t1.company = t2.company
  SET t1.industry = t2.industry
  WHERE t1.industry IS NULL;

## Exploratory Data Analysis (EDA)
SQL queries uncovered trends by time, industry, and geography:

- **Temporal Trends**
  ```sql
  SELECT YEAR(`date`) AS Year, SUM(total_laid_off) AS Total_Laid_Off
  FROM layoffs_staging2
  GROUP BY Year
  ORDER BY Total_Laid_Off DESC;
  
Layoffs peaked in 2022-2023 (Appendix 1), driven by overhiring during the pandemic and subsequent economic corrections (**citation**)

Lower layoffs in 2020-2021 were due to pandemic relief efforts, while 2024 reflected market stabilisation (**citation**)

- **Industry and Company Insights**
  ```sql
    SELECT company, SUM(total_laid_off) AS Total_Laid_Off
    FROM layoffs_staging2
    GROUP BY company
    ORDER BY Total_Laid_Off DESC;

Retail and technology led layoffs, with Amazon and Meta among the top contributors (Appendix 2).

- **Geographic Trends**
    ```sql
    SELECT country, SUM(total_laid_off) AS Total_Laid_Off
    FROM layoffs_staging2
    GROUP BY country
    ORDER BY Total_Laid_Off DESC;

The United States and India experienced the largest layoffs, primarily in tech hubs (Appendix 3).

- **Funding and Layoffs**
    ```sql
    SELECT company, location, percentage_laid_off, funds_raised_millions
    FROM layoffs_staging2
    WHERE percentage_laid_off = 1
    ORDER BY funds_raised_millions DESC;
  
Several companies (e.g. Britishvolt and Deliveroo Australia) underwent complete layoffs despite significant funding (Appendix 4).

- **Monthly Rolling Totals**
    ```sql
    WITH Rolling_Total AS (
      SELECT SUBSTRING(`date`, 1, 7) AS Month, SUM(total_laid_off) AS Total_Laid_Off
      FROM layoffs_staging2
      GROUP BY Month
    )
    SELECT Month, Total_Laid_Off,
    SUM(Total_Laid_Off) OVER(ORDER BY Month) AS Rolling_Total
    FROM Rolling_Total;

Rolling totals provided a cumulative view of layoffs, highlighting key inflection points (Appendix 5).

*The data for the above findings can be found in the Appendices below, or .csv files stored under assets/eda.*

## Visualisations
The cleaned dataset was imported into Power BI to create a dashboard (available to download in this repository).
![PowerBI Visualisation Dashboard](assets/img/dashboard)

## Key Findings
**1. Temporal Trends:**
Layoffs spiked in 2022-2023 due to overhiring during the pandemic and subsequent economic corrections.
Layoffs were minimal in 2020-2021 due to pandemic relief efforts and market uncertainty, with recovery seen in 2024.

**2. Industry Impact:**
Retail and Technology were the most affected industries, with overexpansion during the pandemic leading to significant corrections.

**3. Geographic Insights:**
The United States and India were the most impacted countries, especially in tech sectors.

**4. Funding Patterns:**
Several companies with substantial funding underwent complete layoffs, reflecting poor financial planning.

## References
1. Lee, R. (2022) Layoffs Data 2022. Available at: https://www.kaggle.com/datasets/theakhilb/layoffs-data-2022 (Accessed: 21st Dec 2024).
2. *Citation above*
3. *Citation above*

## Appendices

### Appendix 1
| YEAR(`date`) | SUM(total_laid_off) |
|--------------|---------------------|
| 2023         | 212585             |
| 2022         | 150707             |
| 2024         | 77194              |
| 2020         | 70755              |
| 2021         | 15810              |

### Appendix 2
| industry       | SUM(total_laid_off) |
|----------------|---------------------|
| Retail         | 67368              |
| Consumer       | 63814              |
| Transportation | 57163              |
| Other          | 55864              |
| Food           | 42165              |
| Finance        | 37412              |
| Healthcare     | 23116              |
| Real Estate    | 18138              |
| Education      | 17730              |
| Travel         | 16197              |
| Sales          | 14948              |
| Infrastructure | 14729              |
| Hardware       | 13153              |
| Crypto         | 11235              |
| Fitness        | 9529               |
| Marketing      | 9120               |
| Security       | 8422               |
| Media          | 7941               |
| HR             | 7252               |
| Data           | 6343               |
| Logistics      | 5326               |
| Recruiting     | 4520               |
| Construction   | 3863               |
| Support        | 3491               |
| Energy         | 2867               |
| Product        | 1873               |
| Aerospace      | 1188               |
| Manufacturing  | 1066               |
| Legal          | 921                |
| AI             | 262                |
| Unknown        | 35                 |

### Appendix 3
| country              | SUM(total_laid_off) |
|----------------------|---------------------|
| United States        | 367630             |
| India                | 47127              |
| Germany              | 25345              |
| United Kingdom       | 16733              |
| Sweden               | 12969              |
| Canada               | 9677               |
| Brazil               | 8926               |
| China                | 6205               |
| Singapore            | 5229               |
| Israel               | 4816               |
| ...                  | ...                |
| Ukraine              | 50                 |

### Appendix 4
| Company               | Location       | Percentage Laid Off | Funds Raised (Millions) |
|-----------------------|----------------|---------------------|--------------------------|
| Britishvolt           | London         | 1                   | 2400                     |
| Deliveroo Australia   | Melbourne      | 1                   | 1700                     |
| Katerra               | SF Bay Area    | 1                   | 1600                     |
| Convoy                | Seattle        | 1                   | 1100                     |
| Cue Health            | San Diego      | 1                   | 899                      |
| Openpay               | Melbourne      | 1                   | 299                      |
| Ghost Autonomy        | SF Bay Area    | 1                   | 247                      |
| Zulily                | Seattle        | 1                   | 194                      |
| Simple Feast          | Copenhagen     | 1                   | 173                      |
| ZestMoney             | Bengaluru      | 1                   | 120                      |
| Drizly                | Boston         | 1                   | 119                      |
| Reali                 | SF Bay Area    | 1                   | 117                      |
| Bluprint              | Denver         | 1                   | 108                      |
| HOOQ                  | Singapore      | 1                   | 95                       |
| Phantom Auto          | SF Bay Area    | 1                   | 86                       |
| Milkrun               | Sydney         | 1                   | 86                       |
| Bitwise               | SF Bay Area    | 1                   | 84                       |
| Deliv                 | SF Bay Area    | 1                   | 80                       |
| Stay Alfred           | Spokane        | 1                   | 62                       |
| Hubba                 | Toronto        | 1                   | 61                       |
| Butler Hospitality    | New York City  | 1                   | 50                       |
| Bridge Connector      | Nashville      | 1                   | 45                       |
| Buy.com / Rakuten     | SF Bay Area    | 1                   | 42                       |
| Fifth Season          | Pittsburgh     | 1                   | 35                       |
| Frontdesk             | Milwaukee      | 1                   | 26                       |
| WanderJaunt           | SF Bay Area    | 1                   | 26                       |
| Amplero               | Seattle        | 1                   | 25                       |
| DeepVerge             | Dublin         | 1                   | 20                       |
| Pocketmath            | Singapore      | 1                   | 20                       |
| Metigy                | Sydney         | 1                   | 18                       |
| Soluto                | Tel Aviv       | 1                   | 18                       |
| BeyondMinds           | Tel Aviv       | 1                   | 16                       |
| Jump                  | New York City  | 1                   | 11                       |
| Playdots              | New York City  | 1                   | 10                       |
| Ahead                 | SF Bay Area    | 1                   | 9                        |
| SummerBio             | SF Bay Area    | 1                   | 7                        |
| Help.com              | Austin         | 1                   | 6                        |
| Planetly              | Berlin         | 1                   | 5                        |
| The Grommet           | Boston         | 1                   | 5                        |
| Consider.co           | SF Bay Area    | 1                   | 5                        |
| SEND                  | Sydney         | 1                   | 3                        |
| Dark                  | SF Bay Area    | 1                   | 3                        |
| Crejo.Fun             | Bengaluru      | 1                   | 3                        |
| Dropp                 | Berlin         | 1                   | 2                        |
| Dealtale              | Tel Aviv       | 1                   | 2                        |
| TutorMundi            | Sao Paulo      | 1                   | 2                        |
| Udayy                 | Gurugram       | 1                   | 2                        |
| Eatsy                 | Singapore      | 1                   | 1                        |
| Kitty Hawk            | SF Bay Area    | 1                   | 1                        |
| Glisser               | London         | 1                   | 1                        |
| Atsu                  | Seattle        | 1                   | 1                        |

### Appendix 5
| Month   | Total Off | Rolling Total |
|---------|-----------|---------------|
| 2020-03 | 8981      | 8981          |
| 2020-04 | 25271     | 34252         |
| 2020-05 | 22699     | 56951         |
| 2020-06 | 7046      | 63997         |
| 2020-07 | 2737      | 66734         |
| 2020-08 | 1969      | 68703         |
| 2020-09 | 609       | 69312         |
| 2020-10 | 450       | 69762         |
| 2020-11 | 219       | 69981         |
| 2020-12 | 774       | 70755         |
| 2021-01 | 6813      | 77568         |
| 2021-02 | 855       | 78423         |
| 2021-03 | 47        | 78470         |
| 2021-04 | 261       | 78731         |
| 2021-06 | 2434      | 81165         |
| 2021-07 | 80        | 81245         |
| 2021-08 | 1867      | 83112         |
| 2021-09 | 161       | 83273         |
| 2021-10 | 22        | 83295         |
| 2021-11 | 2070      | 85365         |
| 2021-12 | 1200      | 86565         |
| 2022-01 | 510       | 87075         |
| 2022-02 | 3685      | 90760         |
| 2022-03 | 5714      | 96474         |
| 2022-04 | 4078      | 100552        |
| 2022-05 | 12685     | 113237        |
| 2022-06 | 15934     | 129171        |
| 2022-07 | 15888     | 145059        |
| 2022-08 | 12539     | 157598        |
| 2022-09 | 5881      | 163479        |
| 2022-10 | 12706     | 176185        |
| 2022-11 | 52390     | 228575        |
| 2022-12 | 8697      | 237272        |
| 2023-01 | 70935     | 308207        |
| 2023-02 | 31010     | 339217        |
| 2023-03 | 34089     | 373306        |
| 2023-04 | 16124     | 389430        |
| 2023-05 | 10435     | 399865        |
| 2023-06 | 9551      | 409416        |
| 2023-07 | 6919      | 416335        |
| 2023-08 | 9432      | 425767        |
| 2023-09 | 4425      | 430192        |
| 2023-10 | 6199      | 436391        |
| 2023-11 | 7574      | 443965        |
| 2023-12 | 5892      | 449857        |
| 2024-01 | 32926     | 482783        |
| 2024-02 | 14739     | 497522        |
| 2024-03 | 1127      | 498649        |
| 2024-04 | 21973     | 520622        |
| 2024-05 | 5019      | 525641        |
| 2024-06 | 1410      | 527051        |
