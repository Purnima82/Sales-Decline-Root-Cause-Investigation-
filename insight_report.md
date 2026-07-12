# Insight Report: Why Did Computers & Accessories Revenue Drop in 2018?

**Dataset:** Olist Brazilian E-Commerce (real transactional data, 2016–2018)
**Scope of investigation:** All product categories, Jan 2017–Aug 2018

## 1. What I found

While exploring category-level trends across the marketplace, one category stood out immediately: Computers & Accessories. It hit its highest-ever monthly revenue in **February 2018 — R$117,217 across 791 orders** — and then just kept falling for four straight months, bottoming out at **R$50,533 in June 2018**. That's a **56.9% drop from peak**, and it didn't really recover after — revenue stayed flat around R$48–50K through July and August, so this looks like a new, lower baseline rather than a temporary dip.

What made this worth digging into: the rest of the marketplace didn't do this. Total revenue across all categories stayed roughly steady or even grew over the same months (R$966K in Feb → R$1.13M in Apr → R$1.01M in Jun). So whatever happened, it was specific to this one category, not some seasonal thing hitting the whole platform.

## 2. Ruling things out

My first instinct, honestly, was that this had to be a delivery problem — late shipments, unhappy customers, that kind of thing. So I checked.

| Hypothesis | What I checked | Verdict |
|---|---|---|
| Delivery got slower | Average delivery delay actually *improved* in Q2 vs Q1 — orders were arriving earlier, not later | Ruled out |
| Prices went up | Average price barely moved (R$110 → R$108), and if anything went slightly down | Ruled out |
| Sellers left the category | Active seller count stayed roughly flat (123 → 127) through Q2, only dropping later in Q3 | Not the primary driver |

None of the obvious explanations held up. That's when I looked at review scores.

## 3. What actually lines up

February 2018 — the exact same month the order volume spiked — also had the **lowest average review score for this category in the entire dataset: 3.47**, compared to a normal range of 4.0–4.3. That's the clearest pattern I found. My read on it: the sudden spike in orders probably overwhelmed sellers who weren't set up to handle that volume — stock running out, packaging issues, slower or wrong shipments — and it showed up in how customers rated their experience. The review data doesn't break down exactly *what* went wrong, but the timing lines up too well to be a coincidence.

What's interesting is that scores recovered back to 4.0+ once volume came back down in the following months — but revenue never recovered with them. That gap is what makes me think the February experience actually cost the category some of its repeat/referral business, not just a one-month blip.

**Where I want to be careful here:** this dataset has a very low repeat-purchase rate overall — only about 3% of customers ever order more than once. So I can't actually prove a clean "bad experience → lost repeat customer" chain from this data alone. It's the strongest explanation I've got, and the correlation is real, but it's not a confirmed cause. If I had access to seller-side fulfillment or stock logs from that period, that's what I'd pull next to actually confirm it.

## 4. What I'd recommend

1. **Set up an alert for review score dropping during a volume spike.** Something like: if a category's average review score drops below ~3.6 while order volume is spiking, flag it. That would've caught this in February instead of four months later when the revenue had already tanked.
2. **Talk to top sellers before expected demand spikes.** A handful of sellers accounted for most of February's volume in this category — checking their fulfillment capacity ahead of known busy periods (promotions, seasonal peaks) could avoid the same strain happening again.
3. **Actually test the retention theory.** Right now it's my best guess, not a confirmed fact — the next step would be checking whether customers who ordered specifically during the February spike come back to buy again at a lower rate than customers from a normal month. That would either confirm this whole theory or rule it out.
