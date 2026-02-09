# MrBayes Activity
## Objectives
* Run a Bayesian phylogenetic analysis
* Interpret posterior probabilities
* Understand model choices
* See how MCMC settings affect results
* Compare trees across runs
* Think critically about uncertainty

## Part 1: Simple Tutorial
### start MrBayes
```
mb
```

### Load the data
Replace `<data>` with the correct filename
```
MrBayes > execute <data>.nex
```

### Set evolutionary model and rate variation
This sets the evolutionary model to the GTR substitution model with gamma-distributed
rate variation across sites and a proportion of invariable sites
```
MrBayes > lset nst=6 rates=invgamma
```
To check the model before we start the analysis, type `showmodel`. This will give
an overview of the model settings

### Set chain specifications
This will start the Markov chain Monte Carlo
simulation. It will also ensure that you get at least 200 samples from the posterior
probability distribution, and that diagnostics are calculated every 1,000 generations. 
```
MrBayes > mcmc ngen=20000 samplefreq=100 printfreq=100 diagnfreq=1000
```

### Watch the output
If the standard deviation of split frequencies is below 0.01 after 20,000
generations, stop the run by answering no when the program asks Continue the
analysis? (yes/no). Otherwise, keep adding generations until the value falls
below 0.01.

### Summarize parameter values
Summarize the parameter values using the same burn-in
as the diagnostics in the mcmc command. The program will output a table with
summaries of the samples of the substitution model parameters, including the
mean, mode, and 95 % credibility interval (region of Highest Posterior Density,
HPD) of each parameter. 
```
MrBayes > sump
```
Make sure that the potential scale reduction factor
(PSRF) is reasonably close to 1.0 for all parameters; if not, you need to run the
analysis longer. The same applies for the eﬀective sample size (ESS). If the ESSs
are somewhere below 100, you may want to run longer.

### Summarize trees
The program will output a cladogram with the posterior probabilities
for each split and a phylogram with mean branch lengths. Both trees will also be
printed to a file that can be read by FigTree
```
MrBayes > sumt
```

### Discussion
1. What is the posterior probability on the main clades?
2. Which relationships seem most certain?
3. Which are uncertain?
4. What does a posterior probability of 0.6 vs 0.95 mean?


## Part 2: Change Models
Get into groups of 3 and choose a model for each group

### Groups
* **Group A**: `MrBayes > lset nst=1 rates=equal`
* **Group B**: `MrBayes > lset nst=2 rates=gamma`
* **Group C**: `MrBayes > lset nst=16 rates=invgamma`

### Run
```
MrBayes > mcmc ngen=20000
```

### Discussion within groups and then between groups
1. Did the tree topology change?
2. Did posterior probabilities change?
3. Did branch lengths change?
4. Why might model choice matter?

## Part 3: Explore how MCMC settings affect tree space exploration
Get into groups of 3 and choose a model for each group

### Set evolutionary model and rate variation
```
MrBayes > lset nst=6 rates=invgamma
```

### Groups
* **Group A**: `MrBayes > mcmc ngen=50000`
    * How did the standard deviation of split frequencies change? Does this mean the certainty of the consensus tree is stronger?
    * Did the posterior probabilities on branches change? Did they increase?
    * What do the answers to these questions imply about the number of generations
* **Group B**: `MrBayes > mcmc nchains=1 ngen=20000`
    * How does the likelihood trace compare to when there were more chains?
    * How do posterior probabilities differ from when there were multiple chains?
    * Why use multiple chains?
* **Group C**: `MrBayes > mcmc temp=0.5 ngen=20000`
    * Does chain swapping change (frequency of swap/success rate of accepting swap)?
    * If the temperature is higher, are the chains more different?
    * Does likelihood change?


## Part 4: Tree interpretation
### FigTree
Open the consensus tree in FigTree

### Discussion
1. Which clades are strongly supported (>0.95)?
2. Which are uncertain?
3. Are there polytomies?
4. What might cause uncertainty?
    * limited data
    * model mismatch
    * short sequences

## Part 5: Bayesian reflection
### Concepts
1. Why does Bayesian phylogenetics output many trees?
2. Why not just one best tree?
3. What does posterior probability represent?
4. How is this different from bootstrap support?

### Algorithm
1. Why do we need heated chains?
2. What happens if MCMC gets stuck?
3. Why run two independent runs?