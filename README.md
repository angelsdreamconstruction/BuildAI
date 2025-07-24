# BuildAI
BuildAI
import numpy as np
import random

# Simple Genetic Algorithm for Room Size Optimization (BuildAI MVP)
def fitness(individual, target_area=1000, cost_per_sqft=50):
    total_area = sum(individual)
    area_diff = abs(total_area - target_area)
    total_cost = total_area * cost_per_sqft
    return 1 / (area_diff + 1) - total_cost / 100000  # Maximize fitness (low diff, low cost)

def genetic_algorithm(pop_size=100, generations=50, num_rooms=5):
    population = [np.random.uniform(100, 300, num_rooms) for _ in range(pop_size)]  # Initial pop
    for gen in range(generations):
        fitness_scores = [fitness(ind) for ind in population]
        parents = [population[i] for i in np.argsort(fitness_scores)[-2:]]  # Select top 2
        offspring = [(parents[0] + parents[1]) / 2 for _ in range(pop_size - 2)]  # Crossover
        population = parents + offspring  # New population
    best = max(population, key=fitness)
    return best, sum(best), sum(best) * 50  # Best layout, total area, cost

# Run Example
best_layout, total_area, total_cost = genetic_algorithm()
print(f"Optimized Rooms: {best_layout}\nTotal Area: {total_area}\nTotal Cost: ${total_cost}")
