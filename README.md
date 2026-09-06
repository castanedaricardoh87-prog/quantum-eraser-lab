🔬 Time-Dependent Quantum Eraser Lab

A computational and experimental framework for studying environmental which-path information, interference, and photon loss

«Can the loss of quantum interference be separated into two experimentally measurable processes: photon loss and environmental distinguishability?»

This repository explores that question using a time-dependent, lossy Mach–Zehnder interferometer model and a proposed laboratory experiment designed to test the model against real photon-counting data.

The project is deliberately structured as a hypothesis → simulation → experiment → falsification pipeline.

It does not assume the hypothesis is correct.

The goal is to determine whether the predicted relationship survives experimental measurement.

---

🧪 What Is Being Investigated?

A photon entering a Mach–Zehnder interferometer can produce an interference pattern when the two possible paths remain sufficiently indistinguishable.

If information about the photon's path becomes encoded in another physical system or the environment, the paths can become distinguishable and the interference visibility can decrease.

At the same time, photons may also be physically absorbed or lost.

These are not necessarily the same process.

This project therefore treats them as two separate experimental channels:

                    Photon enters MZI
                           │
                           ▼
                 ┌───────────────────┐
                 │ Quantum evolution │
                 └─────────┬─────────┘
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
      Environmental              Photon absorption
      distinguishability               / loss
              │                         │
              ▼                         ▼
      Interference visibility       Survival probability
              │                         │
              └────────────┬────────────┘
                           ▼
                    Detector statistics

The central experimental question is:

«Can these two effects be independently observed and characterized as functions of interaction time?»

---

⚛️ The Hypothesis

The simulation introduces an environmental marker whose distinguishability increases with time.

The marker strength is modeled as:

[
c(t)=1-e^{-t/\tau_m}
]

where:

- t = interaction time
- \tau_m = marker time constant
- c(t)=0 corresponds to no which-path information
- c(t)\rightarrow1 corresponds to increasingly distinguishable path information

The corresponding environmental-state overlap is represented by:

[
\langle m_A|m_B\rangle=\sqrt{1-c(t)^2}
]

and the surviving-photon interference visibility is therefore modeled as:

[
V(t)=\sqrt{1-c(t)^2}
]

Separately, photon absorption/loss is modeled as:

[
\eta(t)=1-e^{-t/\tau_a}
]

with survival probability:

[
S(t)=1-\eta(t)
]

where \tau_a is the phenomenological absorption time constant.

The important distinction

The model does not equate:

[
\text{photon loss}

\text{loss of interference}
]

Instead, it asks whether the two effects can produce distinguishable experimental signatures.

---

🌊 Mach–Zehnder Model

For a balanced interferometer, the output probabilities are modeled as:

[
P_0(t,\phi)=
\frac{1}{2}
\left[
1+V(t)\cos(\phi)
\right]
]

and

[
P_1(t,\phi)=
\frac{1}{2}
\left[
1-V(t)\cos(\phi)
\right]
]

where \phi is the interferometer phase.

As environmental distinguishability increases:

Environmental interaction
          │
          ▼
Which-path distinguishability ↑
          │
          ▼
Environmental overlap ↓
          │
          ▼
Visibility V(t) ↓

Photon absorption is modeled independently:

Interaction time ↑
        │
        ▼
Absorption probability ↑
        │
        ▼
Surviving photon population ↓

The experiment therefore measures both.

---

💻 Computational Experiment

The primary simulation is:

simulation/time_dependent_quantum_eraser.py

It performs a Monte Carlo simulation of a lossy Mach–Zehnder interferometer.

Current simulation configuration

Photons per time point : 300,000
Time points            : 101
Maximum time           : 10
Marker time constant   : 2
Absorption time        : 4
Phase                  : 0
Random seed            : 12345

The simulation also performs phase sweeps to verify that the predicted visibility is actually recovered from simulated detector statistics.

---

📊 What the Simulation Measures

For every time point the model tracks:

time
marker strength
theoretical visibility
Monte Carlo visibility
absorption probability
survival probability
detector counts

The phase-sweep analysis fits:

[
P_0=A+C\cos(\phi)+S\sin(\phi)
]

and calculates:

[
V_{\text{fit}}

\frac{\sqrt{C^2+S^2}}{A}
]

This provides an independent estimate of fringe visibility from the simulated detector data.

---

🔬 Simulation Prediction

The model predicts a characteristic separation between the two processes.

