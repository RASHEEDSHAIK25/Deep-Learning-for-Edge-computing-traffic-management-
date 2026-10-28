Setup and run (Windows) — create Python 3.7 environment

1) Install Miniconda or Anaconda (if not installed): https://docs.conda.io/en/latest/miniconda.html

2) From a PowerShell (run as normal user) in the project root run:

```powershell
conda env create -f "environment.yml"
conda activate smartcity-py37
python "Deep Learning for Edge Computing in Indian Smart Cities Real Time Analytics on Low Power Devices\Main.py"
```

Notes:
- This environment uses Python 3.7 as required. Some packages (especially TensorFlow 1.14 and older binary wheels) expect Python 3.7.
- If any package fails to build, install the Visual C++ Build Tools from Microsoft and retry.
- If you prefer Docker instead, I can generate a Dockerfile using a Python 3.7 base image.
