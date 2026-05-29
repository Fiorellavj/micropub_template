# Response to Reviewers

We thank both reviewers for their thorough and constructive feedback. Their comments have significantly improved the clarity, reproducibility, and scientific rigor of our micropublication. Below we address each comment in turn.

In this revision we focused on three areas: (1) reproducibility of the full pipeline from source data to figure, (2) alignment of the text with the code and methodological precision, and (3) figure accessibility. We did not modify the underlying analysis in this round; aspects that require analytical changes (sensitivity tests, trend analysis, dependence testing) are noted as planned for a future journal submission extending this work.

---

## Reviewer 1: Ivan Sudakow

### Comment 1: Reproducibility — pipeline completeness

> The repository contains a reproduce.sh pipeline that runs regridding, data combination, SPEI calculation, heatwave detection, flash-drought detection, and compound-event detection, but it does not appear to finish by running the figure-generation script. The separate compose_figure.py script saves the composite figure, but this step is not included in reproduce.sh. Therefore, as currently documented, the code does not clearly regenerate the figure required for the micropublication.

**Response:** Thank you for taking the time to inspect the scripts and identify this gap. The figure-generation step is now included as the final step in both `reproduce.sh` (containerized, Linux) and `reproduce-no-container.sh` (conda/uv, Windows/Linux). As of the date of this response, we have successfully completed three full end-to-end runs of the pipeline: on Windows without containers, on Linux without containers, and on Linux with Apptainer containers. The README now documents the complete reproduction process explicitly, including data access, environment setup, and the single command that regenerates all outputs including the figure.

### Comment 2: Import-time side effect in constants.py

> There is also a likely clean-run problem in the code structure. The reproduce.sh pipeline begins with regrid.py, but regrid.py imports constants.py; constants.py opens the derived masked dataset at import time. That derived dataset is only created later in the pipeline, so a fresh run from scratch is likely to fail unless the derivative file already exists. This can be fixed cleanly by removing file-loading side effects from constants.py and moving the mask_peru calculation into the plotting script or into a function that is called only after the derivative dataset has been created.

**Response:** Thank you for catching this. We have removed the file-loading side effect from `constants.py`. The `load_mask_peru()` function is now only called at the point of use (in plotting code that runs after all derivatives exist), not at import time. The pipeline now runs cleanly from a fresh state.

### Comment 3: Data-access instructions

> The data-access instructions also need to be clearer. The code expects local sourcedata and derivatives paths, including precipitation, temperature, evapotranspiration, and shapefiles, but I did not see a clear README explaining how a reviewer should obtain the data, initialize DataLad, fetch required files, set up the container, and run the full pipeline. A short README would be enough: list the required input datasets, explain how to download or access them, give environment/container setup instructions, then provide one command sequence that regenerates the figure.

**Response:** We have substantially expanded the README to include: (1) instructions for installing DataLad and its dependencies, (2) how to clone the dataset and fetch source data, (3) environment and container setup for all three supported platforms, and (4) a single command sequence that regenerates the figure from source data. We hope the updated documentation now makes reproduction straightforward for reviewers and future users. Please note that the incompleteness of guidance (as well as access to source data) in the first round of submission was largely due to a missing placeholder in the submission airtable. We only shared the repository for code but not for the entire project which is hosted on GIN: https://gin.g-node.org/Las-Ninas/CFDHW_Peru. We have indicated the source of each dataset, and also made clear that all source data are hosted on GIN as part of the project repository.

### Comment 4: Methodological alignment between text and code

> Methodologically, the event definitions are good, but the text should align more tightly with the code. The code makes clear that heatwaves are detected using a joint Tmax and Tmin criterion, where both must exceed the corresponding 90th percentile, with the default detection based on 5-day rolling means. This is important and should be stated explicitly in the paper. The flash-drought method should also clarify whether "daily pentad-SPEI" means a rolling five-day SPEI computed daily. The code maps SPEI to percentiles and then detects events using threshold-crossing rules; this is sensible, but the paper should describe the implementation at the same level of clarity.