Time| Marker strength| Visibility| Survival
0| 0.000| 1.000| 100%
1| 0.394| 0.919| ~78%
2| 0.632| 0.775| ~61%
4| 0.865| 0.502| ~37%
6| 0.950| 0.312| ~22%
8| 0.982| 0.191| ~14%
10| 0.993| 0.116| ~8%

These values are model predictions, not experimental measurements.

---

🎯 Phase-Sweep Verification

The simulation performs explicit phase sweeps at selected interaction times.

The predicted and measured simulated visibilities closely agree:

t = 0     predicted ≈ 1.000    measured ≈ 0.997
t = 2     predicted ≈ 0.775    measured ≈ 0.775
t = 6     predicted ≈ 0.312    measured ≈ 0.309
t = 10    predicted ≈ 0.116    measured ≈ 0.117

This verifies the numerical implementation.

It does not verify the physical hypothesis.

That distinction is intentional.

---

🧫 Proposed Laboratory Experiment

The next stage is to test the model using a physical interferometer.

A conceptual apparatus is:

                 SINGLE-PHOTON SOURCE
                         │
                         ▼
                  ┌─────────────┐
                  │ Beam Splitter│
                  └──────┬──────┘
                         │
                ┌────────┴────────┐
                │                 │
                ▼                 ▼
             PATH A             PATH B
                │                 │
                │   ENVIRONMENT   │
                │    / MARKER     │
                │                 │
                └────────┬────────┘
                         │
                  ┌──────▼──────┐
                  │ Beam Splitter│
                  └──────┬──────┘
                         │
                 ┌───────┴───────┐
                 ▼               ▼
             DETECTOR 0       DETECTOR 1

The exact implementation of the marker can be selected by the collaborating laboratory.

For example, the experiment could use a controllable internal degree of freedom such as polarization, or another experimentally appropriate which-path marker.

---

🧪 Experimental Controls

The laboratory protocol should contain multiple controls.

Control A — Baseline

No which-path marker.

Prediction

High fringe visibility.

---

Control B — Loss Only

Introduce attenuation/absorption without intentionally encoding which-path information.

Prediction

The number of detected photons decreases.

The surviving photons should retain substantially greater visibility than would be expected from loss alone, subject to technical imperfections.

---

Test C — Which-Path Marker

Increase the strength or interaction time of the path marker.

Prediction

Path distinguishability increases while interference visibility decreases.

---

Test D — Quantum Erasure

Where the marker degree of freedom permits it, measure the marker in an appropriate erasing basis.

Prediction

Conditional interference can be recovered in the appropriate correlations.

---

Test E — Marker + Loss

Operate both mechanisms simultaneously.

Measurement

Measure:

Injected photons
        ↓
Absorbed photons
        ↓
Surviving photons
        ↓
Detector 0 / Detector 1
        ↓
Interference visibility
        ↓
Marker information

This is the central combined experiment.

---

📈 Primary Experimental Dataset

A reproducible experimental run should record variables such as:

interaction_time
marker_setting
marker_distinguishability
injected_counts
surviving_counts
lost_counts
port0_counts
port1_counts
fringe_visibility
phase
measurement_uncertainty

The resulting dataset can then be compared directly against the computational model.

---

🧮 Model vs Experiment

The experiment should not simply be fit until it agrees with the simulation.

Instead:

             COMPUTATIONAL MODEL
                     │
                     ▼
              Experimental
               predictions
                     │
                     ▼
              REAL MEASUREMENT
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
       Agreement             Deviation
          │                     │
          ▼                     ▼
   Model supported        Model revised
                                │
                                ▼
                           New experiment

The most scientifically valuable outcome may therefore be a disagreement.

A systematic deviation could indicate that:

- the chosen marker model is incomplete,
- absorption and distinguishability are coupled,
- additional environmental degrees of freedom matter,
- the assumed time dependence is incorrect,
- or experimental technical noise is producing the apparent effect.

---

❌ Falsification Criteria

The project will regard the hypothesis as unsupported if controlled experiments show that the predicted relationship cannot consistently explain the observed data.

Examples include:

- Visibility does not correlate with independently measured distinguishability.
- Loss-only controls produce the same visibility degradation as path marking.
- Increasing marker strength does not produce the predicted trend.
- The time dependence is incompatible with the proposed model.
- Apparent visibility loss disappears after accounting for ordinary experimental imperfections.
- Quantum-erasure measurements fail to produce the expected conditional correlations under conditions where the standard eraser protocol predicts them.

The objective is therefore not to prove the model.

The objective is to determine whether it survives experimental testing.

---

