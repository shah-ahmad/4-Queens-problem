# Convert immutable tuple state to a mutable list of lists
def convert_state_to_list(state_tuple):
    return [list(x) for x in state_tuple]

# Convert mutable list of lists back to immutable tuple of tuples
def convert_state_to_tuple(state_list):
    return tuple(tuple(x) for x in state_list)

# Print the board row by row
def print_matrix(matrix):
    for row in matrix:
        print(row)
    print()

# Minimal Problem class
class Problem:
    def __init__(self, initial):
        self.initial = initial


# Depth First Search that finds ALL solutions
def depth_first_tree_search(problem):
    stack = [problem.initial]
    solutions = []

    while stack:
        state = stack.pop()

        if problem.goal_test(state):
            solutions.append(state)
            continue

        for action in problem.actions(state):
            child = problem.result(state, action)
            stack.append(child)

    return solutions


# Define the Four Queens Problem
class FourQueensProblem(Problem):

    def __init__(self):
        super().__init__(((0,0,0,0),
                          (0,0,0,0),
                          (0,0,0,0),
                          (0,0,0,0)))

    def actions(self, state):

        acts = []

        for i in range(4):

            if not any(state[i][j] == 1 for j in range(4)):

                for j in range(4):

                    if state[i][j] == 0:

                        acts.append((i, j))

                break

        return acts


    def result(self, state, action):

        i, j = action

        new_state = convert_state_to_list(state)

        for l in range(4):

            for k in range(4):

                if l == i and k == j:

                    new_state[l][k] = 1

                elif (l == i or k == j or abs(i-l) == abs(j-k)):

                    if new_state[l][k] != 1:

                        new_state[l][k] = 2

        return convert_state_to_tuple(new_state)


    def goal_test(self, state):

        count = sum(cell == 1 for row in state for cell in row)

        return count == 4


# Main
def main():

    f_queen = FourQueensProblem()

    print("Running DFS to solve 4-Queens...\n")

    solutions = depth_first_tree_search(f_queen)

    unique = []

    for sol in solutions:
        if sol not in unique:
            unique.append(sol)

    print("Solutions found:\n")

    for sol in unique:
        print_matrix(sol)


# Run
main()
