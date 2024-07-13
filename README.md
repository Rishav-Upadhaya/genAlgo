# Genetic Algorithm for Solving Quadratic Equation

This project uses a Genetic Algorithm to solve the quadratic equation \(9x^2 - 4 = 0\). The algorithm helps find the value of \(x\) that makes \(f(x) = |9x^2 - 4|\) as small as possible.

## Problem Parameters

- Population Size: 10
- Chromosome Length: 9 bits (1 sign bit + 3 integer bits + 5 fractional bits)
- Mutation Rate: 0.2
- Generations: 10

## Fitness Function

The fitness function is the absolute value of \(9x^2 - 4\). We want to make this value as small as possible.

```python
def fitness_function(x):
    x_real = decode_binary(x)
    return abs(9*x_real**2 - 4)  # We want to minimize this
```

## Binary Encoding

The binary representation has 1 sign bit, 3 integer bits, and 5 fractional bits. The `decode_binary` function changes a binary string to a real number.

```python
def decode_binary(x):
    sign_bit = int(x[0])
    integer_part = int(x[1:4], 2)
    fractional_part = int(x[4:], 2) / 2**5
    x_real = (-1)**sign_bit * (integer_part + fractional_part)
    return x_real
```

## Genetic Algorithm Implementation

### Initialize Population

The initial population is made randomly.

```python
def initialize_population(population_size):
    population = []
    for _ in range(population_size):
        individual = ''.join(random.choice('01') for _ in range(bit_len))
        population.append(individual)
    return population
```

### Crossover

Two-point crossover is used to create offspring.

```python
def crossover(parent1, parent2):
    point1, point2 = sorted(random.sample(range(1, bit_len), 2))
    child1 = parent1[:point1] + parent2[point1:point2] + parent1[point2:]
    child2 = parent2[:point1] + parent1[point1:point2] + parent2[point2:]
    return child1, child2
```

### Mutation

Each bit of the offspring can change with a certain chance (mutation rate).

```python
def mutate(individual):
    mutated_individual = list(individual)
    for i in range(bit_len):
        if random.random() < mut_rate:
            mutated_individual[i] = '0' if individual[i] == '1' else '1'
    return ''.join(mutated_individual)
```

### Roulette Wheel Selection

This function selects individuals based on their fitness values.

```python
def roulette_wheel_selection(population, fitness_values):
    total_fitness = sum(fitness_values)
    probabilities = [fitness / total_fitness for fitness in fitness_values]
    pointer = random.random()
    cumulative_prob = 0
    for i, prob in enumerate(probabilities):
        cumulative_prob += prob
        if pointer <= cumulative_prob:
            return population[i]
```

### Main Genetic Algorithm

The genetic algorithm runs for a number of generations and evolves the population to find the best solution.

```python
def genetic_algorithm():
    population = initialize_population(popn_size)
    for generation in range(gen):
        fitness_values = [fitness_function(x) for x in population]
        new_population = []
        for _ in range(popn_size // 2):
            parent1 = roulette_wheel_selection(population, fitness_values)
            parent2 = roulette_wheel_selection(population, fitness_values)
            child1, child2 = crossover(parent1, parent2)
            child1 = mutate(child1)
            child2 = mutate(child2)
            new_population.extend([child1, child2])
        population = new_population

        # Optionally, print the best solution in each generation
        best_individual = min(population, key=fitness_function)
        best_solution = decode_binary(best_individual)
        best_fitness = fitness_function(best_individual)
        print(f"Generation {generation + 1}: x = {best_solution}, Fitness = {best_fitness}")

    best_individual = min(population, key=fitness_function)
    best_solution = decode_binary(best_individual)
    return best_individual, best_solution

best_individual, best_solution = genetic_algorithm()
best_fitness = fitness_function(best_individual)

print(f"Best individual found: {best_individual}")
print(f"Decoded best solution: x = {best_solution}, Fitness = {best_fitness}")
```

## Output

The expected output will show the best solution found in each generation and the final best solution after all generations.

```
Generation 1: x = -3.96875, Fitness = 137.7587890625
Generation 2: x = -1.5625, Fitness = 17.97265625
Generation 3: x = -1.71875, Fitness = 22.5869140625
Generation 4: x = 2.59375, Fitness = 56.5478515625
Generation 5: x = 1.46875, Fitness = 15.4150390625
Generation 6: x = 3.1875, Fitness = 87.44140625
Generation 7: x = -3.0, Fitness = 77.0
Generation 8: x = 0.90625, Fitness = 3.3916015625
Generation 9: x = 0.46875, Fitness = 2.0224609375
Generation 10: x = 3.03125, Fitness = 78.6962890625
Best individual found: 001100001
Decoded best solution: x = 3.03125, Fitness = 78.6962890625
```
