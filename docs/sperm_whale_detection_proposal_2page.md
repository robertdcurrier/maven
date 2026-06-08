# AI-Assisted Acoustic Detection of Sperm Whales
### A production-proven passive acoustic monitoring workflow
*Proposal narrative — draft. Bracketed [ ] items are
placeholders to be set with partners.*

---

## 1. The problem

Passive acoustic monitoring (PAM) is now the primary tool for
studying deep-diving, rarely-surfacing cetaceans such as the
sperm whale (*Physeter macrocephalus*). Moored recorders,
gliders, and drifting buoys collect months of continuous
audio per deployment — quantities far beyond what human
analysts can review. The bottleneck is no longer data
collection; it is turning raw audio into validated,
defensible detections at scale.

We propose to apply a machine-learning detection-and-
classification workflow that is already in operational use,
retargeted to sperm whale acoustics, to give [partner] an
automated, explainable, and scientifically defensible
sperm whale monitoring capability.

## 2. Why this approach fits sperm whales

Sperm whales are among the most acoustically active marine
mammals, producing broadband impulsive echolocation clicks
and stereotyped social "codas" almost continuously while
foraging. These signals are distinctive and energetic: in a
time–frequency image, clicks appear as broadband vertical
striations and codas as recognizable temporal patterns —
well suited to image-based classification.

The workflow is fully parameterized per species (sampling
rate, frequency band, time–frequency resolution, color
mapping). Retargeting from a low-frequency baleen call to a
high-frequency click train is therefore a configuration-and-
training exercise on labeled data, not a software redesign —
which is precisely why a capability proven on one species
transfers efficiently to another.

## 3. Technical approach — the workflow

```
   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
   │  Audio in    │   │ Spectrogram  │   │  GPU-based   │
   │ WAV / FLAC   │──▶│  generation  │──▶│ classifier   │
   │ (PAM files)  │   │ (mel, tuned) │   │ (deep CNN)   │
   └──────────────┘   └──────────────┘   └──────┬───────┘
     segment +          species-tuned           │ batched,
     chunk long         band, denoise,           │ parallel
     deployments        RGB image                ▼ inference
                                          ┌──────────────┐
   ┌──────────────┐   ┌──────────────┐    │  Detection / │
   │ CSV + PDF    │◀──│  Explainable │◀───│ no-detection │
   │  reports     │   │  annotation  │    │ + confidence │
   │ (QA / QC)    │   │ (Grad-CAM)   │    └──────────────┘
   └──────────────┘   └──────────────┘
     file, status,      heatmap + bbox
     confidence,        over raw
     call locations     spectrogram
```

**Stage detail:**

1. **Audio ingest.** WAV/FLAC recordings are ingested and
   segmented into uniform analysis windows; long deployment
   files are chunked transparently.
2. **Spectrogram generation.** Each segment becomes a
   mel-scaled spectrogram tuned to the sperm whale acoustic
   band, with configurable noise reduction and color mapping.
3. **GPU-accelerated classification.** Spectrograms stream in
   parallel through a deep convolutional neural network that
   scores each image as detection / no-detection with a
   confidence value; throughput reaches hundreds of images
   per second on a single GPU.
4. **Explainable annotation.** Each detection produces a
   Grad-CAM heatmap showing *where* in the image the model
   identified the call, with bounding boxes over the raw
   spectrogram — making every decision auditable by a human.
5. **Structured output & reporting.** Results are written to
   CSV (file, status, confidence, call locations) and
   compiled into thumbnail PDF review reports for QA/QC,
   archiving, and downstream occurrence/density analysis.

## 4. Track record

The workflow is in operational use for Rice's whale
(*Balaenoptera ricei*) detection in the Gulf of America,
reaching 96–98% accuracy on field data through iterative
retraining on real deployment recordings. The same framework
has configuration placeholders for additional species,
demonstrating designed-in transferability rather than a
one-off model.

---

## 5. Scope of work

**Phase 1 — Data assembly & annotation.** Collect partner
PAM recordings containing confirmed sperm whale activity;
assemble a labeled training/validation set with biologist
review. Define class structure (e.g., sperm whale present /
absent, with provision for confusable sources).

**Phase 2 — Baseline model.** Configure the spectrogram
parameters for sperm whale acoustics; train an initial
classifier; establish baseline accuracy, precision, and
recall on held-out field data.

**Phase 3 — Iterative refinement.** Run the baseline over
real deployment data, review detections and false positives
with analysts, and retrain on hard examples — the same
iterative loop that brought the Rice's whale model to
production accuracy.

**Phase 4 — Operational deployment.** Package the validated
model into the end-to-end pipeline, deliver to partner
infrastructure, and train partner staff on operation and
result review.

## 6. Deliverables

- Trained, validated sperm whale detection model.
- End-to-end processing pipeline (ingest → spectrogram →
  classify → annotate → report).
- Validation report: accuracy/precision/recall on held-out
  field data, with example Grad-CAM annotations.
- Labeled dataset and documentation for future retraining.
- Operator training and handover documentation.

## 7. Partner roles

| Role | Responsibility |
|------|----------------|
| **[Partner / data provider]** | Provide PAM recordings; domain expertise; biologist validation of labels and detections |
| **OCEANCODA LLC** | Spectrogram configuration, model development, training, validation, pipeline delivery, operator training |
| **[Field / deployment partner]** | Recorder deployment and recovery; data transfer (if applicable) |

## 8. Timeline & budget (placeholder)

Indicative schedule, approximately [12] months:

| Phase | Duration | Effort (placeholder) |
|-------|----------|----------------------|
| 1 — Data & annotation | [~3 mo] | [TBD] |
| 2 — Baseline model | [~2 mo] | [TBD] |
| 3 — Iterative refinement | [~4 mo] | [TBD] |
| 4 — Deployment & training | [~3 mo] | [TBD] |

Cost categories to be priced with partners: data curation &
annotation; model development & training; compute/GPU;
validation; pipeline integration & deployment; operator
training; project management. *Note: the detection workflow
is delivered as a service; the partner receives a trained
model and an operational pipeline, with charges for the
expertise and effort to build, validate, and deploy them.*

## 9. Risk & mitigation (brief)

- **Insufficient labeled data** → leverage iterative
  retraining and confirmed-encounter recordings; mitigate
  with a defined minimum dataset in Phase 1.
- **High false-positive rate from confusable sources** →
  Grad-CAM review and hard-negative retraining loop.
- **Site/equipment variability** → validate on held-out
  deployments; retrain as new recorders/sites are added.
