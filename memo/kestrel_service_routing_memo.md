# To: Ritu Deshpande, Head of D2C Operations
## Subject: Routing bot renewal decision

**Recommendation: do not switch the routing bot off next week. Start a two-week shadow pilot of the local router, then decide using actual transfers and final resolutions.**

The model was trained to predict the team that ultimately resolved a request, not merely repeat the old bot's first assignment. On a later-in-time historical holdout, it matched final resolution teams on **84.62%** of requests, compared with **76.67%** for the existing routing decision on those same requests. That is a **7.95 percentage-point** improvement. It still made an error on **333 of 2,165 requests (15.38%)**, so it is not ready to route every request without oversight.

The vendor bot costs **₹3.2 lakh per year**, or approximately **₹26,667 per month**. The local model makes no paid AI calls, so its direct per-request API charge is **₹0**; hosting, support, and staff time are not priced here. Ending the vendor contract could avoid its fee, but the model's validation result is not proof of rupee savings. The operations policy lists ₹305 per transfer and ₹260 for an additional customer contact, but those figures must be checked against observed pilot events before treating them as recoverable savings.

### What to do next week

1. Run the model in **shadow mode** beside the current bot; do not change customer routing yet.
2. For every request, record the model's suggested team, actual first team, final team, transfers, and any employee override.
3. Review wrong and ambiguous suggestions each day, especially overlapping repair/warranty/return requests.
4. At the end of two weeks, compare final-team accuracy, transfer rate, overrides, and cost per request by channel and product. Renew or disable the vendor only after Operations and Finance agree the measured outcomes justify it.

**Bottom line:** the evidence supports a controlled test, not immediate replacement. The decision to defer is about operational risk, not a need for paid AI.