🧠 Why This Could Matter for Quantum Computing

If the experimental results establish a reliable way to distinguish:

[
\text{loss}
]

from

[
\text{environmental distinguishability}
]

then the same measurement philosophy could potentially be applied to photonic quantum hardware.

Instead of describing a quantum device only through an aggregate error rate, one could investigate:

Environmental interaction
          ↓
Path / state distinguishability
          ↓
Coherence degradation
          ↓
Interference degradation
          ↓
Computational error

This raises a longer-term engineering question:

«Can environmental information become a diagnostic signal for predicting degradation of quantum computation?»

That question is outside the scope of the current simulation and would require additional experimental validation.

---

🚀 Longer-Term Research Direction

The project can evolve through several stages:

PHASE 1
Numerical model
     │
     ▼
PHASE 2
Independent laboratory replication
     │
     ▼
PHASE 3
Experimental dataset
     │
     ▼
PHASE 4
Model validation / falsification
     │
     ▼
PHASE 5
Environmental-state diagnostics
     │
     ▼
PHASE 6
Photonic quantum-system testing
     │
     ▼
PHASE 7
Adaptive control / error mitigation research

The later stages are research directions, not established results.

---

📁 Repository Structure

quantum-eraser-lab/
│
├── README.md
│
├── hypothesis/
│   ├── hypothesis.md
│   └── theoretical_model.md
│
├── simulation/
│   ├── time_dependent_quantum_eraser.py
│   ├── requirements.txt
│   └── results/
│
├── experiment/
│   ├── experimental_protocol.md
│   ├── apparatus.md
│   ├── data_schema.md
│   └── controls_and_falsification.md
│
├── analysis/
│   ├── visibility_analysis.py
│   ├── loss_analysis.py
│   └── experimental_fit.py
│
└── figures/
    ├── marker_visibility_vs_time.png
    ├── absorption_survival_vs_time.png
    ├── photon_fate_vs_time.png
    └── fringe_decay_over_time.png

---

▶️ Running the Simulation

Install the dependencies:

pip install numpy matplotlib

Run:

python simulation/time_dependent_quantum_eraser.py

The simulation produces:

marker_visibility_vs_time.png
absorption_survival_vs_time.png
photon_fate_vs_time.png
fringe_decay_over_time.png

These figures provide the initial computational prediction that the laboratory experiment is designed to test.

---

⚠️ Scientific Scope

This repository is an idealized open-system model.

It does not claim to provide a microscopic model of a particular commercial coating, material, detector, or environment.

The marker and absorption time constants are chosen simulation parameters.

The simulation demonstrates the consequences of the specified mathematical model.

It does not establish that a particular physical system will follow those equations.

That distinction is fundamental to the project.

---

🔭 Research Philosophy

The project follows a simple principle:

«Prediction first. Measurement second. Interpretation third.»

A simulation can generate a beautiful result without describing nature correctly.

The purpose of the laboratory experiment is therefore to place the model in a situation where nature is allowed to disagree with it.

If the data agree, the model becomes experimentally interesting.

If the data disagree, the disagreement becomes the next research question.

---

📜 Current Status

Computational

Implemented

- Time-dependent environmental marker
- Interference visibility model
- Independent absorption channel
- Monte Carlo photon detection
- Time-resolved statistics
- Phase-sweep verification
- Visibility fitting
- Experimental prediction plots

Experimental

Proposed

- Mach–Zehnder laboratory implementation
- Controlled which-path marker
- Independent loss measurement
- Time-resolved measurements
- Quantum-erasure control
- Experimental model comparison

Quantum Computing

Future research direction

- Environmental coherence diagnostics
- Photonic hardware characterization
- Error-source discrimination
- Adaptive monitoring
- Potential feedback/control experiments

---

🤝 Collaboration

This project is intended to be testable by an independent quantum-optics laboratory.

A successful collaboration would ideally produce:

1. An independently constructed interferometer.
2. A controlled which-path marker.
3. Independent measurements of photon loss and interference visibility.
4. A reproducible experimental dataset.
5. Statistical comparison between experiment and model.
6. Publication or open dissemination of the results where appropriate.

The strongest outcome is not confirmation.

The strongest outcome is reproducible knowledge.

---

📚 Citation

If this project contributes to your research, please cite the repository and the associated experimental work when available.

---

The Question

At its core, this project asks a very simple question:

«When a quantum system interacts with its environment, can we experimentally resolve the difference between the system disappearing and the system remaining while its interference information becomes distributed into the environment?»

The answer should come from the experiment.
