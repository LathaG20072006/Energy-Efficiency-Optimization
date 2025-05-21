import numpy as np
from scipy.optimize import minimize

# Objective function: E = Power * Time (to be minimized)
def energy_consumption(x):
    power, time = x
    return power * time  # Energy in Watt-hours

# Constraints:
# 1. Time must be at least 1 hour
# 2. Power must be at least 10W
# 3. Power * Time must meet at least a minimum energy need (e.g., 100Wh)
def constraint_energy_min(x):
    return x[0] * x[1] - 100  # Must be >= 0

constraints = ({
    'type': 'ineq',
    'fun': constraint_energy_min
})

# Bounds - (Power in Watts, Time in Hours)
bounds = [(10, 100), (1, 10)]

# Initial guess
x0 = [20, 5]

# Run optimization
result = minimize(energy_consumption, x0, method='SLSQP', bounds=bounds, constraints=constraints)

# Output
if result.success:
    power_opt, time_opt = result.x
    print("[OK] Optimization successful!")
    print(f"Optimal Power: {power_opt:.2f} W")
    print(f"Optimal Time: {time_opt:.2f} hours")
    print(f"Minimum Energy Consumption: {energy_consumption(result.x):.2f} Wh")
else:
    print("[FAIL] Optimization failed:", result.message)
