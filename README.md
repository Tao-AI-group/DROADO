# Drug Repurposing-Oriented Alzheimer’s Disease Ontology (DROADO)

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![OWL 2](https://img.shields.io/badge/Format-OWL%202-blue.svg)](https://www.w3.org/TR/owl2-overview/)
[![DOI](https://img.shields.io/badge/DOI-10.1109%2FICHI.2019.8904505-informational.svg)](https://doi.org/10.1109/ICHI.2019.8904505)

The **Drug Repurposing-Oriented Alzheimer’s Disease Ontology (DROADO)** is a formal semantic framework designed to accelerate candidate therapeutic identification and translational repositioning for Alzheimer’s disease (AD). Guided by molecule- and network-based drug repurposing paradigms, DROADO bridges heterogeneous biomedical data by integrating multi-scale biomedical entities—including molecular targets, pathological processes, signaling pathways, and pharmacological actions—to enable automated reasoning and hypothesis generation.

---

## Table of Contents

- [Overview & Motivation](#overview--motivation)
- [Ontology Metrics](#ontology-metrics)
- [Core Architecture & Knowledge Model](#core-architecture--knowledge-model)
  - [Core Concept Types](#core-concept-types)
  - [Object Properties & n-ary Relations](#object-properties--n-ary-relations)
- [Knowledge Reasoning & Inference Rules](#knowledge-reasoning--inference-rules)
  - [Example Candidate Inference](#example-candidate-inference)
- [Evaluation](#evaluation)
- [File Structure](#file-structure)
- [Usage](#usage)
- [Citation](#citation)
- [Funding & Acknowledgments](#funding--acknowledgments)
- [License](#license)

---

## Overview & Motivation

Alzheimer’s disease (AD) is a multifactorial neurodegenerative disease driven by complex molecular cascades and genetic risk factors. While traditional *de novo* drug discovery suffers from high attrition rates and decade-long timelines, computational drug repurposing offers an efficient alternative. 

However, computational repurposing requires harmonizing and reasoning over fragmented, cross-domain datasets. Existing AD and neuroscience ontologies (e.g., CARDO, ADO, ADMO) either focus on high-level administrative portfolios, lack depth in drug/target concepts, or omit rich relational predicates between genes, pathways, and clinical manifestations. DROADO was engineered specifically to:

1. Capture the latest biological mechanisms and pharmacological targets in AD.
2. Formulate explicit, queryable relations between drugs, genes, proteins, pathways, and diseases.
3. Support multi-hop inference paths that systematically propose `mayTreat` relationships.

---

## Ontology Metrics

* **Format:** OWL 2 (Web Ontology Language)
* **Development Environment:** Protégé 5.5.0
* **Classes:** 1,114
* **Object Properties:** 32
* **Axioms:** 3,244

---

## Core Architecture & Knowledge Model

DROADO is centered on five fundamental biomedical pillars: **Disease**, **Drug**, **Gene**, **Protein**, and **Pathway**.

<img width="320" height="274" alt="image" src="https://github.com/user-attachments/assets/43942599-69e5-4ac3-ad30-325d02a9a2e8" />

Figure 1. Knowledge model for AD drug repurposing 

### Core Concept Types

| Concept Type | Description |
| :--- | :--- |
| **Disease** | Focused on nervous system disorders and AD subtypes/stages (aligned with DO and MeSH). |
| **Gene** | Genetic sequences encoding RNA/protein gene products (aligned with GenBank, OMIM, HGNC). |
| **Protein** | Functional macromolecules targeted by small molecules and biologics. |
| **Drug target** | Container class encompassing functional Genes and Proteins. |
| **Pathway** | Coordinated molecular cascades and dynamic biological programs (aligned with ADMO/Reactome). |
| **Biological Process / Molecular Function** | Biological programs and molecular activities (aligned with Gene Ontology). |
| **Drug / Chemical** | Approved substances, experimental compounds, and clinical agents (aligned with DrugBank and DrugCentral). |

### Object Properties & n-ary Relations

DROADO defines **32 object properties** with domain and range constraints, including inverse relations (e.g., `regulates` / `regulatedBy`, `involves` / `involvedIn`, `treats` / `treatedBy`).

To capture complex biomedical context such as **variant risk levels, probabilities, and provenance**, DROADO adopts **n-ary relation patterns** (e.g., reified intermediate classes such as `Disease_associated_Gene` connecting `associatedWith`, `hasGeneName`, and `hasRiskProbability`).

---

## Knowledge Reasoning & Inference Rules

DROADO provides multi-node semantic inference paths (spanning 3 to 5 nodes) to deduce potential therapeutic utility (`mayTreat`):

* **Rule 1 (3 nodes: `Drug -> Protein -> Disease`):**
  $$\big(\text{binds}(dr, pr) \lor \text{upregulates}(dr, pr) \lor \text{downregulates}(dr, pr)\big) \land \big(\text{associatedWith}(pr, di) \lor \text{beUpregulatedBy}(pr, di) \lor \text{beDownregulatedBy}(pr, di)\big) \implies \text{mayTreat}(dr, di)$$

* **Rule 2 (4 nodes: `Drug -> Protein -> Pathway -> Disease`):**
  $$\big(\text{binds}(dr, pr) \lor \text{upregulates}(dr, pr) \lor \text{downregulates}(dr, pr)\big) \land \text{involvedIn}(pr, pa) \land \text{associatedWith}(pa, di) \implies \text{mayTreat}(dr, di)$$

* **Rule 3 (5 nodes: `Drug -> Protein1 -> Pathway -> Protein2 -> Disease`):**
  $$\big(\text{binds}(dr, pr_1) \lor \dots\big) \land \text{involvedIn}(pr_1, pa) \land \text{involves}(pa, pr_2) \land \big(\text{associatedWith}(pr_2, di) \lor \dots\big) \implies \text{mayTreat}(dr, di)$$

### Example Candidate Inference
Applying **Rule 2**:
1. `Lonafarnib` *(Drug)* $\xrightarrow{\text{downregulates}}$ `Rhes` *(Protein)*
2. `Rhes` *(Protein)* $\xrightarrow{\text{involvedIn}}$ `tau degradation` *(Pathway)*
3. `tau degradation` *(Pathway)* $\xrightarrow{\text{associatedWith}}$ `Alzheimer's Disease` *(Disease)*
4. **Inferred Result:** `Lonafarnib` $\xrightarrow{\textbf{mayTreat}}$ `Alzheimer's Disease`


---

## Evaluation

DROADO was benchmarked using the semiotic ontology evaluation framework **OntoKeeper**, comparing performance across syntactic, semantic, and pragmatic dimensions against three existing AD ontologies:

| Metric | DROADO | ADO | ADMO | ONTOAD |
| :--- | :---: | :---: | :---: | :---: |
| **Syntactic Quality** | 0.64 | 0.65 | 0.64 | 0.58 |
| **Semantic Quality** *(Clarity, Consistency, Interpretability)* | **0.95** | 0.94 | **0.95** | 0.82 |
| **Pragmatic Quality** *(Comprehensiveness)* | **0.75** | 0.11 | 0.25 | 0.62 |
| **Overall Score** | **0.77** | 0.56 | 0.61 | 0.67 |

---

## File Structure

```text
.
├── LICENSE                  # Creative Commons Attribution 4.0 International (CC BY 4.0)
├── README.md                # Project documentation and summary
├── ontology/
│   ├── droado.owl           # Main ontology in OWL/XML format
│   └── droado.ttl           # Ontology serialized in Turtle format (optional)
├── queries/
│   └── inference_rules.sparql # SPARQL implementations of Rule 1, 2, and 3
└── docs/
    └── ontology_schema.png  # Diagram illustrating core schema and relation paths
```

---

## Usage

### Loading in Protégé
1. Download and install [Protégé](https://protege.stanford.edu/) (version 5.5.0 or later recommended).
2. Clone this repository:
   ```bash
   git clone https://github.com/Tao-AI-group/DROADO.git
   ```
3. Open Protégé, select **File > Open**, and select `ontology/droado.owl`.
4. Run standard reasoners (e.g., HermiT or Pellet) to inspect class hierarchies and inferred object properties.

---

## Citation

If you use DROADO in your research, please cite our IEEE ICHI publication:

### IEEE Style
> F. Li, M. Wang, H. A. Pham, Y. Xiang, M. T. Amith, and C. Tao, "Systematic Design of Drug Repurposing-Oriented Alzheimer's Disease Ontology," in *2019 IEEE International Conference on Healthcare Informatics (ICHI)*, Xi'an, China, 2019, pp. 1-6, doi: [10.1109/ICHI.2019.8904505](https://doi.org/10.1109/ICHI.2019.8904505).

### BibTeX
```bibtex
@inproceedings{li2019droado,
  author    = {Li, Fang and Wang, Mingqiang and Pham, Huy Anh and Xiang, Yang and Amith, Muhammad "Tuan" and Tao, Cui},
  booktitle = {2019 IEEE International Conference on Healthcare Informatics (ICHI)}, 
  title     = {Systematic Design of Drug Repurposing-Oriented Alzheimer's Disease Ontology}, 
  year      = {2019},
  pages     = {1--6},
  doi       = {10.1109/ICHI.2019.8904505},
  url       = {https://ieeexplore.ieee.org/document/8904505}
}
```

---

## Funding & Acknowledgments
This research was supported in part by:
* The **National Institutes of Health (NIH)** under Award Number **R01 LM011829**.
* The **Cancer Prevention and Research Institute of Texas (CPRIT)** Training Grant **#RP160015**.

---

## License

The DROADO ontology file and documentation are distributed under the **[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)** license. You are free to share and adapt the material as long as appropriate credit is given.