**Response:** We have revised the methods section to explicitly state that heatwave detection uses the joint criterion: "both must exceed the corresponding 90th percentile of the reference period (1981–2010), with the default detection based on 5-day rolling means, and such an event must last for at least 3 consecutive days." We have also clarified that pentad-SPEI is "estimated daily instead of every five days" to ensure comparability with the smoothed daily temperature data.

### Comment 5: Statistical interpretation of trends

> The statistical interpretation should be softened or supported. The spatial results are descriptive and are well supported by the maps. However, statements about an "increasing trend" in flash drought severity are not fully supported unless a trend test is reported. If there is no space or time to add a formal Mann–Kendall, Sen slope, or regression test, the authors should simply write that the pattern "suggests" or "is visually consistent with" increasing severity, rather than presenting it as a confirmed trend.

**Response:** We have softened the language. The revised text now reads: "a visible positive long-term trend suggesting increasing [severity] mainly since the 2000s," rather than asserting a confirmed trend. A formal trend analysis (e.g., Mann-Kendall test) is planned for the extended journal version of this work.

### Comment 6: Plot accessibility

> The plots are effective but should be made more accessible. Some map labels and color bars are small, and the red/brown/purple palette may be difficult for some readers. I recommend increasing font sizes where possible, ensuring color-blind-friendly contrast, and making the annual severity panel easier to interpret. In particular, panel (b) should state clearly that it shows annual maximum severity and that the flash-drought y-axis is inverted because the severity values are negative.

**Response:** We have made the following changes to the figure:

- **Color-blind-friendly palette:** Switched from the red/brown/purple scheme to the Okabe-Ito palette (orange, blue, reddish-purple) for line and shading colors, and to perceptually distinct sequential colormaps (YlOrBr, YlGnBu, Purples) for the maps. These are distinguishable under all common forms of color vision deficiency.
- **Larger font sizes:** All text elements (axis labels, tick labels, legends, colorbar labels, panel titles) have been increased by approximately 2 points throughout.
- **Layout adjustments:** Margins and spacing have been adjusted to prevent overlap between panel labels, titles, and plot content.
- **Caption clarification:** The figure caption now states that panel (b) shows "Annual maximum severity" and explicitly explains: "flash droughts (middle, brown; y-axis inverted because the severity values are negative)."

### Minor comments

> Change "3. Result" to "3. Results." Remove the placeholder "(add ref.)" in the conclusion. Fix the duplicated punctuation in the Figure 1 caption: "events. ." Replace "El-Niño events assert contrasting hydroclimatic effects" with "El Niño events exert contrasting hydroclimatic effects." Use "El Niño" consistently rather than "El-Niño." Consider replacing "Pacific basin" with "Pacific watershed" or "Pacific drainage basin," because "Pacific basin" may sound like the ocean basin rather than the Peruvian drainage region. The explanation that fewer heatwaves in the Andes may be due to "the regulating effect of the high altitudes and the glaciers on the minimum temperatures" should be softened or cited. A safer wording would be: "This may reflect the combined influence of elevation, complex topography, and high-altitude climate conditions." Clarify the sign convention for flash-drought severity. Equation 2 gives negative values, while "severity" is often interpreted as a positive magnitude. It is fine to keep negative values, but the text and caption should explain this clearly. Check the reference list carefully. The conclusion is useful but a little long for a micropublication.

All minor comments have been addressed:

- "Result" changed to "Results"
- Placeholder "(add ref.)" removed from the conclusion
- Duplicated punctuation ". ." fixed in the Figure 1 caption
- "El-Niño events assert" corrected to "El Niño events exert"
- "El Niño" used consistently throughout (without hyphen)
- The figure caption now refers to "hydrographic watersheds (Pacific, Atlantic, and Titicaca)" to distinguish these from the ocean basin
- The interpretation of fewer heatwaves in the Andes has been complemented with a new paragraph providing a more physically grounded explanation: "their lower persistence in the Andes may be related to strong diurnal temperature ranges, complex topography, and frequent air mass changes that moderate extreme temperature persistence"
- The sign convention for flash-drought severity is now explained in both the methods text ("the cumulative deficit of pentad-SPEI below the 40th percentile") and figure caption ("y-axis inverted because the severity values are negative")
- Reference list has been checked and corrected

---

## Reviewer 2: Daniel Hagan

### Comment 1: Physical meaning of compound severity metric

