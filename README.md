# Medicare Part B Billing Outliers and OIG Exclusions

Can payment and service-intensity screens identify different sets of providers for review at the same workload? This team project compares national and specialty-relative screens using 2024 Medicare Part B provider summaries and a snapshot of the HHS Office of Inspector General's List of Excluded Individuals/Entities (LEIE).

[Open the analysis notebook](B01-Medicare-Fraud-Detection.ipynb)

## Findings

| Question | Result |
| --- | --- |
| How concentrated are payments? | The top 10% of providers received 66.5% of observed payments. |
| Do specialty-relative payment and service-intensity flags overlap? | Of 755,745 eligible providers across 93 specialties, 2.71% were flagged by each measure alone and 2.30% by both. Among providers with at least one flag, 29.8% had both. |
| Does either screen capture more OIG matches at the same workload? | At 4,269 providers per review list, the national payment screen, national service-intensity screen, and specialty-relative intensity screen included 1, 0, and 1 OIG matches, respectively. These counts are too small to rank the rules. |
| How do exclusions line up with the billing year? | For 142 of 157 matched providers, the earliest recorded exclusion began after 2024. |

## Method

- Standardize National Provider Identifiers and collapse multiple LEIE records per valid NPI before joining to the CMS provider table.
- Calculate payments and services per beneficiary. The per-beneficiary analyses use a minimum of 100 beneficiaries, with additional eligibility checks that vary by chart.
- Compare national thresholds, within-specialty percentiles, and peer medians within specialty and broad patient-risk groups. Hold review workload constant when comparing screen coverage.

The results describe review workload and overlap with a historical exclusion snapshot. Exclusions cover multiple grounds, and many matches postdate the billing year. An annual provider summary cannot establish claim-level impropriety or determine whether services were furnished during an exclusion. An unmatched provider has no record linked in this analysis. Peer comparisons account for some observed differences while leaving room for service mix, coding, and patient complexity.

## Team and contribution

Ahan Nair managed the project and built the specialty-relative service-intensity comparison, the peer-median payment comparison, and the equal-workload screening comparison (charts 11, 13, and 14). The team also included Sree Dandu, Alice Yen, Yuxin Li, and Ujjwala Kapoor.

## Reproduce

Place both CSVs in the notebook's working directory.

| Expected filename | Source |
| --- | --- |
| `Medicare_Physician_Other_Practitioners_by_Provider_2024.csv` | [CMS Medicare Physician & Other Practitioners by Provider](https://data.cms.gov/provider-summary-by-type-of-service/medicare-physician-other-practitioners/medicare-physician-other-practitioners-by-provider), year 2024 |
| `LEIE.csv` | [HHS OIG LEIE downloads](https://oig.hhs.gov/exclusions/exclusions-list/) |

```bash
python -m pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook
```

Open `B01-Medicare-Fraud-Detection.ipynb` and run all cells. LEIE is updated over time, so a later download can change match counts.
