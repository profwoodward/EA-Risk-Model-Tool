# EA Risk Model Tool

A browser-based companion to the paper *Quantifying Compromise Risk in
Exceptional Access Architectures Under Sparse and Indirect Evidence*. It lets
you vary the model's parameters and see, in real time, the resulting **range**
of compromise probability for exceptional-access (EA) systems — together with
plain-English materials that explain what the model does, what it finds, and how
to read the numbers honestly.

- **Paper:** <https://arxiv.org/abs/2606.19106>
- **Author:** Alan Woodward, Surrey Centre for Cyber Security, University of Surrey

---

## What is in this repository

| File | What it is |
|------|------------|
| `index.html` | The interactive **EA Risk Explorer** — a single, self-contained web page. Open it in any modern browser; nothing is installed and no data leaves your machine. |
| `Plain_English_Explainer_of_Risk_Model_for_EA_systems.pdf` | A glossary and plain-language account of every concept, method and finding in the paper, written for policy and non-technical readers. |
| `Risk_Modelling_EA_decison_aid.pdf` | The **full technical paper** (108 pp.), as published on arXiv. Contains the complete methodology, the three-pillar and Bayesian derivations, Proposition 1, and all supplementary material. (The filename is retained for link stability; the contents are the paper itself.) |

---

## The question, in one paragraph

If a government requires that encrypted communications carry some built-in means
for an authorised party to read the contents — an **exceptional access** (EA)
mechanism — how likely is it that *the access mechanism itself* is broken into
and abused by an attacker? Every such mechanism creates a second door into
otherwise-protected data, and that door is a target. This work puts defensible
numbers on how often that door is likely to be forced, and explains exactly how
much confidence each number can bear.

---

## How the model answers it

No EA system has ever been deployed at the scale being debated, so there is no
direct track record to count from. That is a state of **deep uncertainty**: it
is not honest to produce a single precise figure, because the figure would
depend almost entirely on unstated assumptions. The methodologically correct
response is to attack the same question several structurally different ways and
report where they agree and where they diverge. The model does this through
three independent "pillars" plus a fourth, dependence-aware layer:

- **Pillar I — Historical analogy.** Measure the actual compromise rates of real
  systems that resemble EA in some important respect (high-security government
  key custodians, certificate authorities, large platform operators). These land
  on the order of 0.5–1% per system per year.
- **Pillar II — Scenario simulation.** Decompose the threat into a small set of
  attack scenarios, parameterise each from public empirical sources, and
  propagate them through a Monte Carlo simulation to produce a distribution of
  outcomes.
- **Pillar III — Architectural benchmark.** Assume every component performs at
  the best level ever observed historically, and ask how good the system would
  have to be just to match that best practice.
- **Layer IV — Bayesian dependence model.** Replace point estimates with full
  distributions, and model the fact that real adversaries run *coordinated
  campaigns* rather than independent one-off attempts — which the three pillars,
  by sharing an independence assumption, cannot capture.

Two architectures are modelled: **T-EA** (interception built into the network
carrier's infrastructure, e.g. CALEA) and **OTT-EA** (interception built into
the messaging or storage platform). The Levy–Robinson "ghost user" proposal is
the canonical example of OTT-EA, though the OTT-EA risk figures are anchored
chiefly on platform key-custody compromises; the ghost variant maps most
directly onto abuse of the authorised-access pathway rather than theft of key
material.

---

## What it finds

1. **EA always increases risk.** Across every defensible parameter choice, an
   architecture with EA modifiers carries strictly higher modelled compromise
   risk than the same architecture without them. This is the firmest finding: it
   concerns the *direction* of the effect, not its magnitude.

   *Why this is not the truism it first appears.* It can look obvious — surely an
   extra access path adds risk. But the comparison is deliberately against the
   *same* architecture with the EA mechanism removed, which rules out the genuine
   counter-argument that a well-engineered mechanism (strong segregation, hardware
   key custody, mandated standards) might be risk-neutral or even risk-reducing.
   The mathematics behind the result is intentionally slight; its substance is the
   evidence — from incidents such as Salt Typhoon, the Athens affair and
   Storm-0558 — that any workable EA mechanism necessarily adds attack surface,
   attracts targeting, and concentrates key material. The claim is therefore
   falsifiable, not definitional: to reject it, one must name which of those three
   effects could fall to zero, and for which design.

2. **The central estimates are in the low single digits per year.** The methods
   converge on roughly 4–7% annually for T-EA and 3–4% for OTT-EA.

3. **The tails are heavy, and OTT-EA's is heavier.** Both distributions place
   non-negligible probability above any plausible acceptability threshold. In the
   most extreme scenarios OTT-EA's apparent advantage disappears and it overtakes
   T-EA — at the 99th percentile, roughly 59% versus 35%.

4. **Assuming independence understates the risk materially.** Modelling realistic
   coordinated campaigns inflates the annual 95th percentile by about ×1.53
   (T-EA) and ×2.83 (OTT-EA), and raises the ten-year cumulative median from
   ~27% to ~37% (T-EA) and ~16% to ~32% (OTT-EA).

5. **A materially bad outcome is probable.** Under the primary calibration the
   probability that the true annual rate exceeds 1% is about 0.99 (T-EA) and 0.91
   (OTT-EA).

6. **Risk compounds, so the safe deployment window is short.** At a 5% cumulative
   acceptability threshold, the maximum defensible deployment is about two years
   for both architectures.

7. **How you build it matters more than which design you pick.** Threshold
   cryptography (requiring a quorum of independent custodians) cuts annual risk by
   roughly an order of magnitude by removing the single point of failure. Hybrid
   client-side scanning moves the other way and should be treated as carrying at
   least the OTT-EA risk profile, with additional endpoint risk on top.

