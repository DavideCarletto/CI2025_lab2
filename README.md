# TSP Problem Implementation

## Disclaimer
Some parts of this work were discussed with Michele Carena (s349483), Alessandro Benvenuti (s343784), and Federico Carollo (s339645) to exchange ideas and approaches.

## Approach used

To solve the TSP, I adopted an evolutionary strategy following the hyper-modern Genetic Algorithm (GA) flow.

### Individual Representation

A single individual is implemented as a class containing its genotype, which is represented as a list of consecutive nodes (cities) to be visited. The `fitness()` function of the `Individual` class computes the total cost by summing the costs of all edges in the tour.

### Initial Population Generation

The `generate_initial_population()` function uses a primarily greedy approach: given a current city, the next city is chosen as the one with the lowest cost. A small probability parameter allows for occasional random selection instead of the greedy choice to introduce diversity. However, through experimentation, I found that a fully greedy initial population (low randomness probability) provides the best starting point for optimization.

### Evolution Strategy

The `solve()` function performs the evolutionary strategy. To achieve the maximum balance between exploration and exploitation, the hyper-modern GA flow is applied. Two genetic operators are used to generate offspring:

1. **Inversion Mutation**: Minimizes changes in edges so that most neighbors remain neighbors. This operator allows more exploration without losing the relative order trait. The `perform_inversion_mutation()` function selects two random indices and inverts all values between them.

2. **Edge Recombination Crossover**: Preserves low-cost edges from parents, allowing more exploitation while maintaining edge (neighbor) relationships. The `perform_xover()` function takes two parents and creates a child by forming a path that preserves as much as possible the common edges between the parents. At each step, it chooses the next node among those with fewer alternatives to maintain the best path structure.

### Adaptive Operator Selection

To dynamically balance exploration and exploitation, an `idle_counter` tracks stagnation in the best fitness. When the best fitness has not improved for several iterations, the mutation rate is increased to favor exploration. Conversely, when improvement is detected, the mutation rate returns to normal, favoring crossover and exploitation.

### Parent Selection

Tournament selection is used to select parents. This method takes `tau` random individuals from the population and selects the best among them.

## Results

The following table shows the performance of the evolutionary algorithm on the proposed problems:

| Problem File | Size | Best Fitness |
|--------------|------|--------------|
| problem_g_10.npy | 10 | 1497.66 |
| problem_g_20.npy | 20 | 1755.51 |
| problem_g_50.npy | 50 | 2891.44 |
| problem_g_100.npy | 100 | 4266.21 |
| problem_g_200.npy | 200 | 6043.80 |
| problem_g_500.npy | 500 | 9582.61 |
| problem_g_1000.npy | 1000 | 14025.74 |
| problem_r1_10.npy | 10 | 184.27 |
| problem_r1_20.npy | 20 | 350.38 |
| problem_r1_50.npy | 50 | 628.08 |
| problem_r1_100.npy | 100 | 780.49 |
| problem_r1_200.npy | 200 | 1115.63 |
| problem_r1_500.npy | 500 | 1744.63 |
| problem_r1_1000.npy | 1000 | 2549.62 |
| problem_r2_10.npy | 10 | -411.70 |
| problem_r2_20.npy | 20 | -796.86 |
| problem_r2_50.npy | 50 | -2232.38 |
| problem_r2_100.npy | 100 | -4661.42 |
| problem_r2_200.npy | 200 | -9589.93 |
| problem_r2_500.npy | 500 | -24584.05 |
| problem_r2_1000.npy | 1000 | -49477.87 |

## Conclusions

The evolutionary algorithm performs well and helps improve the initial solution, as demonstrated by the output of the problems. However, it is important to note that initialization makes a significant difference: a greedy initialization allows the algorithm to start from a good solution, with the evolutionary process further improving the fitness. A completely random initial solution could cause the evolutionary algorithm to take too long to converge or even converge to a local minimum rather than a global one.
