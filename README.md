# Basic Linear Optimization for Decision Making Models

This repository contains Python solutions to basic linear optimization problems for the course "Decision Making Models". Below are the first two exercises along with their solutions.

## Exercise 1: Optimal Diet Problem

### Problem Statement
A person wants to follow a diet that meets their nutritional requirements while minimizing its cost. They have access to different foods:
- 700 grams of nuts costing $15/gr
- 2 kilograms of powdered supplement costing $4500/pound
- 5 liters of liquid supplement costing $12000/liter

Each gram of nuts provides 1 unit of protein, 3 units of carbohydrates, and 4 units of fat; the powdered supplement provides 3 units of protein, 3 units of carbohydrates, and 1 unit of fat; the liquid supplement provides 2 units of protein, 3 units of carbohydrates, and 2 units of fat. The dietary requirement is a minimum of 80 protein units, a minimum of 90 carbohydrate units, and exactly 65 fat units.

### Solution
```python
import cplex
from docplex.mp.model import Model

m = Model(name='Optimal Diet')

x1 = m.continuous_var(name='x1')  # grams of nuts
x2 = m.continuous_var(name='x2')  # grams of powdered supplement
x3 = m.continuous_var(name='x3')  # grams of liquid supplement

m.add_constraint(x1 + 3*x2 + 2*x3 >= 80)  # protein
m.add_constraint(3*x1 + 3*x2 + 3*x3 >= 90)  # carbohydrates
m.add_constraint(4*x1 + x2 + 2*x3 == 65)  # fat

m.add_constraints([x1 <= 700, x2 <= 2000, x3 <= 5000])

m.minimize(15*x1 + 9*x2 + 12*x3)

solution = m.solve()
m.print_solution()

