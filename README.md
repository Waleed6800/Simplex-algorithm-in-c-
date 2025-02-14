# Simplex-algorithm-in-c-
Simplex algorithm 
Here is a simple implementation of the simplex algorithm in C++:
```
#include <iostream>
#include <vector>
#include <cmath>

// Define a struct to represent a variable
struct Variable {
    double coefficient;
    double value;
};

// Define a struct to represent a constraint
struct Constraint {
    double coefficients[2]; // coefficients of x and y
    double rhs; // right-hand side value
};

// Define a function to perform the simplex algorithm
void simplex(std::vector<Constraint> constraints, std::vector<Variable> variables, double objectiveFunction[2]) {
    int numConstraints = constraints.size();
    int numVariables = variables.size();

    // Create the initial tableau
    double tableau[numConstraints + 1][numVariables + 2];
    for (int i = 0; i < numConstraints; i++) {
        for (int j = 0; j < numVariables; j++) {
            tableau[i][j] = constraints[i].coefficients[j];
        }
        tableau[i][numVariables] = constraints[i].rhs;
    }
    for (int j = 0; j < numVariables; j++) {
        tableau[numConstraints][j] = objectiveFunction[j];
    }

    // Perform the simplex algorithm
    while (true) {
        // Find the pivot column
        int pivotColumn = -1;
        double minRatio = INFINITY;
        for (int j = 0; j < numVariables; j++) {
            if (tableau[numConstraints][j] < 0) {
                double ratio = tableau[numConstraints][numVariables + 1] / tableau[numConstraints][j];
                if (ratio < minRatio) {
                    minRatio = ratio;
                    pivotColumn = j;
                }
            }
        }

        // If no pivot column is found, the solution is optimal
        if (pivotColumn == -1) {
            break;
        }

        // Find the pivot row
        int pivotRow = -1;
        minRatio = INFINITY;
        for (int i = 0; i < numConstraints; i++) {
            if (tableau[i][pivotColumn] > 0) {
                double ratio = tableau[i][numVariables + 1] / tableau[i][pivotColumn];
                if (ratio < minRatio) {
                    minRatio = ratio;
                    pivotRow = i;
                }
            }
        }

        // If no pivot row is found, the problem is unbounded
        if (pivotRow == -1) {
            std::cout << "Problem is unbounded" << std::endl;
            return;
        }

        // Perform the pivot operation
        double pivotValue = tableau[pivotRow][pivotColumn];
        for (int j = 0; j < numVariables + 2; j++) {
            tableau[pivotRow][j] /= pivotValue;
        }
        for (int i = 0; i < numConstraints + 1; i++) {
            if (i != pivotRow) {
                double multiplier = tableau[i][pivotColumn];
                for (int j = 0; j < numVariables + 2; j++) {
                    tableau[i][j] -= multiplier * tableau[pivotRow][j];
                }
            }
        }
    }

    // Print the solution
    std::cout << "Solution:" << std::endl;
    for (int j = 0; j < numVariables; j++) {
        std::cout << "x" << j + 1 << " = " << tableau[numConstraints][j] << std::endl;
    }
    std::cout << "Objective function value: " << tableau[numConstraints][numVariables + 1] << std::endl;
}

int main() {
    // Define the constraints and variables
    std::vector<Constraint> constraints;
    constraints.push_back({{2, 3}, 12});
    constraints.push_back({{1, 2}, 8});

    std::vector<Variable> variables;
    variables.push_back({3, 0});
    variables.push_back({4, 0});

    double objectiveFunction[2] = {3, 4};

    // Perform the simplex algorithm
    simplex(constraints, variables, objectiveFunction);

    return 0;
}
```
This code defines a `simplex` function that takes in a vector of constraints, a vector of variables, and an objective function as input. It then performs the simplex algorithm to find the optimal solution.

The `main` function defines the constraints and variables for a sample problem and calls the `simplex` function to solve it. The solution is then printed to the console.
