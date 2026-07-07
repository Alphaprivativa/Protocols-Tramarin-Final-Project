# Protocols Tamarin Final Project

## General Idea
This repository contains the final project for the Protocols course, focusing on the formal verification of a secure e-prescription protocol using the **Tamarin Prover**. The protocol leverages Ciphertext-Policy Attribute-Based Encryption (CP-ABE) to ensure patient privacy, anonymity, and unlinkability when requesting and retrieving prescribed medication.

The project is divided into two main Tamarin models located in the `main/` directory:

1. **`Equivalence_model.spthy`**: This model proves the **unlinkability** (observational equivalence) of two different patients holding the same attributes for a given prescription. It demonstrates that an attacker (modeled as a curious pharmacy) cannot distinguish between the two patients based on the protocol's execution trace.
2. **`trace_model.spthy`**: This model verifies standard trace properties, such as **anonymity** and **secrecy**. It proves that the patient's identity and secret key remain hidden unless the patient explicitly and voluntarily reveals them.

### Protocol Overview
The protocol consists of three main phases:
1. **Prescription Request**: The patient requests a prescription from a physician, who forwards it to a Medical Authority.
2. **Credential Issuance**: The Medical Authority issues an attribute-based secret key to the patient, tied to their specific attributes (e.g., illness, temporal validity).
3. **ETSI Handshake & Data Retrieval**: The patient initiates a handshake with a pharmacy. The pharmacy sends a challenge encrypted under the required access policy. The patient decrypts it using their secret key and responds, proving they possess the correct attributes without revealing their identity. Upon successful verification, the pharmacy dispenses the medication.

---

## How to Run the Code

This repository is configured with a **Devcontainer**, meaning all dependencies (Tamarin Prover, Maude, Graphviz, etc.) can be installed in a Docker container running the following bash code in CLI. 

``` bash
# Build the image and tag it as "tamarin-dev"
docker build -t tamarin-dev -f .devcontainer/Dockerfile .
docker run -it --rm -v "$(pwd):/workspace" -w /workspace tamarin-dev /bin/bash
```

otherwise, you do can skip manual installation via docker using VsCode docker containers plugin or running it in a codespace.

### Option A: GitHub Codespaces (Browser - No Local Installation)
*The Docker environment is automatically mounted and configured for you.*

1. Click the green **Code** button at the top of this repository.
2. Select the **Codespaces** tab.
3. Click **Create codespace on main**.
4. Wait a few moments for the pre-built container image to be pulled and the environment to initialize.
5. Once the VS Code interface loads in your browser, open the `main/` directory.
6. You can now open `Equivalence_model.spthy` or `trace_model.spthy` and use the Tamarin VS Code extension for syntax highlighting and error checking.

**To run the models:**[^1]
Open the integrated terminal in Codespaces and execute:

```bash
# For the observational equivalence (unlinkability) model
tamarin-prover --diff  --prove --quit-on-warning '/workspaces/Protocols-Tramarin-Final-Project/main/Equivalence_model.spthy'

# For the trace properties (anonymity/secrecy) model
tamarin-prover --prove --quit-on-warning '/workspaces/Protocols-Tramarin-Final-Project/main/Trace_model.spthy'
```


[^1]: This code works also for russing the models in local, the only difference is you could need to build the docker container manually via the code above.