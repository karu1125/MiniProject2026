# Context-Aware Framework for Logistic Incident Reconstruction

## 1. Purpose

### 1.1 Introduction

Logistics operations involve the movement of goods through multiple stages, including manufacturing, transportation, warehousing, and delivery. During these operations, incidents such as cargo damage, handling impacts, packaging failures, and temperature excursions may occur.

Identifying the exact time, cause, and responsible logistics stage of an incident can be difficult because the available evidence is often incomplete, inconsistent, or collected from different sources.

The Context-Aware Framework for Logistic Incident Reconstruction is a Python-based prototype designed to reconstruct logistics incidents using available evidence. It analyzes shipment records, inspection checkpoints, sensor readings, photographs, and custody information to produce an evidence-based incident assessment.

### 1.2 Objectives

The main objectives of this project are:

- To identify the possible time interval during which shipment damage occurred.
- To evaluate the credibility of collected evidence.
- To compare competing causes of a logistics incident.
- To identify the logistics stage associated with the incident.
- To generate a transparent assessment based on supporting evidence.
- To detect modifications to collected evidence using cryptographic hashing.
- To reduce incorrect attribution by allowing the system to return an unresolved result when evidence is insufficient.

### 1.3 Scope

The prototype focuses on four incident causes:

1. Handling impact
2. Packaging failure
3. Securing failure
4. Temperature excursion

The framework uses simulated logistics scenarios to demonstrate and evaluate its functionality. It is intended to support incident investigation rather than replace human judgment or automatically establish legal responsibility.

---

## 2. Architecture Diagram

### 2.1 System Architecture

The system follows a modular architecture in which the web interface communicates with a Python backend. The backend processes shipment information through seven interconnected modules and returns the results to the user.

<img width="3678" height="2184" alt="mermaid-diagram" src="https://github.com/user-attachments/assets/6375e6bd-432f-42fe-bbe5-7c12637906cc" />



### 2.2 Module Description

*Module 1: Evidence Collection*

This module represents the collection and organization of shipment evidence. Evidence may include inspection records, sensor readings, photographs, timestamps, and source information.

Each evidence item is represented using a structured data model so that it can be processed consistently by the remaining modules.

*Module 2: Evidence Credibility Assessment*

This module evaluates the quality of each evidence item based on consistency, source reliability, temporal quality, and corroboration.

The credibility score determines whether an evidence item is sufficiently reliable to be used in subsequent analysis.

*Module 3: Damage Localization*

This module estimates the interval during which shipment damage occurred.

It starts with the last credible undamaged inspection and the first credible damaged inspection. Additional evidence is used to refine the interval while considering possible clock errors and conflicting observations.

*Module 4: Competing-Cause Evaluation*

This module evaluates possible causes of the incident using a predefined support-value table.

Evidence is compared against each candidate cause, and the system calculates a weighted support score. If the evidence does not sufficiently distinguish between competing causes, the framework can return an unresolved result.

*Module 5: Custody Mapping*

This module compares the estimated damage interval with the shipment custody timeline.

It identifies the logistics stages that overlap with the damage interval and determines whether a particular custody stage can be associated with the incident.

*Module 6: Evidence-Based Assessment*

This module combines the outputs of damage localization, cause evaluation, and custody mapping.

It produces the final assessment, including the estimated damage interval, possible cause, candidate custody party, and confidence level.

*Module 7: Evidence Preservation*

This module uses SHA-256 cryptographic hashing and an audit trail to detect changes to collected evidence and generated reports.

The preservation mechanism is separate from evidence credibility assessment. A record may be authentic but unreliable, or reliable but subsequently modified.

### 2.3 Technology Stack

| Component | Technology |
|---|---|
| Programming language | Python 3.10+ |
| Frontend | HTML, CSS, JavaScript |
| Backend | Python HTTP server |
| Data exchange | JSON |
| Core processing | Rule-based logic and mathematical calculations |
| Evidence integrity | SHA-256 hashing |
| Evaluation | Python-based simulation and metrics |