> The occurrence definition of compound flash drought–heatwave events as temporal overlap between independently detected heatwaves and flash droughts is reasonable. However, the severity metric, defined as the sum of −SPEIpentad × T′max over the overlap window, needs clearer interpretation. SPEI and normalized temperature are not physically commensurate quantities, so their product should be presented as an empirical compound-intensity index rather than a direct physical severity measure. The multiplication also assumes a bilinear interaction between heat and dryness, which may not always represent the underlying land–atmosphere mechanisms. The authors could strengthen the manuscript by briefly discussing this limitation and, if possible, testing whether compound severity is mainly controlled by heatwave severity, flash drought severity, or overlap duration. I wonder if the authors used something like a sensitivity analysis to compare the current product-based severity index with alternatives such as standardized joint anomalies, overlap duration weighted by normalized exceedances, or a copula-based joint probability/tail-dependence measure (the last one here is my preferred approach)?

**Response:** We agree and have revised the text accordingly. The methods section now describes the compound severity as "an empirical compound intensity index over the overlap window, proposed by Alizadeh et al. (2023), [...] used as a proxy to compound event severity during the overlap period between heatwaves and flash droughts." We further acknowledge that "although this indicator has some limitations in terms of physical interpretability, it provides a useful approximation of the overall intensity of compound events."

Regarding the suggestion of alternative metrics (standardized joint anomalies, copula-based tail-dependence measures): we were unable to perform copula-based analysis due to the limited number of compound events in the study area, which does not provide sufficient data points for robust tail-dependence estimation. We plan to explore these alternatives, as well as overlap-duration-weighted normalized exceedances, in the extended journal version of this work. Thank you for these specific and constructive suggestions.

### Comment 2: Distinction between co-occurrence, dependence, and interaction

> The manuscript correctly defines compound events as co-occurring heatwave and flash drought conditions. However, some parts of the interpretation move toward land–atmosphere feedback language. This is plausible, but the current method primarily detects temporal overlap; it does not directly demonstrate dependence, causality, or feedback. It would improve the scientific precision to distinguish among co-occurrence, statistical dependence, and physical interaction or feedback. I think that a copula, conditional probability, event coincidence analysis, or comparison against randomized/surrogate event sequences could help assess statistical dependence. Please see the methodology in https://rmets.onlinelibrary.wiley.com/doi/10.1002/joc.70306.

**Response:** We agree that our method detects co-occurrence but does not establish statistical dependence or physical interaction. We have removed land-atmosphere feedback language from the conclusion. The methods we use identify temporal overlap of independently detected events; establishing dependence would require conditional probability analysis, event coincidence analysis, or comparison against surrogate event sequences, as the reviewer suggests. We note the reference provided (doi:10.1002/joc.70306) as a valuable methodological guide for future work in this direction.

### Comment 3: Temporal alignment and event-merging rules

> The central methodological strength of the paper is the attempt to place heatwaves and flash droughts on comparable daily-to-pentad timescales. However, it would be helpful to clarify exactly how overlapping events are counted. For example, when one long flash drought overlaps multiple short heatwaves, is this counted as one compound event or several? When a heatwave starts before flash drought onset, is only the overlap period used for duration and severity? Are compound-event durations based strictly on the intersection of the two events, or on the full combined event envelope? I think that these details matter because the manuscript reports the number of events, annual frequency, mean duration, and severity. Clearer event-merging rules would make the results more reproducible and easier to compare with other compound-event studies. Again, please see the methodology in https://rmets.onlinelibrary.wiley.com/doi/10.1002/joc.70306.

**Response:** We have clarified in the methods section that compound events are defined as the co-occurrence (temporal overlap) of heatwaves and flash droughts, and that compound severity is computed "during the overlap period between heatwaves and flash droughts." To be explicit here: compound event duration is based strictly on the intersection of the heatwave and flash drought masks; when one long flash drought overlaps multiple short heatwaves, each overlap is counted as a separate compound event; and compound event metrics (duration, severity) are computed over the overlap window only.

### Comment 4: Sensitivity tests for key thresholds

