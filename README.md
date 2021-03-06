# Hierarchical Dirichlet Processes for conditional distribution estimation
This package offers an accurate parameter estimation technique for Bayesian Network classifiers. It uses a Hierarchical Dirichlet Process to estimate the parameters (using a collapsed Gibbs sampler). Note that the package is built in a generic way such that it can estimate any conditional probability distributions over categorical variables. 

More information available at http://www.francois-petitjean.com/Research

# Underlying research and scientific paper

This code is supporting a submissiong to Machine Learning entitled "Accurate parameter estimation for Bayesian Network Classifiers using Hierarchical Dirichlet Processes". 

When using this repository, please cite:
```
@INPROCEEDINGS{Petitjean2017-HDP,
  author = {Petitjean, Francois and Buntine, Wray and Webb, Geoffrey I. and Zaidi, Nayyar},
  title = {Accurate parameter estimation for Bayesian Network Classifiers using Hierarchical Dirichlet Processes},
  booktitle = {arxiv},
  year = 2017
  url = {https://arxiv.org/abs/1708.07581}
}
```

# Prerequisites

This package requires Java 8 (to run) and Ant (to compile); other supporting library are providing in the `lib` folder (with associated licenses). 

# Installing

## Compiling HDP estimation
```
git clone https://github.com/fpetitjean/HierarchicalDirichelProcessEstimation HDP
cd HDP
ant compile
``` 
## Getting a cross-platform jar
Simply entering `ant jar` will create a jar file that you can execute in most environments in `bin/jar/HDP.jar`. 
Normal execution would then look like
```java -jar -Xmx1g bin/jar/HDP.jar```
Note that `Xmx1g` means that you are allowing the Java Virtual Machine to use 1GB - although this is ok for most datasets, please increase if your dataset is large. Note that the memory footprint increases linearly with the size of the tree, which in turns increases (in general) exponentially with the number of conditioning variables. 

## Running HDP estimation in command line
The `compile` command creates all `.class` files in the `bin/` directory. To execute the demos, simply run:
```
java -Xmx1g -classpath bin:lib/commons-math3-3.6.1.jar:lib/junit-4.12.jar testing.Test1LevelMain
```
This will run a simple example first sampling data from a known tree and then learning the probability distributions. 

## Using it for your own library
The code available at `src/testing/Test1LevelMain.java` gives a good idea on how to plug your own code with this library. 
Basically, you have to create a dataset in the form of a matrix of integers (`int[N][M+1]`) where `N` is the number of samples, and `M` the number of covariates (or features). `+1` is because the first column gives the values of the target variable you want to get a conditional estimate over. A cell `data[i][j]` represents the value taken by sample `i` for feature `x_{j-1}`. `data[i][0]` represents the value taken for the target variable. Things are coded over integers because this code is for categorical distributions. 
```
int nValuesY = 2;//binary target
int []arities = {2,2}; //2 covariates, both binary
int[][]data = {
  {0,1,1}, //meaning this sample has target=0 and x1=1 and x2=1
  ...
  {0,0,1} //meaning this sample has target=0 and x1=0 and x2=1
};
ProbabilityTree learnedTree = new ProbabilityTree(nValuesY, arities, 50000, TyingStrategy.LEVEL); //initialise tree
learnedTree.addDataset(data); //add data to statistics and runs the sampler
System.out.println(learnedTree.printFinalPks()); //print resulting conditional probability distribution
```

# Support
YourKit is supporting this open-source project with its full-featured Java Profiler.
YourKit is the creator of innovative and intelligent tools for profiling Java and .NET applications. http://www.yourkit.com 