The application uses Python's standard library and does not require third-party Python packages.

---

## 3. Setup

### 3.1 Prerequisites

The following software is required:

- Python 3.10 or later
- Visual Studio Code or another Python-compatible editor
- A modern web browser

### 3.2 Project Structure
```
text
logistics-framework/
│
├── app.py
├── evaluate.py
├── demo_tamper_detection.py
├── README.md
│
├── framework/
│   ├── models.py
│   ├── credibility.py
│   ├── localization.py
│   ├── causes.py
│   ├── custody.py
│   ├── assessment.py
│   ├── preservation.py
│   ├── pipeline.py
│   ├── scenarios.py
│   ├── baselines.py
│   └── serialize.py
│
└── web/
    └── index.html
```

### 3.3 Installation and Execution

Download or extract the project folder and open it in Visual Studio Code.

Open the integrated terminal and navigate to the project directory.

Verify the Python installation:

bash
python --version


Start the web application:

bash
python app.py


The application starts a local HTTP server and opens the web interface in the browser.

If the browser does not open automatically, navigate to:

text
http://127.0.0.1:8000


To run the application on another port:

bash
python app.py 8080


The application can then be accessed at:

text
http://127.0.0.1:8080


No additional package installation is required.

### 3.4 Running the Evaluation

To execute the evaluation script:

bash
python evaluate.py


To demonstrate evidence tamper detection:

bash
python demo_tamper_detection.py


The evaluation script runs the proposed framework and three baseline methods against the available simulated scenarios.

---

## 4. Usage

### 4.1 Accessing the Application

After starting the application, the user can access the web interface through the local browser.

The interface allows the user to select one of the predefined logistics scenarios or create a custom scenario.

### 4.2 Selecting a Scenario

The user selects a simulated incident from the scenario list.

Each scenario contains shipment evidence, custody information, and relevant incident details.

The selected scenario is passed to the backend for processing.

### 4.3 Customizing Evidence and Parameters

The interface allows the user to modify evidence, custody information, and selected analysis parameters.

The framework uses the following default parameters:

| Parameter | Default value | Purpose |
|---|---:|---|
| Evidence credibility threshold | 0.5 | Minimum credibility score for evidence use |
| Minimum cause support | 0.3 | Minimum support required for a candidate cause |
| Cause separation margin | 0.2 | Minimum difference between leading cause scores |

These parameters control how evidence is accepted and how competing causes are evaluated.

### 4.4 Running the Analysis

When the user selects or modifies a scenario, the web interface sends the shipment data to the Python backend.

The backend processes the data through the seven-module pipeline.

The system then returns the analysis results to the web interface.

### 4.5 Understanding the Results

The interface displays the following information:

- Incident verdict and confidence level
- Estimated damage interval
- Shipment event timeline
- Step-by-step reasoning
- Evidence credibility scores
- Competing-cause support scores
- Custody-stage mapping
- Evidence preservation and tamper-detection information

The user can inspect the intermediate results to understand how the final assessment was produced.

### 4.6 Tamper Detection

The tamper-detection demonstration creates an audit trail containing hashes of collected evidence records.

When a record is modified after collection, its calculated hash no longer matches the original recorded hash.

The system identifies the modification and reports the integrity failure.

This demonstrates the use of cryptographic hashing for evidence integrity verification.

---

## 5. Results and Evaluation

### 5.1 Evaluation Methodology

The framework is evaluated using eight predefined simulated logistics incident scenarios.

The evaluation script compares the proposed framework against three baseline methods:

1. First Bad Checkpoint
2. Anomaly-Only Analysis
3. Custody-Only Analysis

The evaluation considers damage-localization error, interval coverage, cause-classification performance, custody attribution accuracy, abstention rate, and false-attribution rate.

The objective is to assess whether the proposed framework produces more reliable and evidence-supported conclusions than simpler methods.

### 5.2 Evaluation Metrics

