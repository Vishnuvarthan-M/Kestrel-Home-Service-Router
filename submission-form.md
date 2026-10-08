# Submission Form

### What did you build, and what business decision does it support? State the number and the rupees.

I built a local text-and-metadata routing classifier, a FastAPI endpoint, a Streamlit screen, chronological validation evidence, and a submission file. It supports a **shadow pilot rather than immediately turning off the current bot**: validation accuracy against eventual resolution teams (`final_team`) was **84.62%**, compared with **76.67%** for the historical first routing decision on the same holdout, a **7.95 percentage-point** improvement.

The vendor fee is **₹3,20,000/year = ₹26,667/month**. The model uses no paid inference API, so direct paid inference is **₹0/request** and **₹0/month at 700 requests**; hosting, maintenance, and staff time are excluded. Do not count the vendor fee as savings until a monitored pilot establishes the model is safe to replace it.

### What score do you expect predictions.csv to get on the hidden outcomes, on which metric, and why that metric? Say how you estimated it.

If hidden outcomes mean eventual resolution team (`final_team`), I expect approximately **84.6% accuracy**. This estimate comes from the chronological holdout: **1,832 correct out of 2,165 requests**. Accuracy is easy to interpret as the share of requests assigned to the right single team; macro F1 (**0.8402**) is included as a secondary check across all seven teams. This is an estimate, not a guarantee.

The original ask instead names `team_label`. The final predictions target `final_team`, so this score estimate does **not** claim 90% agreement with `team_label`. An earlier `team_label` experiment is reported in the README as 96.49%, but its validation predictions are not in the repository and I cannot reproduce that number from this clone.

### How do you know it works? How you validated, on what split, error rate, and the kind of case it gets wrong.

The model evidence records an 80/20 chronological split: 8,657 training requests followed by 2,165 later validation requests. The validation file confirms **1,832 correct / 333 incorrect**, or a **15.38% error rate**. The same holdout's historical routing decision was correct against `final_team` for 1,660/2,165 requests (**76.67%**). On that holdout, the model corrected 243 cases where the old bot was wrong and newly missed 71 cases where it had been right.

Errors cluster around short or mixed-intent requests, especially repairs vs warranty, returns vs billing, and advice vs returns. Robot Vacuum was the weakest product-family slice in the saved analysis (20.45% error). The checked-in prediction file has 2,178 unique request IDs, the expected `request_id,team` columns, no blank team values, and only the seven expected team names.

### Did you change, narrow, or push back on the client's ask? What, when, and why?

Yes. I did not optimize the final submission to imitate `team_label`. I used `final_team` as the target because the supplied analysis records 2,471/10,822 initial-label mismatches (22.83%) and 2,696/10,822 requests with at least one transfer (24.91%). The question is therefore whether routing reaches the team that resolves the request, not merely whether it repeats the old assignment. This is a deliberate change to the stated 90% target and means the original bot should **not** be retired on this result alone.

### What is wrong with what you are handing us, or with the data we handed you?

- This clone does not contain the raw CSV pack, `ops-policy.pdf`, `email-thread.txt`, or the trained model artifact. Data/model paths are git-ignored. Consequently I could inspect saved evidence and validate the submission file's shape, but could not retrain or independently re-run the reported model scores from this workspace.
- The original policy-cost calculations are not independently auditable here because the policy PDF is absent. The stated ₹305/transfer and ₹260/additional-contact figures should be verified before being treated as actual avoidable savings.
- The hidden-outcome distribution may differ from the holdout. No confidence interval or live pilot result is available.
- A previous evidence note had stale bot/model correction counts; it has been reconciled to the saved validation predictions (243 / 71 / 262).
- The generated predictions file could not be compared row-by-row with `sample_submission.csv` because that file is absent. It does match the documented two-column format and 2,178-row count.
- This working copy has not been pushed with these edits. The public URL below is the cloned upstream repository, not a claim that the upstream already includes this submission.

### What did you deliberately leave out, and why that rather than something else?

I left out automatic production cutover, paid LLM calls, and uncalibrated confidence-based routing. The held-out error rate is still 15.38%, so a shadow pilot with employee review is safer and yields the missing live transfer and cost evidence. The local TF-IDF/LinearSVC design avoids a key dependency and has no paid per-request inference charge.

### Anything you built or found that nobody asked for?

The operational-target audit compares initial routing with eventual resolution, and the handoff recommends measuring transfers and overrides during a shadow pilot. The API also starts without the ignored model file, reports degraded health, and returns an explicit HTTP 503 from `/route` until the provided data pack is used to train the model.

### What did you use AI for? Which tools and models, where they helped, where they wasted your time, what you threw away. Link your three-minute screen recording here.

For this implementation pass, I used a coding assistant through Copilot SDK in VS Code for repository inspection, code changes, and documentation. The routing product itself uses no external LLM or paid model API; direct paid API spend is **₹0**. Any Copilot subscription cost is not available from this workspace. I discarded paid-API and more complex model approaches in favor of the local, inspectable scikit-learn model.

**Screen recording:** Not recorded or uploaded yet. **Required before submission:** record a walkthrough of the data/target decision, validation, API/UI, and what was changed or discarded (under three minutes), upload it to Google Drive with public viewing enabled, and replace this line with the link.

### Your Public Google Drive Link

**Pending:** use the public Google Drive link to the screen recording above.

### Someone picks this up on Monday and you are unreachable. The three things they need to know.

1. Run in shadow mode first; the model is 84.62% accurate against `final_team` on the saved chronological holdout, not proven safe for autonomous production routing.
2. The raw client pack and trained model are not in this public repository. Put the supplied CSV files in `data/`, install `requirements.txt`, and run `python src/train_final.py`; without the model, the API reports degraded health and `/route` returns HTTP 503.
3. Monitor final-team accuracy, transfers, employee overrides, and costs per channel/product. The ₹3.2 lakh annual vendor fee (about ₹26,667/month) is potential avoided spend only after a successful pilot and renewal decision.

### Honest hours spent. One number.

**7 hours.**

### Github Repo Link

https://github.com/ermonsterking/kestrel-service-routing

### What does one prediction cost, and what would a month cost at Kestrel's volume (about 700 orders a month)? Show the arithmetic. If you used no paid calls, say so.

Paid model/API calls: **none**.

- Direct paid inference cost: **₹0/prediction**
- At about 700 predictions/month: **700 × ₹0 = ₹0/month** in paid inference charges.
- This excludes local compute/hosting, integration, maintenance, and employee review; their costs were not provided.
- For comparison, the current bot fee is **₹3,20,000 ÷ 12 = ₹26,666.67/month**, or approximately **₹38.10 per request** at 700 requests/month. That is not a savings claim or a recommendation to cancel before the pilot.
