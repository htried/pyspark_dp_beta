# pyspark_dp_beta
The first notebook is a simple pyspark script that implements the most basic Laplacian version of DP on (project, country, page, # views) tuples.

In order to execute this jupyter notebook, you need to ssh into an analytics machine (I'm using stat1005) and kinit to access HDFS, following the guidelines laid out on wikitech at this page: https://wikitech.wikimedia.org/wiki/Analytics/Systems/Jupyter#Access.

Since I am posting this on publicly on Github, I've made sure to clear all outputs that show individual actor signatures or pageviews. People running this notebook with proper access to WMF resources should be able to run all cells and see intermediate outputs as they like. However, before committing to Github, ensure that all confidential information about users has been cleared from publicly viewable cell outputs!

This notebook roughly follows along with the exercises in the two DP tutorials linked within OpenMined's PipelineDP github repo:
- https://github.com/OpenMined/PipelineDP/blob/main/docs/tutorial_1/1_beam_introduction.ipynb
- https://github.com/OpenMined/PipelineDP/blob/main/docs/tutorial_1/2_beam_laplace_mechansim.ipynb

I've adapted them to execute using Apache Spark rather than Apache Beam.

# Installing PyDP on a production analytics machine (using pip)
start from home directory

clone pydp source
```
git clone https://github.com/OpenMined/PyDP.git
```

install bazel
```
wget https://github.com/bazelbuild/bazel/releases/download/5.0.0-pre.20210817.2/bazel-5.0.0-pre.20210817.2-installer-linux-x86_64.sh
chmod +x bazel-5.0.0-pre.20210817.2-installer-linux-x86_64.sh 
./bazel-5.0.0-pre.20210817.2-installer-linux-x86_64.sh --user
export PATH="$PATH:$HOME/bin"
(and/or add the above line to .bash_profile)
```

make intended import target
```
mkdir <dir>
cd <dir>
```

make target env
```
python3 -m venv env
source env/bin/activate
pip install --upgrade pip
pip install poetry
```

make pydp from source
```
cd ../PyDP
git submodule deinit -f --all
git submodule init
git submodule update
make build
```

If you want this to load in jupyter:
```
pip install jupyter
pip install ipython
export PYSPARK_DRIVER_PYTHON=ipython
export PYSPARK_DRIVER_PYTHON_OPTS="notebook --port 8123  --ip='*' --no-browser"
kinit
```

equivalent to ‘yarn-large’ in wmfdata
```
pyspark2 --master yarn --deploy-mode client --executor-memory 8G --executor-cores 4 --driver-memory 4G --conf spark.dynamicAllocation.maxExecutors=128
```

You’ll see something like:
```
To access the notebook, open this file in a browser:
         file:///srv/home/joal/.local/share/jupyter/runtime/nbserver-13696-open.html
     Or copy and paste one of these URLs:
         http://stat1004:8123/?token=BLAH
     or http://127.0.0.1:8123/?token=BLAH
```

 On your local machine, run something like
```
ssh -N stat1005.eqiad.wmnet -L 8123:stat1005.eqiad.wmnet:8123
```

You should then be able to open your own env in jupyter notebook

# Installing PyDP on a production analytics machine (using conda)
start from home directory

clone pydp source, create conda env, activate conda env with necessary packages
```
git clone https://github.com/OpenMined/PyDP.git
conda-create-stacked env
source conda-activate-stacked env
conda install -c conda-forge bazel
conda install -c conda-forge poetry
```

make intended import target
```
mkdir <dir>
cd <dir>
```

make pydp from source and kinit
```
cd ../PyDP
git submodule deinit -f --all
git submodule init
git submodule update
poetry install
kinit
```

Now start a jupyter notebook by running `ssh <username>@stat100x.eqiad.wmnet -L 8880:127.0.0.1:8880` and navigating to localhost:8880. Select your conda environment and you should be good to go!
