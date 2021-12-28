# Algorithmic Techniques

## BackTracking
The backtracking technique is a way to build an algorithmn for some hard problem L. Such an algoirthmn searches through a large, possibly even 
exponenital-size, set of possiblties in a systematic way.

The backrackign algorithm traverses through possible "search paths" to locate solutions or "dead end" The configuration at the end of such a path consists
of a pair(x,y), where x is the remaining subproblem to be solved and y is the set of choices that have ben made to get to thsi subproblem from the original problem.

When backtracking algo. finds a configuration which can't be lead to a solution will cut   off the rest of path and "backtrack" to another configuration regardless
what future choices might be in the "dead end" 

```
Algorithm Backtrack(x):
    Input: A problem instance x for a hard problem
    Output a solution for x or "no solution" if none exists
    
    F<-{(x, empty_set)}
    
    while F != empty_set do
        select from F the most pormising configuration (x,y)
        expand9x,y) by making a small set of additinoal choices
        let (x_1, y_1),(x_2, y_2)...(x_k,y_k) be the set of new configurations
        
        for each new configuration(x_i, y_i)
            perform a simple consistency check no (x_i, y_i)
            if the check returns solutino found
                return the solution derived from (x_i, y_i)
            if the check returns dead end
                discard the configuration(x_i y_i). //backtrack
            else
                F<- F union {(x_i, y_i)}
      return no solution
```
**Key Details** In order to turn the backracking strategy into an ctual algorithm, we need only fill in the following details
1. Define a way of selecing the most "promising" condidate configuration from the frontier set F
2. Specify the way of expanding a configuration(x,y) into subproblem configurations. This expansion process should be able to generate all feasible configurations, starting from the initial configuration, (x, empty_set)
3. Describe how to perform a simple consistency check for a configuration(x,y) that returns "solution foud" "dead end" or "continue"

### Branch and Bound
We can extend the backtracking algorithm to work for such optimisation prblems, and in so doing derive the algorithmic techniques known as *Brach and Bound*

In addtion to backtracking, branch and bound has a scoring mechanism to always choose the most promising configuration to explore in each iteration.





