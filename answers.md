# CMPS 2200 Assignment 4
## Answers

**Name:**Zack Mitra

Place all written answers from `assignment-04.md` here for easier grading.


**1 Quicksort probabilities**

- **1a. (5 pts)** What is the probability that Quicksort does $\Omega({n^2})$
  comparisons? 
   Using Markov's inequality, let X represent the number of comparisons performed by Quicksort. Since its expected work is O(nlog(n)), we set E[X] = O(nlog(n)) and alpha = n^2.
  P(X >= n^2) <= E[X] / n^2 = O(nlog(n) / n^2) = O(log(n) / n).
  Therefore, the probability that Quicksort performs Omega(n^2) comparisons is at most O(log(n) / n). This upper bound approaches zero as n increases.



- **1b. (5 pts)** What is the probability that Quicksort does $10^c n \lg n$
comparisons, for a given $c>0$? What does this say about the
deviation of the actual work from the expected work for Quicksort?
  Let X represent the number of comparisons performed by Quicksort. Since E[X] = O(nlog(n)), there exists a constant K such that E[X] <= K nlog(n). Using Markov's inequality with alpha = 10^c nlog(n):
  P(X >= 10^c nlog(n)) <= E[X] / (10^c nlog(n)) <= K / 10^c = O(10^(-c)).
  Therefore, as c increases, the probability of Quicksort performing significantly more work than its expected running time decreases exponentially in c. This indicates that very large deviations above the expected work become increasingly unlikely.

**2 From "Maybe" to "Definitely"**

- **2a. (10 pts)** Give an algorithm to improve the guaranteed success probability and state its work.
To construct A', run algorithm A independently multiple times and verify each result using checker C. If a result is correct, return it immediately. Otherwise, continue until a maximum number of attempts has been reached.
Since each run succeeds with probability epsilon, the probability that all k independent runs fail is (1 - epsilon)^k. Therefore, the probability of obtaining at least one correct result is 1 - (1 - epsilon)^k.
To guarantee a success probability of at least delta, choose:
k = ceil(log(1 - delta) / log(1 - epsilon)).
Each run of A and its verification requires O(w(n)) work since v(n) is O(w(n)). Therefore, the worst-case work of A' is O(k * w(n)), or equivalently O(w(n) * ceil(log(1 - delta) / log(1 - epsilon))).



- **2b. (6 pts)** Show how to convert $\mathcal{A}$ into an
  algorithm that always produces the correct result, but has an
  expected runtime that depends on $w(n)$ and a success probability
  $\epsilon$.

  To guarantee a correct result, repeatedly run algorithm A and verify its output using checker C. If the output is incorrect, run A again independently until C confirms that the result is correct. Then return that result.
  Since each run succeeds with probability epsilon, the number of attempts follows a geometric distribution, with an expected number of attempts equal to 1/epsilon.
  Each attempt, including verification, takes O(w(n)) work because v(n) is O(w(n)). Therefore, the total expected work is O(w(n)/epsilon).
  Since epsilon > 0, the probability that the algorithm continues forever is zero. Thus, the algorithm returns a verified correct answer with probability 1, although there is no fixed worst-case bound on the number of attempts.

**3 Determinism versus Randomization in Quicksort**


- **3b. (8 pts)** Compare running times using `compare-qsort` between variants of
Quicksort and the provided implementation of selection sort (`ssort`).
I compared selection sort, fixed-pivot Quicksort, and randomized Quicksort using randomly shuffled and already sorted lists of sizes 100, 200, 400, and 800.
For randomly shuffled lists, both Quicksort implementations performed significantly better than selection sort as the input size increased. At n = 800, selection sort took 4.925 ms, fixed-pivot Quicksort took 0.713 ms, and randomized Quicksort took 0.787 ms. This is consistent with selection sort's O(n^2) complexity and Quicksort's expected O(nlog(n)) complexity.
For already sorted lists, fixed-pivot Quicksort performed much worse. At n = 800, it took 16.793 ms compared to 0.713 ms on shuffled inputs. This occurs because selecting the first element as the pivot produces highly unbalanced partitions, resulting in O(n^2) work.
Randomized Quicksort remained efficient on both input types, taking 0.787 ms for shuffled inputs and 0.808 ms for sorted inputs at n = 800. Random pivot selection reduces the likelihood of consistently unbalanced partitions, giving an expected running time of O(nlog(n)) regardless of the initial input order.
Overall, the experimental results agree with the theoretical complexities of the three algorithms and demonstrate the advantage of randomized pivot selection.



- **3c. (8 pts)** Compare the fastest of your sorting implementations to the Python sorting function `sorted`, conducting the tests in 3b above. 
I compared randomized Quicksort with Python's built-in Timsort using randomly shuffled and already sorted lists of 8,000 elements.
For the randomly shuffled list, randomized Quicksort took 11.039 ms, while Timsort took 0.604 ms, making Timsort approximately 18.3 times faster.
For the already sorted list, randomized Quicksort took 8.527 ms, while Timsort took only 0.030 ms, making Timsort approximately 284 times faster.
Although both algorithms have O(nlog(n)) expected or worst-case complexity, Timsort is highly optimized and can take advantage of existing order in the input. It also has an O(n) best-case running time for already sorted lists. Our Quicksort implementation uses Python recursion and creates additional lists during partitioning, resulting in more overhead.
Overall, Timsort performed significantly better in both experiments, especially when the input was already sorted.