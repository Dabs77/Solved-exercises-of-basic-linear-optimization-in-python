# Basic Linear Optimization for Decision Making Models

This repository contains Python solutions to basic linear optimization problems for the course "Decision Making Models". Below are the first two exercises along with their solutions.

## Exercise 1: Optimal Diet Problem

### Problem Statement
A person wants to follow a diet that meets their nutritional requirements while minimizing its cost. They have access to different foods:
- 700 grams of nuts costing $15/gr
- 2 kilograms of powdered supplement costing $4500/pound
- 5 liters of liquid supplement costing $12000/liter

Each gram of nuts provides 1 unit of protein, 3 units of carbohydrates, and 4 units of fat; the powdered supplement provides 3 units of protein, 3 units of carbohydrates, and 1 unit of fat; the liquid supplement provides 2 units of protein, 3 units of carbohydrates, and 2 units of fat. The dietary requirement is a minimum of 80 protein units, a minimum of 90 carbohydrate units, and exactly 65 fat units.

# Planteamiento

## Variables de decisión:
- **X1**: Cantidad de gramos de Frutos Secos
- **X2**: Cantidad de gramos de suplemento en polvo
- **X3**: Cantidad de gramos de suplemento líquido

## Objetivo: F.O
\[ F \cdot O : \text{Min}(Z) = 15X1 + 9X2 + 12X3 \]

## S.A:
### Restricciones: en términos de los parámetros

#### Máximo de suplementos para comprar:
- **Frutos rojos**: \( X1 \leq 700 \)
- **Suplemento en polvo**: \( X2 \leq 2000 \)
- **Suplemento líquido**: \( X3 \leq 5000 \)

Añadir restricciones de no negatividad: \( X1, X2, X3 \geq 0 \)

#### Mínimo de cada componente:
- **UDP**: \( X1 + 3X2 + 2X3 \geq 80 \)
- **UDC**: \( 3X1 + 3X2 + 3X3 \geq 90 \)
- **UDG**: \( 4X1 + 2X2 + 3X3 \geq 65 \)

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
```

## Exercise 2: Pen Factory Optimization

### Problem Statement
A pen factory produces two types of pens that require various amounts of ink and pen bodies. Each type one pen requires 2 ml of black ink and 3 ml of blue ink, while each type two pen requires 2 ml of black ink and 2 ml of red ink. The factory has 1.2 liters of black ink, 1 liter of blue ink, and 0.8 liters of red ink; 800 plastic bodies, 750 metal bodies, and 1200 caps available. The profitability after covering costs is COP $350 per plastic pen and COP $420 per metal pen. The goal is to maximize profitability from pen sales.

### Solution
```python
from docplex.mp.model import Model

m2 = Model(name="Pen Production")

y1 = m2.continuous_var(name='y1')  # type I pens
y2 = m2.continuous_var(name='y2')  # type II pens

m2.add_constraints([2*y1 + 2*y2 <= 1200, 3*y1 <= 1000, 2*y2 <= 800])
m2.add_constraints([y1 <= 800, y2 <= 750, y1 + y2 <= 1200])

m2.maximize(350*y1 + 420*y2)

solution2 = m2.solve()
m2.print_solution()
```

This section provides a clear description of the problem and the complete Python code used to model and solve the optimization problem for maximizing profitability in a pen factory scenario.
