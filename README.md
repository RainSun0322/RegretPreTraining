# Reproducing GlobalRegret and LocalRegret with OLMoE

This repository provides the necessary code and instructions to reproduce the **GlobalRegret** and **LocalRegret** experiments based on the [OLMoE](https://github.com/allenai/OLMoE) framework.

## Prerequisites

Before using the code in this repository, you must first have a working installation of the standard OLMoE.

1.  Follow the official installation guide at [https://github.com/allenai/OLMoE](https://github.com/allenai/OLMoE).
2.  Verify that your standard OLMoE installation runs correctly.

If you encounter issues with the standard installation, please refer to the [Workaround Installation](#workaround-installation) section below.

## Reproduction Steps

Follow these steps to set up the environment for reproducing the experiments.

1.  **Prepare the Training Script**

    You need to choose which experiment you want to run and rename the corresponding training script to `train.py`.

    *   **To reproduce GlobalRegret:**
        Rename `olmo/train-global.py` to `olmo/train.py`.
        ```bash
        mv olmo/train-global.py olmo/train.py
        ```

    *   **To reproduce LocalRegret:**
        Rename `olmo/train-local.py` to `olmo/train.py`.
        ```bash
        mv olmo/train-local.py olmo/train.py
        ```

2.  **Overwrite OLMoE Code**

    Copy the `olmo` directory from this repository and use it to replace the corresponding `olmo` directory in your original OLMoE installation. This will apply the necessary modifications for the experiments.

    ```bash
    # Example command:
    # Assumes `OLMoE-reproduction` is this repo and `OLMo` is the original installation
    cp -rT ./olmo /path/to/your/OLMo/olmo
    ```

3.  **Start Training**

    Use the `OLMoE-7B-A1B.yaml` configuration file provided in this repository to launch the training process. We only added the regret_alpha parameter in this file, so the command will be the same as the standard OLMoE training command.

## Data and Checkpoints

The training data and a 50k-step checkpoint required for the experiments are available on Hugging Face.

*   **You can download them here:** [https://huggingface.co/](https://huggingface.co/xiayusun/olmoe-regret)

## Workaround Installation

If you have difficulty with the standard OLMoE installation from the official repository, you can try the following steps as an alternative.

1.  **Clone the necessary repositories:**

    ```bash
    # Clone the OLMoE repository
    git clone https://github.com/allenai/OLMoE.git

    # Clone a specific branch of the base OLMo repository
    git clone -b Muennighoff/MoE https://github.com/allenai/OLMo.git
    ```

2.  **Merge the directories:**

    Move all contents from the `OLMoE` directory into the `OLMo` directory, overwriting any existing files.

    ```bash
    mv -f ./OLMoE/* ./OLMo
    ```

3.  **Install dependencies:**

    Navigate into the `OLMo` directory and install the required packages.

    ```bash
    cd OLMo

    # Install OLMo in editable mode
    pip install -e .

    # Install the custom megablocks dependency
    pip install git+https://github.com/Muennighoff/megablocks.git@olmoe

    # Install specific versions of torch and related libraries
    pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1

    # Install flash-attention
    # Feel free to use a pre-built wheel (.whl) if you have one compatible with your system
    pip install flash-attention --no-build-isolation
    ```

After completing these steps, your environment should be ready. You can then proceed with the [Reproduction Steps](#reproduction-steps).
