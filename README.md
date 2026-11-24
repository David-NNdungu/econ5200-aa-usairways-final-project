# American–US Airways Merger: Proposal and EDA

## Research Question

Did the American Airlines–US Airways merger increase fares on routes where the two airlines previously competed?

I propose a difference-in-differences design comparing changes on overlap routes with changes on routes where neither airline appeared before the merger.

## Client and Decision

My proposed client is the DOJ Antitrust Division. The project would help assess whether overlap routes warrant closer fare monitoring and review of entry conditions.

For this project, I use a sustained, credibly identified 5% fare increase as a proposed threshold for closer review. This is a project decision rule, not a legal standard.

## Data

I use a previously prepared panel from the Bureau of Transportation Statistics’ DB1B Market survey.

- **Period:** 2011–2012 and 2014–2016.
- **Transition year excluded:** 2013.
- **Input:** 140,026 route-carrier-quarter observations.
- **Route-quarter observations before comparison restrictions:** 45,065.
- **EDA sample:** 25,737 route-quarter observations across 1,653 routes.

The file `airline_panel.csv` is an export of my existing collapsed dataset. The notebook performs the EDA in Python; Stata is not required.

Source: [BTS DB1B](https://www.transtats.bts.gov/DatabaseInfo.asp?QO_VQ=EFI&Yv0x=D).

## Treatment and Comparison Groups

I define treatment using pre-merger information only.

- **Treated:** 418 routes where AA and US carrier codes appeared in the same quarter at least once during 2011–2012.
- **Comparison:** 1,235 routes where neither code appeared during 2011–2012.

I exclude other routes from this comparison, including routes first observed after 2012. Carrier affiliations and operating-versus-marketing carrier definitions still need further checking.

## Preliminary Fare Measure

The saved passenger field was produced using analytic-weighted sums, so I cannot treat it as a raw passenger count.

For this preliminary EDA, I calculate an equal-weight average of the stored carrier fares within each route-quarter. I label this measure `fare_proxy`. It is not a passenger-weighted average fare paid by travelers.

Before causal estimation, I need to rebuild passenger totals and fare numerators from the original records. Using equal carrier weights here does not undo earlier aggregation or sample-selection issues.

## Preliminary Findings

Before the merger, the fare proxy averages:

| Group | Mean fare proxy |
|---|---:|
| Overlap routes | $218.26 |
| Comparison routes | $186.87 |
| Difference | $31.39 |

The groups’ 2011–2012 difference in log fare growth is 2.73 log-percentage points. Restricting the calculation to routes observed in all eight pre-merger quarters gives 1.65 log-percentage points.

These are descriptive comparisons, not causal estimates or formal parallel-trends tests. The differences in fares, distance, carrier counts and pre-period growth need to be addressed before interpreting a merger effect.

## Identification Strategy

I propose a common-date difference-in-differences model with route and year-quarter fixed effects.

The key assumption is that, without the merger, fares on overlap and comparison routes would have followed similar trends. I will assess pre-merger comparability and examine pre-treatment event-study coefficients.

My main concern is stronger demand growth on overlap routes. If this raises their fares independently of the merger, it would bias the estimated merger effect upward.

The design evaluates the merger together with its remedies. It cannot separately identify what would have happened without the divestitures.

## Files

| File | Contents |
|---|---|
| `Airline_Merger_Proposal.pdf` | Two-page proposal |
| `Airline_Merger_EDA.ipynb` | Python EDA, tables, figure and interpretation |
| `airline_panel.csv` | Input dataset |
| `README.md` | Project overview and running instructions |

## Run in Google Colab

1. Open `Airline_Merger_EDA.ipynb` in Colab.
2. Connect to a Python runtime.
3. Select **Runtime → Run all**.
4. When prompted, upload `airline_panel.csv`.

The setup cell installs missing packages if needed. No API key or Stata installation is required.

## Run in Jupyter

Place `airline_panel.csv` beside the notebook.

Install the required packages:

```bash
python -m pip install pandas numpy matplotlib ipython jupyter
```

Open the notebook and run all cells. Python 3.12 was tested.

The notebook saves generated tables, its figure and a summary in `eda_outputs/`. These outputs do not need to be uploaded separately because the notebook regenerates them.

## Next Steps

- Rebuild passenger totals and passenger-weighted fares from the original records.
- Check carrier affiliations and the overlap definition.
- Assess common support using pre-merger characteristics.
- Examine pre-treatment trends and alternative comparison groups.
- Report uncertainty and sensitivity before making a causal recommendation.

## Assistance

### 2.12 What I will do next
1. Rebuild passenger totals and fare numerators from the raw ticket data in Python; audit the traffic-floor rule and operating-versus-marketing carrier mapping.
2. Fix the overlap definition and comparison rules before fitting the final treatment model. Use baseline route characteristics to assess common support; do not use post-merger characteristics for matching.
3. Plot and test pre-treatment event-study leads, report all results, and consider a longer pre-period. A non-rejection will not prove parallel trends.
4. Report sensitivity to fare construction, balanced panels, overlap persistence and comparison definitions. I will not interpret a statistically insignificant estimate as proof that the merger caused no harm.
5. Keep the merger and the remedies together as the treatment package. This design cannot separately identify what would have happened without the divestitures.

**Milestone status:** Part 2 EDA is completed on the supplied collapsed panel. Causal estimation, the dashboard and the final recommendation belong to later milestones.

**Sources**
- BTS DB1B: https://www.transtats.bts.gov/DatabaseInfo.asp?QO_VQ=EFI&Yv0x=D
- DOJ settlement: https://www.justice.gov/archives/opa/pr/justice-department-statement-us-district-court-finding-department-s-settlement-us
- Stata weighted collapse definitions: https://www.stata.com/manuals/dcollapse.pdf
- Course starter: https://github.com/RyanPiao/econ5200-student-notebooks/blob/main/5200/final-project-starter.ipynb

**Assistance:** 
I used ChatGPT to help review my code after writing it myself and debugging where I got stuck. The notebook distinguishes the calculations completed here from the causal analysis planned for later milestones.