> The study uses established percentile thresholds, where heatwaves are detected using smoothed Tmax and Tmin above the 90th percentile, while flash droughts are based on pentad-SPEI percentile thresholds including the 40th, 20th, and 30th percentile conditions. These choices are reasonable and literature-based, but the spatial patterns may be sensitive to threshold selection, especially in a country with sharp climatic gradients like Peru. For a micropublication, this does not need to be exhaustive. Even a short statement or supplementary figure showing whether the main coastal and northern Amazon patterns persist under slightly stricter or relaxed thresholds would strengthen the robustness of the conclusions.

**Response:** We appreciate this suggestion and agree that sensitivity analysis would strengthen the results. Due to time constraints for this revision, we have not yet performed threshold sensitivity tests. This is a priority for the extended journal version, where we plan to systematically vary the detection thresholds and assess the robustness of the spatial patterns.

### Minor comment 1: Mechanistic interpretation of regional contrasts

> Improve the mechanistic interpretation of regional contrasts. The spatial results are interesting, showing that heatwaves are more frequent and persistent along the coast and northern Amazon, flash droughts are more widespread and longer in the Pacific and Titicaca basins, and compound events cluster along parts of the coast and northern Amazon. The discussion would benefit from a slightly stronger mechanistic framing. The Andes may appear less exposed to heatwave persistence partly because of elevation, but the interpretation should also consider whether temperature thresholds based on local percentiles could still detect relative extremes in high-altitude climates. A short paragraph linking the regional patterns to Peru's major hydroclimatic regimes would make the results more physically compelling without greatly expanding the manuscript.

**Response:** We have added a paragraph in the results section linking the observed spatial patterns to Peru's main hydroclimatic regimes. This new paragraph discusses: atmospheric subsidence and ENSO-related warming along the coast; strong diurnal temperature ranges and complex topography in the Andes; high evaporative demand and strong radiative forcing in the Pacific drainage basin; strong seasonality and abrupt precipitation interruptions in the Titicaca basin; and episodic reductions in convection and moisture recycling in the Amazon.

### Minor comment 2: Outlook — land-atmosphere feedback vs. advection

> For any future consideration, it might be interesting to apply the analyses used by Miralles et al (2012) to assess the role of land–atmosphere feedback versus advection on the compound event. I suspect that advection would dominate pre-2000 and L–A feedback would emerge as getting stronger post-2000. Secondly, I think there would be a shift from decreases in longer droughts to increasing flash droughts, which would make the study very interesting.

**Response:** Thank you for this stimulating suggestion. Investigating the relative contributions of large-scale advection and local land-atmosphere feedbacks to compound event occurrence and intensification is an exciting direction that we plan to point out and discuss in the closing part of the journal version of our work.

---

## Summary of changes

| Aspect                                                              | Status |
|---------------------------------------------------------------------|--------|
| Pipeline includes figure generation                                 | Done |
| Import-time side effect in constants.py removed                     | Done |
| README with full reproduction instructions                          | Done |
| 3 successful end-to-end pipeline runs (Win, Linux, Linux+container) | Done |
| Methods text aligned with code (HW joint criterion, pentad-SPEI)    | Done |
| Trend language softened                                             | Done |
| Colorblind-safe palette (Okabe-Ito)                                 | Done |
| Larger font sizes throughout figure                                 | Done |
| Panel (b) caption: max severity, inverted axis explained            | Done |
| Compound severity described as empirical index with limitations     | Done |
| Land-atmosphere feedback language removed from conclusion           | Done |
| Event-merging rules clarified                                       | Done |
| Regional patterns linked to hydroclimatic regimes                   | Done |
| All minor textual corrections (El Niño, Results, punctuation, refs) | Done |
| Sensitivity tests for thresholds                                    | Planned for journal version |
| Formal trend analysis (Mann-Kendall)                                | Planned for journal version |
| Dependence analysis                                                 | Planned for journal version |
| FD frequency/duration trends over time                              | Planned for journal version |
| Advection vs. L-A feedback analysis (Miralles et al.)               | Planned for journal version |

We are grateful to both reviewers for their detailed and constructive feedback. Reviewer 2's suggestions regarding sensitivity tests, dependence analysis and the Miralles et al. (2019) framework are particularly valuable for our planned journal submission, and we would welcome the opportunity to stay in contact as this work develops further.
