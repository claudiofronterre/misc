# Useful things for Stan

## Matrix and vectors

The following code fragment shows all four ways to declare a two-dimensional
container of size `M x N`.
```
real b[M, N];         // b[m] : real[] (efficient)
vector[N] b[M];       // b[m] : vector (efficient)
row_vector[N] b[M];   // b[m] : row_vector (efficient)
matrix[M, N] b;       // b[m] : row_vector (inefficient)
```
If Sigma is a variable of type matrix, then Sigma[1] denotes the first row of Sigma and has the type `row_vector`.
