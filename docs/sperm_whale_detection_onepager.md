# AI-Assisted Acoustic Detection of Sperm Whales
### A production-proven passive acoustic monitoring workflow

**Overview.** Passive acoustic monitoring (PAM) generates
enormous volumes of underwater audio — far more than analysts
can review by hand. We have developed and operationally
deployed a machine-learning workflow that automatically
detects and classifies marine mammal vocalizations in PAM
recordings and returns explainable, analyst-reviewable
results. The approach is proven in the field on Rice's whale
(*Balaenoptera ricei*) at 96–98% accuracy and is directly
extensible to sperm whale (*Physeter macrocephalus*)
detection.

**Why sperm whales fit this approach.** Sperm whales are
among the most acoustically active cetaceans, producing
broadband impulsive echolocation clicks and stereotyped
social "codas" almost continuously while foraging. These
signals are distinctive and energetic — clicks render in a
spectrogram as broadband vertical striations, codas as
recognizable temporal patterns — making them well suited to
image-based classification. Because the workflow is fully
parameterized per species (sampling rate, frequency band,
time–frequency resolution, color mapping), retargeting from
Rice's whale's low-frequency calls to sperm whale's
high-frequency click trains is a configuration-and-training
exercise on labeled data, not a redesign.

**The workflow — five stages:**

1. **Audio ingest.** WAV/FLAC recordings from moored
   recorders, gliders, or drifting buoys are ingested and
   segmented into uniform time windows; long deployment files
   are chunked transparently for consistent analysis.
2. **Spectrogram generation.** Each segment is converted to a
   mel-scaled spectrogram tuned to the target species'
   acoustic band, with configurable noise reduction and color
   mapping, yielding standardized RGB images.
3. **GPU-accelerated classification.** Spectrograms are
   streamed in parallel — multi-threaded / multi-process I/O
   feeding batched GPU inference — through a deep
   convolutional neural network that scores each image as
   detection / no-detection with a confidence value.
   Throughput scales to hundreds of images per second on a
   single GPU.
4. **Explainable annotation.** For every detection the system
   produces a Grad-CAM heatmap overlay that highlights *where*
   in the time–frequency image the model identified the
   vocalization, with bounding boxes, alongside the raw
   spectrogram. Every machine decision is visually auditable
   by a human.
5. **Structured output & reporting.** Results are written to
   CSV (file, detection status, confidence, vocalization
   locations) and compiled into thumbnail PDF review reports —
   ready for QA/QC, archiving, or downstream occurrence and
   density analysis.

**What partners get:**

- **Scale** — automated triage of multi-month PAM deployments
  that would take analysts weeks to review manually.
- **Trust** — explainable AI (Grad-CAM) so detections are
  validated rather than black-boxed; essential for scientific
  and regulatory defensibility.
- **Transferability** — a species-agnostic pipeline with a
  documented production track record, proposed here for sperm
  whales and extensible to additional species (beaked whales,
  delphinids, and others) within the same framework.
- **Operational maturity** — an end-to-end deployment
  pipeline, not a research prototype.

**Track record.** The workflow is in operational use for
Rice's whale detection in the Gulf of America, reaching
96–98% accuracy on field data through iterative retraining
on real deployment recordings.
