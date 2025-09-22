# Optimizing Marketing Budgets with Linear Programming
Linear programming (also called linear optimization) is a mathematical framework that represents an objective and constraints with linear relationships.  These systems of equations can be solved (typically with the Simplex Method) to determine an optimal outcome across some number of variables.  With countless applications in engineering, supply chain, marketing, and more, linear programming has proven to be an invaluable tool for organizaional optimization.<br />
<br />
In this quick personal project, I will use Linear Programming in Python to optimize marketing ROI.  The problem is based on the following publicly available example from RPubs: https://rpubs.com/PE_Stat/569796.  This example includes a R-implemented solution, but here we will use the pyomo Python package, instead.

### Problem Definition
Multimedia marketing campaigns are a prime candidate for linear optimization, as they typically seek to allocate limited resources (a marketing budget) across several different channels in order to maximize outreach, ROI, or some other metric within the constraints.  Furthermore, historical data is fequently collected on the efficacy of marketing techniques, providing key parameters for the linear model's objective.<br />

In this example, we want to determine the most effective way to spread our marketing budget accross three channels: TV, Print, and Radio.  We have the following metrics for the projected ROI and outreach/$ for each method:<br />
<img width="938" height="200" alt="image" src="https://github.com/user-attachments/assets/3159399b-53fd-476a-b438-51eeff4be64a" />

Additionally, the following constraints are present, aimed at keeping a healthy balance across all three channels and achieving campaign goals:
* There is a maximum budget of $500,000
* TV advertisement must fall within 45-55% of the total budget
* Print media must not receive less than $50,000
* Radio budget must not be less than $25,000
* The campaign must deliver at least 15 million impressions

From these constraints, it is quite easy to set up a mathematical framework to solve for the amount of the budget that should be allocated to TV (denoted x<sub>1</sub>), Print (denoted x<sub>2</sub>), and Radio (denoted x<sub>3</sub>).

### LP Framework
We define our objective to maximize the ROI for the marketing campaign as follows:<br />
<p style="text-align:center"> max 0.25x<sub>1</sub> + 0.18x<sub>2</sub> + 0.10x<sub>3</sub></p>
Subject to the following constraints:<br />
x<sub>1</sub> + x<sub>2</sub> + x<sub>3</sub> <= 500,000 ----------------- "The maximum budget is $500,000"<br />
x<sub>1</sub> <= 0.55 * (x<sub>1</sub> + x<sub>2</sub> + x<sub>3</sub>) ----------------- "TV budget must be no more than 55% of the total"<br />
x<sub>1</sub> >= 0.45 * (x<sub>1</sub> + x<sub>2</sub> + x<sub>3</sub>) ----------------- "TV budget must be at least 45% of the total"<br />
x<sub>2</sub> >= 50,000 ----------------- "Print budget must be at least $50,000"<br />
x<sub>3</sub> >= 25,000 ----------------- "Radio budget must be at least $25,000"<br />
29.41x<sub>1</sub> + 40.0x<sub>2</sub> + 62.50x<sub>3</sub> >= 15,000,000 ----------------- "The campaign must deliver at least 15M impressions"<br />

### Solving the LP
The above constraints can be input into a Python script and solved using the pyomo package.  Annotated code is available in the marketing_budget_problem.ipynb file.

### Conclusions
From the Python script, we have determined the following optimal solution:

**Optimal Variable Values:**
* TV Budget x<sub>1</sub> = $275,000
* Print Budget x<sub>2</sub> = $200,000
* Radio Budget x<sub>3</sub> = 25,000<br />

**Optimized Objective:**
* Total cost: $500,000 (all of the budget)
* Total impressions: 17,650,250 (exceeding the goal of 15M)
* **Total ROI: $107,250**

To double check - does this solution make sense?  TV has the highest possibel ROI at ~30%, so the model elected to dedicate the maximum 55% of the $0.5M budget to TV marketing.  With the remaining budget, we could either invest in Print or Radio.  Print media has the better ROI-to-Impressions ratio (0.0045 ROI-to-Impression vs. Radio's 0.0016), so as much of the remaining budget as possible was dedicated to Print.  Finally, since Radio has the poorest ROI and a lower ROI-to-Impression ratio, it is kept at the minimum value within the constraints, only $25,000.  From the parameters we outlined above, the model's solution makes good, logical sense.  

Note: this is actually a better solution than the one outlined in the RPubs example; this is likely because Python's pyomo package with glpk solver follows a different algorithm than R's lpsolve.  For this particular problem, the Python solver was able to locate a superior maxima, highlighting the importance of using multiple tools and methods to determine the best business outcome.

Thus, from the LP frameworked described, we have found the optimal distribution of the marketing budget across three channels: TV, Print, and Radio.  This will allow our marketing campaign to maximize ROI within the designated constraints and parameters.  Although applied to a small, niche application in this example, the linear programming framework can be applied to any set of problems with defined objectives and constraints.  From supply chain opimization to workforce scheduling, this is an extremely powerful mathematical tool for optimizing real-world systems.

*Project completed September 2025*
