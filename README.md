# KleptoSyn

Synthetic data generation for investigative graphs based on patterns
of bad-actor tradecraft.

Default input data sources:

  * <https://www.opensanctions.org/>
  * <https://www.openownership.org/>
  * <https://www.occrp.org/en/project/the-azerbaijani-laundromat/the-raw-data>

Ontologies used:

  * <https://followthemoney.tech/>

The simulation uses the following process:

  1. Construct a _Network_ that represents bad-actor subgraphs

    - Use `OpenSanctions` (risk data) and `OpenOwnership` (link data) for real-world UBO data
    - Run `Senzing` entity resolution to generate a "backbone" for organizing the graph
    - Partition into subgraphs and run centrality measures

  2. Configure a _Simulation_ for generating patterns of bad-actor tradecraft

    - Analyze the transactions of the OCCRP "Azerbaijani Laundromat" leaked dataset (event data)
    - Sample probability distributions for shell topologies, transfer amounts, and transfer timing
    - Generate a large portion of "legit" transfers (49:1 ratio)

  3. Generate the _SynData_ (synthetic data) by applying the simulation on the network

    - Track the generated bad-actor transactions
    - Serialize the transactions and people/companies involved


## build an environment

Based on using Python 3.11+

```bash
python3 -m venv venv
source venv/bin/activate
python3 -m pip install -U pip wheel
python3 -m pip install -r requirements.txt
```

This project uses [pre-commit hooks](https://pre-commit.com/) for code
linting, etc.

You will also need the CLI for Google Cloud to get the default input
datasets:
<https://cloud.google.com/storage/docs/discover-object-storage-gcloud>


## load the default data

```bash
gcloud storage cp gs://erkg/starterkit/open-sanctions.json .
gcloud storage cp gs://erkg/starterkit/open-ownership.json .
gcloud storage cp gs://erkg/starterkit/export.json .

wget https://raw.githubusercontent.com/cj2001/senzing_occrp_mapping_demo/refs/heads/main/occrp_17k.csv
```

## run the demo script

```bash
./demo.py
```


## run the notebooks

```bash
./venv/bin/jupyter-lab
```


## use the results

By default, the output results will be serialized into these files:

  + `graph.json`: the network representation
  + `transact.csv`: transactions generated by the simulation
  + `entities.csv`: entities generated by the simulation
