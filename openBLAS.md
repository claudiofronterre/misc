# How to install openBLAS library for R in Ubuntu

The standard base R installation makes use of BLAS library for matrix computations. However, the openBLAS library is much faster and optimised and can work also with multiple processors. First of all install the library `benchmarkme`. It contains the function `get_linear_algebra` that will show which libraries R is currently using for algebraic calculus (Note: it currently works only under Linux and Apple OS). Moreover, it allows us to run test to see the difference in speed after installing openBLAS.

```r
install.packages("benchmarkme")
library("benchmarkme")
get_linear_algebra()
```
If openBLAS is not the default, before installing it lets run a a set of standard benchmarks. We will use the function `benckmark_std` that runs 3 different types of benckmark. In particular, it will run the following tests:

* Programming benchmarks (5 tests):
  * Fibonacci numbers calculation (vector calc)
  * Grand common divisors of 1,000,000 pairs (recursion)
  * Creation of a 3500x3500 Hilbert matrix (matrix calc)
  * Creation of a 3000x3000 Toeplitz matrix (loops)
  * Escoufier's method on a 60x60 matrix (mixed)
* Matrix calculation benchmarks (5 tests):
	* Creation, transp., deformation of a 5000x5000 matrix
	* 2500x2500 normal distributed random matrix ^1000
	* Sorting of 7,000,000 random values
	* 2500x2500 cross-product matrix (b = a' * a)
	* Linear regr. over a 3000x3000 matrix (c = a \ b')
* Matrix function benchmarks (5 tests):
	* Cholesky decomposition of a 3000x3000 matrix
	* Determinant of a 2500x2500 random matrix
	* Eigenvalues of a 640x640 random matrix
	* FFT over 2,500,000 random values
	* Inverse of a 1600x1600 random matrix

```r
res_before <- benchmark_std()
plot(res_before)
```

If openBLAS was not the default then you can easily instally it from terminal using the following code

```
sudo apt-get install libopenblas-base libopenblas-dev
```

R should automatically detect the new library. Just to be sure check that openBLAS is now the used library using the following command from terminal. If it is not the default the select it from the list typing the correct number.

```
sudo update-alternatives --config libblas.so
```

And now run the benchmark again and compare the resutls. You will notice a considerable gain especially for matrix computations

```r
res_after <- benchmark_std()
plot(res_after)
```
Remember that by default openBLAS will use all the cores/threads available. This could be not the best thing to do in some cases because it could cause overhead. If you want to change the number of cores used you can do it with this function written by [Simon Fuller](https://github.com/simonfullernuim/OpenBlasThreads)

```r
require(inline)
openblas.set.num.threads <- cfunction( signature(ipt="integer"),
      body = 'openblas_set_num_threads(*ipt);',
      otherdefs = c ('extern void openblas_set_num_threads(int);'),
      libargs = c ('-L/opt/openblas/lib -lopenblas'),
      language = "C",
      convention = ".C"
      )
```

If we want to use only two cores it will be enough to use `openblas.set.num.threads(2)` and for that R session openBLAS will use only 2 threads.

For a more extensive guide on linear algebra libraries for R check the official R manual [section](https://cran.r-project.org/doc/manuals/r-release/R-admin.html#Linear-algebra). Moreover, [Brett Klamer](http://brettklamer.com/diversions/statistical/faster-blas-in-r/) provides a nice comparison of ATLAS, OpenBLAS and Intel MKL BLAS libraries. He also gives a description of how to install the different libraries.
