layout  : wiki
title   : Keras
summary : 
date    : 2019-10-21 14:32:07 +0900
updated : 2019-10-21 14:43:36 +0900
tag     : ML
toc     : true
public  : true
parent  : ML
latex   : false
---
* TOC
{:toc}

## preparation for Running Keras

#### Environment
Pop OS(Ubuntu 18.04)

### Install numpy and Keras

```
sudo apt-get install python-pip  
sudo pip install numpy scipy
```
### Install Virtual Environment
```
sudo pip install virtualenv
```
### Activate Virtual Environment
```
$ source venv/bin/activate
```
### Install Jupyter Notebook on virtual environment
```
pip install ipython
pip install --upgrade pip
sudo apt install jupyter-notebook
jupyter-notebook
```
### Install Core Packages
```
pip install numpy scipy scikit-learn matplotlib pandas pydot h5py
```
pydot is used for model visualizationJJ
