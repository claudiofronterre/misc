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
