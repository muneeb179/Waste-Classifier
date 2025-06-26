# Rule-Based Waste Classifier ♻️

A transparent, completely offline **expert system** that tells you *what bin* to use for everyday waste items.
No heavy machine-learning models, no internet connection—just clear, editable rules.

1. Features

* Rule-based knowledge base** – every decision is an “if–then” rule you can read.
* Synonym map** – “tin can” is treated the same as “can”, “bottle” → “plastic bottle”, etc.
* Fuzzy matching** – minor typos such as “plastick bottle” are corrected automatically.
* Persistent learning** – new rules are saved to *waste\_rules.json* and survive restarts.
* Built-in metrics** – one call prints Accuracy, Precision, Recall, F1 and a confusion matrix.
* Zero external libraries** – standard Python ≥ 3.9 only.


2. Repository Layout

```
waste_classifier/
│
├─ waste_core.py          ← team-member #1 (core engine, persistence, metrics)
├─ notebook_demo.ipynb    ← team-member #2 (demo & evaluation notebook)
│
├─ waste_rules.json       ← auto-generated knowledge base (editable)
├─ classifier.log         ← log of unknown items for future rule updates
└─ README.txt             ← this file
```

3. Quick Start

1. Clone and open the folder

```
git clone <repo-url>
cd waste_classifier
```

2. Run the interactive demo (no installs needed)

```python
# one-liner demo
python - << 'PY'
from waste_core import WasteClassifier
clf = WasteClassifier()
print(clf.classify("plastic bottle"))   # → ('Plastic', 'Recycle', 'plastic bottle')
PY
```

3. Jupyter Notebook walkthrough

Open *notebook\_demo.ipynb*, run all cells, and screenshot:

* the three required example inputs (banana peel, plastic bottle, battery)
* a few custom inputs (glass jar, tin can, coated paper)
* the evaluation report (accuracy & F-scores)

4. Adding New Rules
---

```python
from waste_core import WasteClassifier
clf = WasteClassifier()
clf.add_rule("coated paper", "Paper", "Recycle")
```

A new entry is written to *waste\_rules.json*.
Re-run `clf.classify("coated paper")` → `('Paper', 'Recycle', 'coated paper')`.

5. Testing & Metrics

Inside the notebook you can call

```python
TEST_SET = {
    "banana peel"   : "Organic",
    "plastic bottle": "Plastic",
    "battery"       : "Hazardous",
    "glass jar"     : "Glass",
    "egg shell"     : "Organic",
    "can"           : "Metal",
    "unknown item"  : None
}
clf.evaluate(TEST_SET)
```

You’ll get a table of Precision, Recall, F1 and an overall Accuracy line.


6. Contributing

1. **Fork** → **branch** → **commit**
2. Add or improve rules in *waste\_rules.json* **or** via `clf.add_rule()` and commit the updated file.
3. Run *notebook\_demo.ipynb*; make sure accuracy is still ≥ 90 %.
4. Open a pull request.