| Metric | Description |
|---|---|
| Median localization error | Median difference between the estimated damage time and the simulated ground-truth time |
| Interval coverage | Percentage of evaluated incidents where the true damage time falls within the estimated interval |
| Average interval width | Average duration of the estimated damage intervals |
| Cause precision | Proportion of predicted causes that are correct |
| Cause recall | Proportion of known causes correctly identified |
| Macro-F1 | Average F1 score across the evaluated cause classes |
| Custody attribution accuracy | Accuracy of custody-stage attribution |
| Abstention rate | Percentage of scenarios where the system declines to make a definitive attribution |
| False-attribution rate | Percentage of scenarios where an incorrect custody party is named |
| Audit integrity | Whether the audit trail remains consistent with the recorded evidence |

### 5.3 Results

The current prototype produces the following results on its eight simulated scenarios:

| Evaluation measure | Result |
|---|---:|
| False-attribution rate | 0% |
| Cause precision | 1.00 |
| Cause recall | 0.80 |
| Cause macro-F1 | 0.89 |
| Abstention rate | 50% |
| Audit integrity | Passed across all scenarios |

The proposed framework achieves a 0% false-attribution rate on the current simulated dataset.

The three baseline methods have false-attribution rates ranging from 12.5% to 50%.

The framework abstains on 50% of the scenarios. These scenarios were designed to contain ambiguous or insufficient evidence, including sensor drift, conflicting evidence, missing logs, and shock events without confirmed damage.

The results demonstrate that the framework can process evidence, reconstruct incident timelines, compare possible causes, and avoid making unsupported attributions in the designed scenarios.

The tamper-detection demonstration also successfully identifies a modified evidence record while keeping evidence credibility assessment separate from integrity verification.

### 5.4 Limitations

The evaluation results are based on eight hand-authored simulated scenarios.

They demonstrate the functionality of the implemented pipeline but do not establish real-world performance.

The current prototype has the following limitations:

- The scenario dataset is small and does not represent the full complexity of real logistics operations.
- The cause-support table uses predefined rules that require further calibration.
- The audit trail is maintained in memory rather than a permanent append-only storage system.
- The framework has not been validated using a large independent real-world dataset.
- The evaluation does not yet use the larger calibration and test datasets required for reliable performance estimation.

Further development should include a larger randomized scenario dataset, a separate calibration and test process, and validation using real logistics evidence.

---

## 6. Reflection

The biggest technical hurdle I solved during this project was integrating multiple evidence-processing modules into a single incident reconstruction pipeline while ensuring that each module contributed meaningfully to the final result. Initially, evidence collection, credibility assessment, damage localization, cause evaluation, and custody mapping could be treated as separate tasks. However, connecting their outputs required careful data modeling and consistent interfaces between modules.

Another important challenge was handling uncertainty. A system that always identifies a cause or responsible party can produce misleading conclusions when evidence is incomplete or conflicting. I addressed this by implementing credibility thresholds, interval refinement, competing-cause evaluation, and abstention rules. These mechanisms allow the framework to produce an unresolved assessment when the available evidence is insufficient.

Implementing SHA-256 hashing and an audit trail also helped me understand the difference between evidence integrity and evidence credibility. Through this project, I gained practical experience in modular software architecture, Python backend development, mathematical reasoning, and systematic evaluation. The project also taught me the importance of testing assumptions and clearly communicating the limitations of simulated results.

---

## 7. Conclusion

The Context-Aware Framework for Logistic Incident Reconstruction demonstrates how structured evidence analysis can support logistics incident investigation.

By combining evidence credibility assessment, damage localization, cause evaluation, custody mapping, and evidence preservation, the framework provides a systematic approach to reconstructing shipment incidents.

The prototype demonstrates its functionality using simulated scenarios and provides an interactive web interface for examining incident assessments.

Future work will focus on expanding the scenario dataset, improving cause-support rules, implementing permanent evidence storage, and validating the framework using real-world logistics data.
