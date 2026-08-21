# Setup your Azure ML VM

Generic steps to take a fresh Azure ML VM (or compute instance) from bare
metal to a working geospatial/ML Python environment, using this repo's
conda env files.

## 1. Connect to the VM

- Install the **Remote - SSH** extension in VS Code, if not already installed.
- `Ctrl+Shift+P` -> `Remote-SSH: Connect to Host...` -> select (or add) your
  VM's host.
  - If the VM doesn't have an SSH config entry yet, add one to
    `~/.ssh/config` (or your Windows user's `.ssh/config`) pointing at its
    hostname/IP and your username/key.
- Open a terminal in VS Code (`` Ctrl+` ``) once connected — it runs on the VM.

Alternatively, if you're using an Azure ML compute instance, you can open its
built-in JupyterLab or terminal directly from Azure ML Studio instead of
Remote-SSH.

## 2. Clone this repo onto the VM

```bash
git clone https://github.com/jowa-ea/az_envs_setup.git ~/az_envs_setup
cd ~/az_envs_setup
```

(If it's already cloned there, just `cd` in and `git pull`.)

> **Azure ML compute instance note:** the Notebooks/Terminal shell there
> starts you in `~/cloudfiles/code` (a persistent storage mount), not the
> plain home directory. If you run the `git clone` above from that
> directory, the repo ends up at `~/cloudfiles/code/az_envs_setup` — `cd`
> there instead of `~/az_envs_setup`. Either `cd ~` first before cloning to
> get the plain-VM path, or just adjust the `cd` to wherever it actually
> landed (`pwd` after cloning will tell you).

## 3. Install Miniconda

Skip this step if `conda` is already available (check with `conda --version`).

```bash
cd ~
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
bash miniconda.sh -b -p "$HOME/miniconda3"
rm miniconda.sh
"$HOME/miniconda3/bin/conda" init bash
```

Close and reopen the terminal (or `source ~/.bashrc`) so `conda` is on `PATH`.

## 4. Create an environment from one of this repo's `environments/*.yml` files

Start with the base geospatial/ML environment:

```bash
cd ~/az_envs_setup   # or ~/cloudfiles/code/az_envs_setup on an Azure ML compute instance
conda env create -f environments/geospatial-base.yml
conda activate geospatial-base
```

See [`environments/README.md`](environments/README.md) for what each env
file contains and when to use it.

## 5. Point VS Code at the env

- `Ctrl+Shift+P` -> `Python: Select Interpreter` -> pick
  `~/miniconda3/envs/geospatial-base/bin/python`.
- If it's not listed, run `which python` inside the activated env and paste
  that path manually, or `Ctrl+Shift+P` -> `Python: Refresh Interpreters`.

## 6. Verify

```bash
python -c "import pandas, geopandas, rasterio, shapely, pyproj, sklearn; print('ok')"
```

## 7. (Optional) Register a Jupyter kernel

`ipykernel` is already included in `geospatial-base.yml`, so you can
register the env as a kernel directly:

```bash
python -m ipykernel install --user --name geospatial-base --display-name "Python (geospatial-base)"
```

## Adding packages

- Ad hoc, one-off need: `conda install <package>` (or `pip install`) into
  the activated env.
- Something you'll want every time you set up a new VM: add it to the
  relevant `environments/*.yml` file and commit the change, so future
  clones of this repo pick it up automatically.
