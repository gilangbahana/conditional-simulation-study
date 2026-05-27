# Quantifying Uncertainty using Conditional Simulation – a Nickel Laterite Case Study

## Problem Framing
- The area is a 12ha wide consist of 53 drillholes with average drillhole spacing around 25m.
- Most of the drillhole are clustered in the western part of the area, while the spacing is irregular in some part of the area.
- The client needed an uncertainty model. Specifically the model should:
  - Contain probability of block grades exceeding 0.8% and 1% cut-off.
  - The probability required is above 0.8.
  - Grade tonnage curve
- Conditional simulation technique is proposed to answer the problem. Kriging estimate is employed in the area as a means to validate the simulation results.
<img width="1747" height="764" alt="Slide-4" src="https://github.com/user-attachments/assets/dcce73cf-52b7-4241-bab8-e79b437467ee" />

## There’s some limitation of the kriging technique…
- Before choosing conditional simulation, it's worth understanding exactly what kriging can and cannot do, because the two methods serve fundamentally different purposes.
- Kriging is an estimation method. The goal is to give the lowest variance at each location.
- Would it honor the global distribution? The variance is too low!
- Would it honor the variogram? It would not be the same, super continuous!
- How could one quantify uncertainty? Kriging variance might be the answer, but:
  - KV is only conditioned to data configuration, not by data values.
  - Because of that, KV can be used only as an indication of quality of the estimate
  - KV can’t be used to determine grade variability within the blocks, nor to understand the probability of block grades exceeding a given cut of.
- How to get around that? Conditional simulation might be used to answer the problem.
  - Conditional simulation solves this by generating 100+ equally probable grade fields, all honouring the data and the variogram, allowing block-by-block probability statements to be made.
<img width="1184" height="1155" alt="Slide-5" src="https://github.com/user-attachments/assets/b99f0371-bd99-423f-b2f8-b16d564ea095" />
*The diagram illustrates a key flaw: two blocks can share the same kriging variance (KV = 0.75) even when their actual grade distributions are completely different. Block 1 has a tight, well-constrained grade; Block 2 is wildly uncertain. KV cannot distinguish them, it only reflects data geometry, not grade variability.*

## Workflow
<img width="1096" height="681" alt="Workflow" src="https://github.com/user-attachments/assets/1a218738-f01f-4f03-8632-2754c6a75cff" />
- Key difference from kriging workflow: after the variogram is fitted, a dense simulation grid is created and populated with 100 realisations using Sequential Gaussian Simulation (SGS). Each realisation is a valid, equally probable image of the deposit. Post-processing then extracts probability, cutoff, and confidence statistics from those 100 values at every block.
- Gaussian variography (Normal Score transform) is required because SGS operates in Gaussian space, grades are transformed to a standard normal distribution before simulation, then back-transformed to real grades afterwards.

## Simulation validation: statistics
- A valid simulation must reproduce the original data's statistical distribution. Each red curve = one realisation's CDF. The black curve = the original composite data CDF. If the simulation is working correctly, the red curves should envelope the black line tightly and symmetrically.
- Domain 1001 (LIM): Red curves envelope the black line closely across the full grade range.
- Domain 1002 (SAP): Wider spread of red curves, especially at the high-grade tail.
<img width="3086" height="1187" alt="Simulation Validation Stats" src="https://github.com/user-attachments/assets/cf3cb38d-e664-4e95-946d-9c196e0295dc" />

## Simulation validation: variogram 
- A valid simulation must also reproduce the spatial structure of the data, not just the histogram.
- The left chart shows the experimental variogram of the original data in the major direction.
- The right chart shows variograms from all 100 realisations (grey lines). They should envelope the input variogram model.
- Two in the left for domain 1001, while two in the right for domain 1002
<img width="5742" height="1081" alt="Simulation Validation Variogram" src="https://github.com/user-attachments/assets/3bb92a4c-3139-4e8d-8f29-ba41f05b7649" />

## Simulation validation: comparison to kriging
- The mean of 100 simulation realisations should approximate the kriging estimate globally (though not block-by-block). This table confirms the simulation average is consistent with kriging: tonnage differences are within 1–4%, and mean grades match closely. This is the final validation check before post-processing.
- A %diff above ~10% would suggest either a bias in the simulation or a data issue. The results here are within acceptable tolerance.
<img width="1076" height="391" alt="Differences" src="https://github.com/user-attachments/assets/8c51ae31-bcf8-4d4f-9216-2eaa98b2f062" />

## Simulation results
- Each of the 100 realisations is a complete, valid grade model of the deposit. Individually they look noisy.
- Unlike kriging, simulation preserves the natural variability of the ore.
- The average of all 100 realisations (rightmost image) converges toward the kriging estimate, but the individual realisations are what enable probability calculations.
<img width="3117" height="846" alt="sim results" src="https://github.com/user-attachments/assets/007efbb4-27ee-4ff9-880d-df36bae80e25" />

## Post-processing: 1% Cutoff (Plan View)
- For each block, we count: "In how many of the 100 realisations did this block's grade exceed 1%?" That count divided by 100 is the probability.
- The maps show blocks coloured by that probability at three threshold levels (0.7, 0.8, 0.9).
- At 0.8 probability, only blocks with at least 80 out of 100 realisations above 1% Ni are included.
<img width="3209" height="846" alt="sim-results 2" src="https://github.com/user-attachments/assets/6be8d79d-06b1-4913-a0c8-4219e5e151b3" />

## Post-processing: 0.8% Cutoff (Plan View)
- Same approach as the 1% cutoff, but now asking: "does this block exceed 0.8% Ni in at least X% of realisations?"
- Because 0.8% is a lower bar, more blocks qualify at each probability level.
- The qualifying footprint is noticeably larger than for the 1% cutoff maps on the previous slide.
<img width="3209" height="846" alt="sim-results 3" src="https://github.com/user-attachments/assets/2efcae23-c421-4f30-b7c0-64e3cbd13c2b" />

## Comparison Table
- Applying a probability filter to the simulation results has a dramatic effect on reportable tonnage.
- The 85% tonnage reduction at 1% cutoff reflects that the deposit is predominantly sub-1% Ni material; only a small, high-confidence core clears that bar at 80% probability.
- The 0.8% cutoff is less restrictive and retains more material (69% reduction), but still represents a significant tightening versus the kriged estimate
- This results higlights how much uncertainty exists in this deposit at the current drill density.
- The table below compares the simulation-derived resources (at 80% probability) against the unconstrained simulation average, which is equivalent to a zero-cutoff baseline.
<img width="1952" height="785" alt="table" src="https://github.com/user-attachments/assets/487e03cf-921c-46d7-af52-dd856a5ee12d" />

## Conclusion
- Conditional simulation technique can be utilized to create uncertainty model in nickel deposit.
- Simulation requires a thorough validation, since the purpose of simulation is to create equiprobable realisations.
- Based on the simulation results, the resource above 1% cutoff using 80% probability is decreasing 85% compared to the resource without cutoff.
- Meanwhile, resource above 0.8% cutoff using 80% probability is decreasing 69% compared to the resource without cutoff.
- Conditional simulation is the recommended tool whenever the client needs to understand where in a deposit they can mine with confidence

## Presentation Deck
[Conditional Simulation - Porto.pdf](https://github.com/user-attachments/files/28312386/Conditional.Simulation.-.Porto.pdf)

