# EE C249B Project

## Data Cleaning and Component Selection
Very briefly, there are two parts to this repo. The first part is the importing and cleaning of data directly downloaded from DigiKey (https://www.digikey.com/) for given components. This required the use of regex expressions to do some calculations for quantities such as volume and energy density, particularly when these quantities are not directly provided. Furthermore, code for importing full folders of csv files is included, as this made it possible to avoid doing many manual changes in Excel. volumecalc_ceram() and energy_density() are used to calculate the volume of the capacitor/inductor and energy density of the capacitor respectively. The data I used is included in appropriate folders. This data makes it possible to choose components.  

The data cleaning can be summarized as 
* Importing the data from a folder of downloaded DigiKey data.
* Clean the data necessary for analysis. Other columns can be cleaned as needed. Check out the Quantiphy package for scientific notation https://pypi.org/project/quantiphy/.
* Then use Jamie Bull's awesome functions to find the Pareto optimal values in each set.
* Plot the data and then choose a value on the Pareto front.

Then, there is some Pareto optimal point selection done, using functions written by Jamie Bull from https://code.activestate.com/recipes/578287-multidimensional-pareto-front/ and https://oco-carbon.com/metrics/find-pareto-frontiers-in-python/ respectively. Here is a sample of the 2D Pareto optimal set for the ceramic capacitors using function.
<img width="600" alt="Screen Shot 2022-05-14 at 3 47 10 PM" src="https://user-images.githubusercontent.com/49104657/168450411-6baa8530-d5d3-4e6c-a425-30973fe2b7e7.png">

## Multi-Objective Optimization
The next part of the project is the optimization.
* We first initialize the optimization using the Pymoo framework.
* We then define the loss and volume functions.
* We define the constraint g1.
* We optimize our two objective functions.
* Plot the results.

To provide some more details, the number of capacitors in parallel in particular must be found, as they determine both the loss and the volume of the overall converter. Furthermore, they dictate the voltage across C2.   In the initialization of the function we set there to be two objectives (loss and volume), 1 constraint, and then lower and upper bounds of 1000uF and 200uF. In the ```evaluate``` function we define the capacitors as ```C1 = x[0]*2000e-6``` and ```C2 = x[1]*1000e-6```. Our lower bounds in the class initialization are thus 0.5 and 0.2 respectively.

After generating the objective functions ```f1``` and ```f2``` we can optimize them both! As there are two objectives to be minimized, we use NGSA-II. We use 200 generations. 
```
Generations =200

algorithm = NSGA2(
    pop_size=60,
    n_offsprings=Generations,
    sampling=get_sampling("real_random"),
    crossover=get_crossover("real_sbx", prob = 0.9, eta=15),
    mutation=get_mutation("real_pm", eta = 15),
    eliminate_duplicates=True)


res = minimize(problem,
               algorithm,
               termination,
               seed=2,
               save_history=True,
               verbose=True);

X = res.X
F = res.F
```
Doing the optimization results in a Pareto front generated. We find that the selected components produce results between 8cm^3 -19cm^3 of volume and  and 3 - 8W of loss.

<img width="1004" alt="Screen Shot 2022-05-14 at 4 09 53 PM" src="https://user-images.githubusercontent.com/49104657/168450878-0d1aa482-b979-4fc4-847b-fc74a8c7b80c.png">


