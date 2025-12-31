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

To see the resulting structure of the yaml files using DUMMY DATA, run the demo pipeline for ERCP procedure note generation.
```bash
# keep --proc=ercp_base for ercp
# --module is `module_id` listed in the yaml file's metadata section
# --demo_data_json is the demo data file located in `templating/demo_data/`
python templating/demo_ercp_yaml_pipeline.py --proc=ercp_base --module=0.2 --demo_data_json=0.2stone.json5
```
Feel free to add as many test json files as needed in the `templating/demo_data/` folder to test different scenarios. Fields in the JSON file should match those defined in the corresponding YAML fields. Fields not present in the JSON will be treated as empty.

The demo pipeline will generate a procedure note using the demo data. The output note will be saved in `templating/demo_ercp_report.txt`.

For example, to generate a note for an "ERCP with stone extraction" procedure, using the demo data file `0.2stone.json5`, run:
```bash
python templating/demo_ercp_yaml_pipeline.py --proc=ercp_base --module=0.2 --demo_data_json=0.2stone.json5
```
The terminal will display this:
```
Procedure type: stone_extraction
Loading field definitions from: /Users/emilyguan/Downloads/EndoScribe/endo-templating/prompts/ercp/yaml/modules/0.2_stone_extraction.yaml
  Generated prompt: prompts/ercp/subtypes/generated_stone_extraction_prompt.txt
  Generated base template: drafters/procedures/ercp/generated_stone_extraction.yaml
  Generated Pydantic model: models/generated_stone_extraction_model.py

Using demo data file (templating/demo_data/0.2stone.json5)...

Procedure note written to: /Users/emilyguan/Downloads/EndoScribe/endo-templating/templating/demo_ercp_report.txt
```
I can open `templating/demo_ercp_report.txt` to see the generated procedure note. I can also open the generated prompt, base template, and Pydantic model in their respective folders to see how they were created based on the field definitions and demo data.