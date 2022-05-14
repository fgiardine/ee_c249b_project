# EE C249B Project

Very briefly, there are two parts to this repo. The first part is the importing and cleaning of data directly downloaded from DigiKey (https://www.digikey.com/) for given components. This required the use of regex expressions to do some calculations for quantities such as volume and energy density, particularrly when these quantities are not directly provided. Furthermore, code for importing full folders of csv files is included, as this made it possible to avoid doing many manual changes in Excel.

Then, there is some Pareto optimal point selection done, using functions written by Jamie Bull from https://code.activestate.com/recipes/578287-multidimensional-pareto-front/ and https://oco-carbon.com/metrics/find-pareto-frontiers-in-python/ respectively.

![capacitors](https://user-images.githubusercontent.com/49104657/168449639-30b6ca9d-dde2-4a27-9eda-aa35e05464c5.png)


