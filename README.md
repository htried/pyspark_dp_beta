# pyspark_dp_beta
A simple pyspark script that implements the most basic Laplacian version of DP on (project, country, page, # views) tuples.

In order to execute this jupyter notebook, you need to ssh into an analytics machine (I'm using stat1005) and kinit to access HDFS, following the guidelines laid out on wikitech at this page: https://wikitech.wikimedia.org/wiki/Analytics/Systems/Jupyter#Access.

Since I am posting this on publicly on Github, I've made sure to clear all outputs that show individual actor signatures or pageviews. People running this notebook with proper access to WMF resources should be able to run all cells and see intermediate outputs as they like. However, before committing to Github, ensure that all confidential information about users has been cleared from publicly viewable cell outputs!

This notebook roughly follows along with the exercises in the two DP tutorials linked within OpenMined's PipelineDP github repo:
- https://github.com/OpenMined/PipelineDP/blob/main/docs/tutorial_1/1_beam_introduction.ipynb
- https://github.com/OpenMined/PipelineDP/blob/main/docs/tutorial_1/2_beam_laplace_mechansim.ipynb

I've adapted them to execute using Apache Spark rather than Apache Beam.
