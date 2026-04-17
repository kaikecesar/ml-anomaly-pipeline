# 🤖 ML Anomaly Pipeline

> Production-grade MLOps pipeline for anomaly detection in fleet telemetry — part of the fleet-telemetry ecosystem.

[![Python](https://img.shields.io/badge/Python-3.12-blue)](https://www.python.org/)
[![SageMaker](https://img.shields.io/badge/AWS-SageMaker-orange)](https://aws.amazon.com/sagemaker/)
[![MLflow](https://img.shields.io/badge/MLflow-2.x-blue)](https://mlflow.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## 📖 About

This project demonstrates **end-to-end MLOps** for detecting anomalies in high-volume vehicle telemetry data. It's the third pillar of my fleet-telemetry portfolio — showing that I can not only build data platforms and APIs, but also **productionize machine learning**.

The goal: detect unusual patterns in vehicle behavior (sudden temperature spikes, abnormal fuel consumption, unexpected stops) in near real-time, with fully automated model training, deployment, monitoring, and retraining.

## 🎯 What This Project Demonstrates

- ✅ Feature engineering for time-series IoT data
- ✅ ML model training with proper experimentation (MLflow)
- ✅ Model deployment with SageMaker Endpoints
- ✅ Automated retraining pipelines
- ✅ Model monitoring + drift detection
- ✅ A/B testing of model versions
- ✅ Feature Store (Feast) for online + offline features
- ✅ MLOps best practices (versioning, lineage, reproducibility)

## 🧰 Tech Stack

**Language:** Python 3.12
**Cloud:** AWS (SageMaker, S3, Lambda, Step Functions, EventBridge)
**ML Frameworks:** scikit-learn, PyOD (anomaly detection), PyTorch (for deep learning option)
**MLOps:** MLflow (experiments + registry), Feast (feature store)
**Data Processing:** PySpark (for large-scale features)
**Orchestration:** SageMaker Pipelines + Airflow
**Monitoring:** Evidently AI (drift), CloudWatch
**IaC:** Terraform

## 🏗️ Architecture

```
┌─────────────────────┐
│ Fleet Data (S3)     │ ← Output from fleet-telemetry-platform
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐     ┌──────────────────┐
│ Feature Engineering │────▶│  Feature Store   │
│ (PySpark on EMR)    │     │     (Feast)      │
└──────────┬──────────┘     └────────┬─────────┘
           │                         │
           ▼                         ▼
┌─────────────────────┐     ┌──────────────────┐
│  Training Pipeline  │────▶│  Model Registry  │
│    (SageMaker)      │     │    (MLflow)      │
└──────────┬──────────┘     └────────┬─────────┘
           │                         │
           ▼                         ▼
┌─────────────────────┐     ┌──────────────────┐
│ SageMaker Endpoint  │◀────│   Deployment     │
│   (Inference)       │     │  (Blue/Green)    │
└──────────┬──────────┘     └──────────────────┘
           │
           ▼
┌─────────────────────┐     ┌──────────────────┐
│  Predictions → API  │────▶│   Monitoring     │
│  (back to Node API) │     │   + Drift Alerts │
└─────────────────────┘     └──────────────────┘
```

## 📂 Repository Structure

```
ml-anomaly-pipeline/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── model-card.md             # Model documentation (what/why/limits)
│   └── decisions/                # ADRs
├── data/
│   ├── preprocessing/            # Data cleaning scripts
│   └── features/                 # Feature engineering
├── models/
│   ├── baseline/                 # Isolation Forest (simple)
│   ├── advanced/                 # LSTM Autoencoder (deep learning)
│   └── evaluation/               # Model comparison framework
├── pipelines/
│   ├── training/                 # SageMaker training pipeline
│   ├── deployment/               # Deployment pipeline
│   └── retraining/               # Automated retraining
├── feature_store/
│   └── feast/                    # Feast definitions
├── monitoring/
│   ├── drift_detection/
│   └── dashboards/
├── infrastructure/
│   └── terraform/
├── notebooks/                    # Exploratory data analysis
├── tests/
└── .github/workflows/
```

## 🗓️ Implementation Roadmap (8 weeks — Months 5-6)

This project starts in Month 5, after the data platform is mature enough to provide clean data.

### Phase 1 — EDA + Baseline (Weeks 17-18)

- [ ] **Week 17:** Exploratory data analysis + problem definition
  - What is an anomaly in this domain?
  - Labeled vs unlabeled approach?
  - Baseline metrics
- [ ] **Week 18:** Baseline model — Isolation Forest
  - Training pipeline in SageMaker
  - First MLflow experiments
  - Basic evaluation metrics

**Deliverable:** Simple but working anomaly detector, versioned in MLflow.

### Phase 2 — Feature Store + Deployment (Weeks 19-20)

- [ ] **Week 19:** Feature engineering pipeline in PySpark
  - Rolling statistics (mean, std, min, max) at 1min/5min/1h windows
  - Fuel efficiency trends
  - Route deviation features
  - Feast integration
- [ ] **Week 20:** SageMaker Endpoint deployment
  - Model registry workflow
  - Blue/green deployment strategy
  - Basic monitoring

**Deliverable:** Deployed model serving predictions via API.

### Phase 3 — Advanced Models (Weeks 21-22)

- [ ] **Week 21:** LSTM Autoencoder for sequential anomalies
  - Deep learning alternative comparison
  - PyTorch + SageMaker integration
- [ ] **Week 22:** Model comparison + champion/challenger
  - A/B testing infrastructure
  - Automated model selection

**Deliverable:** Multiple models in production with traffic splitting.

### Phase 4 — Production MLOps (Weeks 23-24)

- [ ] **Week 23:** Drift detection with Evidently AI
  - Data drift alerts
  - Concept drift detection
  - Automated retraining triggers
- [ ] **Week 24:** Full documentation + model card
  - Production runbook
  - Retraining playbook
  - Blog post write-up

**Deliverable:** Complete MLOps lifecycle documented and running.

## 🔬 Technical Concepts Demonstrated

### Feature Engineering
- Rolling window aggregates for time-series
- Feature store patterns (online + offline consistency)
- Feature versioning and lineage

### Model Development
- Baseline-first approach (don't start with deep learning)
- Proper train/validation/test splits for time-series
- Hyperparameter tuning with SageMaker

### Deployment
- Model registry workflow
- Blue/green deployments
- Canary releases
- Real-time inference endpoints

### Monitoring
- Data drift (feature distributions changing)
- Concept drift (relationship between features and target changing)
- Model performance degradation
- Automated alerts and retraining triggers

### Reproducibility
- All experiments tracked in MLflow
- Data versioning (or at least data hash tracking)
- Environment pinning (Poetry/uv)
- Infrastructure as code (Terraform)

## 📊 Data Strategy

Since this is a portfolio project, I use:
1. **NYC TLC Trip Data** — public dataset, treat trips as "vehicles", inject synthetic anomalies
2. **Generated anomalies** — injected synthetically with labels for validation
3. **Time-based splits** — realistic for time-series ML

## 🚀 Getting Started

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/ml-anomaly-pipeline.git
cd ml-anomaly-pipeline

# Setup environment (using uv for speed)
uv venv
uv pip install -r requirements.txt

# Configure AWS credentials
aws configure

# Run local training (baseline model)
python pipelines/training/local_baseline.py

# View experiments
mlflow ui
```

## 📖 Model Card

See [docs/model-card.md](docs/model-card.md) for detailed model documentation including:
- Intended use
- Training data characteristics
- Performance metrics
- Known limitations
- Fairness considerations
- Maintenance schedule

## 📈 Current Status

**Current phase:** Not started — begins Week 17 (Month 5)
**Prerequisite:** fleet-telemetry-platform Phase 3+ complete

## 🔗 Related Projects

- [fleet-telemetry-platform](https://github.com/YOUR_USERNAME/fleet-telemetry-platform) — Provides clean data for training
- [node-telemetry-api](https://github.com/YOUR_USERNAME/node-telemetry-api) — Consumes predictions and surfaces alerts

## 👤 Author

**Kaike Oliveira** — Software Engineer transitioning to MLOps / Data Engineering.

- 💼 LinkedIn: [linkedin.com/in/kaikeoliveira](https://www.linkedin.com/in/kaikeoliveira)

## 📝 License

MIT