---

## Putting the numbers in context: what counts as "tolerable"?

A few percent per year is hard to evaluate in the abstract. The natural
reference points are the tolerability standards that other high-consequence
domains have made explicit.

| Reference standard | Tolerable annual frequency | EA central (~10⁻²/yr) sits… |
|--------------------|---------------------------|------------------------------|
| HSE "broadly acceptable" individual risk | 10⁻⁶/yr | ~10⁴–10⁵× above |
| HSE intolerable line, public (involuntary risk) | 10⁻⁴/yr | ~10²–10³× above |
| HSE intolerable line, worker | 10⁻³/yr | ~30–70× above |
| IEC 61508 SIL 4, low-demand systems | 10⁻⁵–10⁻⁴ per demand | ~10²–10⁴× above |
| SIL 4 / aviation DAL A, continuous operation | ~10⁻⁹/hr (≈10⁻⁵–10⁻⁴/yr) | ~10²–10⁴× above |

On every yardstick that has an established number, the modelled EA compromise
rates sit one to five orders of magnitude into the **intolerable** region.

**The best comparator, however, is not individual risk — it is societal risk.**
An EA compromise is not an isolated event affecting one person; it is a single,
correlated failure that exposes an entire population's communications at once,
and (because exfiltrated key material retrospectively decrypts historical
traffic) the loss is permanent. The standard apparatus for risks of that shape
is the **F-N curve**, which plots the tolerable frequency of an event against the
number of people it harms, drawn with a slope of about -1 and with deliberate
*aversion to large-scale events*: the more people a single event can affect, the
lower the frequency that is considered tolerable. On such a curve, an event whose
"N" is a national user base of tens of millions would demand a frequency far
below 10⁻⁴/yr — nowhere near a few percent. This is the quantitative form of the
paper's central policy argument: EA belongs in the **catastrophic-risk** class,
to be judged with aversion to its tail, rather than the routine expected-value
cost-benefit class.

Two honest caveats accompany the comparison:

- It is **not strictly like-for-like.** The engineering and HSE thresholds are
  probabilities of a *harm event* (typically death); the EA figure is the
  probability of a *security-control failure* whose consequence is bulk loss of
  privacy. The comparison is structural — it locates EA in the catastrophic,
  treat-with-aversion class — not a claim that a 3% compromise rate equals a 3%
  annual death rate.
- For the specific outcome being legislated for — **bulk loss of personal data
  and privacy — there is no quantified tolerability standard to test against at
  all.** Data-protection law requires security "appropriate to the risk" rather
  than meeting a numeric threshold, and even within the safety world the UK has
  essentially no adopted societal-risk criteria beyond a single anchor point.
  That regulatory vacuum is itself a finding: a mandate cannot be shown to meet a
  bulk-privacy tolerability bar that no one has ever set.

---

## What the work does *not* claim

It does not offer a single defensible point estimate — it argues none is
available. It does not assert that any specific compromise rate will occur; all
projections are conditional on assumptions made explicit throughout. It does not
argue that EA mandates should never be considered. Its conclusion is narrower and
harder to dismiss: the evidentiary bar for any specific mandate is higher than a
central-tendency comparison alone implies, the appropriate framing is
catastrophic-risk policy rather than expected-value cost-benefit, and OTT-EA —
though lower-risk on average — is the *less* defensible of the two designs on the
two dimensions that matter for catastrophic risk: tail probability and
consequence given compromise.

---

## Using the EA Risk Explorer

1. Open `index.html` in any modern browser (or visit the GitHub Pages
   deployment, if enabled).
2. Adjust the model parameters — targeting premium, segregation factor,
   campaign-dependence coupling, deployment horizon, acceptability threshold —
   using the controls.
3. Read off the resulting distribution of annual compromise probability, the
   Fréchet–Hoeffding interval, the tail percentiles, and the tolerability context.

Everything runs locally in the browser; no inputs are transmitted or stored.

---

## How to read the numbers

The model reports several kinds of number, and they are not interchangeable:

- **Empirical anchors** (Stream A ≈ 0.92%/yr, Stream B ≈ 0.445%/yr,
  Stream C ≈ 0.95%/yr) are *measurements* of historical analogue rates, not
  projections of EA risk.
- **Fréchet–Hoeffding intervals** ([2.2%, 7.5%] T-EA; [1.1%, 4.0%] OTT-EA) are the
  model-consistent ranges under any dependence structure within the scenario
  decomposition — recommended for policy use over any single point estimate.
- **Independence-conditional benchmarks** (5.4% T-EA; 3.0% OTT-EA) are what would
  follow *if* attack channels failed independently; they are not robust to
  correlated campaigns.
- **Bayesian prior-predictive intervals** ([1.4%, 16.5%] T-EA; [0.8%, 17.4%]
  OTT-EA) express full distributional uncertainty under the calibrated priors.
  They are wide because the underlying uncertainty is wide.

The most assumption-robust claim is not any single percentage but the
**structural ordering**: under empirically grounded constraints, EA-equipped
architectures of either class carry strictly higher risk than the same
architecture without EA. The magnitude is open to legitimate debate; the
direction is not.

---

## Citation

If you use this tool or the model, please cite the paper:

```bibtex
@misc{woodward2026ea,
  author = {Woodward, Alan},
  title  = {Quantifying Compromise Risk in Exceptional Access Architectures
            Under Sparse and Indirect Evidence},
  year   = {2026},
  eprint = {2606.19106},
  archivePrefix = {arXiv}
}
```

---

## Reproducibility

The simulations use fixed random seeds so that any reader can re-run them and
obtain identical figures; the headline numbers are stable across seeds. See the
paper's reproducibility package for the full code.
