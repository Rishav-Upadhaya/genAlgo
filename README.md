# Genetic Algorithm for Solving Quadratic Equations

This project implements a genetic algorithm to find the solution for a quadratic equation of the form `x^2-49` using binary encoding, uniform crossover, and mutation.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Algorithm Details](#algorithm-details)
  - [Fitness Function](#fitness-function)
  - [Binary Encoding](#binary-encoding)
  - [Binary Decoding](#binary-decoding)
  - [Two Point Crossover](#two-point-crossover)
  - [Mutation](#mutation)
- [Results](#results)

## Introduction

This project demonstrates how to use genetic algorithms to solve a quadratic equation. The genetic algorithm evolves a population of binary-encoded individuals over several generations to find the best solution.

## Features

- Binary encoding and decoding of individuals
- Two point crossover to produce offspring
- Mutation to introduce variability
- Fitness function based on the quadratic equation `x^2 - 49`

## Installation (Jupyter)

To run this project locally, follow these steps:

### 1. Clone the repository:

   ```bash
   git clone https://github.com/ronishg27/Genetic-algorithm
   cd Genetic-algorithm
   ```


### 2. Install Jupyter Notebook and dependencies:

   Install Jupyter Notebook and other dependencies using `pip`.

   ```bash
   pip install jupyter
   ```


### 3. Start Jupyter Notebook:

   Launch Jupyter Notebook to run the genetic algorithm notebook.

   ```bash
   jupyter notebook
   ```

### 4. Open the notebook:

   Navigate to the `genetic_algo.ipynb` file in your Jupyter Notebook interface and open it to execute the genetic algorithm code.

### 5. Run the genetic algorithm:

   Follow the instructions within the notebook to run the genetic algorithm and view results.

### 6. Shutdown Jupyter Notebook:

   Once finished, you can shutdown Jupyter Notebook by pressing `Ctrl + C` in the terminal where it's running and confirming the shutdown.

## Algorithm Details
### Fitness Function
  The fitness function evaluates how close a given solution is to the actual solution of the quadratic equation:
```python
def fitness_function(x):
   return 49 - x_real**2
```

### Binary Encoding
  Each individual in the population is represented as a binary string:

```python
def encode_binary(x_real):
    sign_bit = 0 if x_real >= 0 else 1
    x_real = abs(x_real)
    integer_part = int(x_real)
    fractional_part = x_real - integer_part
    integer_bin = f'{integer_part:05b}'
    fractional_bin = f'{int(fractional_part * 2**6):06b}'
    return str(sign_bit) + integer_bin + fractional_bin

```

### Binary Decoding
The binary string is decoded back to a real number:

```python
def decode_binary(x):
   sign_bit = int(x[0])
   integer_part = int(x[1:6], 2)
   fractional_part = int(x[6:], 2) / 2**6
   x_real = (-1)**sign_bit * (integer_part + fractional_part)
   return x_real

```
### Two Point Crossover
  Two point crossover creates two offspring from two parents by randomly selecting two points in the parent's binary string:

```python
def crossover(parent1, parent2):
   point1 = random.randint(1, GENOME_LENGTH - 2)
   point2 = random.randint(point1 + 1, GENOME_LENGTH - 1)
   child1 = parent1[:point1] + parent2[point1:point2] + parent1[point2:]
   child2 = parent2[:point1] + parent1[point1:point2] + parent2[point2:]
   return child1, child2
```
### Mutation
  Mutation introduces random changes to an individual to maintain genetic diversity:

```python
def mutate(individual):
    mutated = ''.join(
        bit if random.random() > MUTATION_RATE else '1' if bit == '0' else '0'
        for bit in individual
    )
    return mutated
```
### Results
  After running the genetic algorithm, the best solution found will be printed along with its fitness value:
  
```
Best value found: 29.84375, Fitness: -841.6494140625
```

