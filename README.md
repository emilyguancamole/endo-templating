Install Miniforge, which includes conda and mamba. We use conda (or mamba) to create an isolated environment for the project.
```
# for mac:
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh"
bash Miniforge3-MacOSX-arm64.sh
```
Verify installation by restarting terminal and running:
```
conda --version
```

Create conda environment (e.g. named 'myenv') and install packages:

```bash
conda create --name myenv python=3.10
# Activate the environment
conda activate myenv
```

Once you've activated, install required packages (they'll be installed in the conda environment):
```bash
pip install -r requirements.txt
```

To run the demo pipeline for ERCP procedure note generation, use the following command:
```bash
python templating/demo_ercp_yaml_pipeline.py --demo_data_json ercp_demo_data.json
```
This will generate a procedure note using the demo data provided in `templating/demo_data/ercp_demo_data.json`. The output note will be saved in the `output/` directory.