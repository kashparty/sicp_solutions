# SICP Answers

## Chapter 1

### Exercise 1.1

```scheme
10 = 10
(+ 5 3 4) = 12
(- 9 1) = 8
(/ 6 2) = 3
(+ (* 2 4) (- 4 6)) = 6
(define a 3) = a
(define b (+ a 1)) = b
(if (and (> b a) (< b (a * b))) b a) = 4
(cond ((= a 4) 6) ((= b 4) (+ 6 7 a)) (else 25)) = 16
(+ 2 (if (> b a) b a)) = 6
(* (cond ((> a b) a) ((< a b) b) (else -1)) (+ a 1)) = 16
```

### Exercise 1.2

```scheme
(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) (* 3 (- 6 2) (- 2 7)))
```

### Exercise 1.3

```scheme
(define (square x) (* x x))
(define (sum-squares a b) (+ (square a) (square b)))

(define (sum-squares-larger a b c)
  (cond ((and (< a b) (< a c)) (sum-squares b c))
        ((and (< b a) (< b c)) (sum-squares a c))
        ((and (< c a) (< c b)) (sum-squares a b))))
```

### Exercise 1.4

If `b` is greater than `0`, then `(if (> b 0) + -)` evaluates to the operator `+`, and so the absolute value of `b` is added to `a` by this procedure. If `b` is less than or equal to `0`, then `(if (> b 0) + -)` evaluates to the operator `-`, meaning that `b` is subtracted from `a` - this is equivalent to adding the absolute value of `b` to `a`, since we know that `b` is non-positive. The `if` statement determines which operator to use and returns the correct one based on the value of `b`.

### Exercise 1.5

If the interpreter is using applicative-order evaluation, it will first evaluate `test`, which is a procedure in the environment. Then, it evaluates `0`, which is a numeral whose value is the number `0`. Finally, it evaluates `(p)`, whose value is the value of `(p)` - this creates an infinite cycle and so the evaluation never finishes.

If the interpreter is using normal-order evaluation, it first substitutes the formal parameters in `(if (= x 0) 0 y)` as `(if (= 0 0) 0 (p))`. Then, since `if` is a special form, it evaluates the predicate, which evaluates to a true value, and so the consequent `0` is returned as the value of the expression - in finite time.

### Exercise 1.6

Since `new-if` is not a special form, it is evaluated using the general evaluation rule, which means that the `sqrt-iter` procedure is invoked recursively (due to applicative-order evaluation) before the expression can be evaluated, forming an infinite loop. This means that the program never terminates.

### Exercise 1.7

For small numbers, take $x = 0.0000005$. The square root of this number is roughly $0.00070710678$. Consider another number, $z = 0.0009$. Since the difference between $x$ and $z$ is less than $0.001$, a guess of $y = 0.03$ (the square root of $z$) would be acceptable as the value of the square root of $x$, even though this answer is $42$ times larger than the correct answer.

For very large numbers, the difference of $0.001$ will be subject to rounding errors and so it is possible that, for some inputs, the program never achieves an answer that is "good enough" and therefore does not terminate. One such input is $6969696969696969696969$.

The amended program is shown below:

```scheme
(define (sqrt-iter guess prev x)
  (if (good-enough? guess prev x)
      guess
      (sqrt-iter (improve guess x) guess x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess prev x)
  (< (/ (abs (- guess prev)) guess) 0.001))

(define (square x) (* x x))

(define (sqrt x)
  (sqrt-iter 1.0 0.0 x))
```

This works much better for small and large numbers. Both of the examples given above work well with this new procedure.

### Exercise 1.8

```scheme
(define (cbrt-iter guess prev x)
  (if (good-enough? guess prev x)
      guess
      (cbrt-iter (improve guess x) guess x)))

(define (good-enough? guess prev x)
  (< (/ (abs (- guess prev)) guess) 0.001))

(define (cube x) (* x x x))

(define (improve guess x)
  (/ (+ (/ x (* guess guess)) (* 2 guess)) 3))

(define (cbrt x) (cbrt-iter 1.0 0.0 x))
```

### Exercise 1.9

```scheme
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8)
9
```

```scheme
(+ 4 5)
(+ 3 6)
(+ 2 7)
(+ 1 8)
(+ 0 9)
9
```

The first process is recursive, whereas the second is iterative.

### Exercise 1.10

```scheme
(A 1 10)
(A 0 (A 1 9))
(A 0 (A 0 (A 1 8)))
(A 0 (A 0 (A 0 (A 1 7))))
(A 0 (A 0 (A 0 (A 0 (A 1 6)))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 1 5))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 4)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 3))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 8)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 16))))))
(A 0 (A 0 (A 0 (A 0 (A 0 32)))))
(A 0 (A 0 (A 0 (A 0 64))))
(A 0 (A 0 (A 0 128)))
(A 0 (A 0 256))
(A 0 512)
1024
```

```scheme
(A 2 4)
(A 1 (A 2 3))
(A 1 (A 1 (A 2 2)))
(A 1 (A 1 (A 1 (A 2 1))))
(A 1 (A 1 (A 1 2)))
(A 1 (A 1 (A 0 (A 1 1))))
(A 1 (A 1 (A 0 2)))
(A 1 (A 1 4))
(A 1 (A 0 (A 1 3)))
(A 1 (A 0 (A 0 (A 1 2))))
(A 1 (A 0 (A 0 (A 0 (A 1 1)))))
(A 1 (A 0 (A 0 (A 0 2))))
(A 1 (A 0 (A 0 4)))
(A 1 (A 0 8))
(A 1 16)
(A 0 (A 1 15))
(A 0 (A 0 (A 1 14)))
(A 0 (A 0 (A 0 (A 1 13))))
(A 0 (A 0 (A 0 (A 0 (A 1 12)))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 1 11))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 10)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 9))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 8)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 7))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 6)))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 5))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 4)))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 3))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 2)))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 8)))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 16))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 32)))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 64))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 128)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 256))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 512)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 1024))))))
(A 0 (A 0 (A 0 (A 0 (A 0 2048)))))
(A 0 (A 0 (A 0 (A 0 4096))))
(A 0 (A 0 (A 0 8192)))
(A 0 (A 0 16384))
(A 0 32768)
65536
```

```scheme
(A 3 3)
(A 2 (A 3 2))
(A 2 (A 2 (A 3 1)))
(A 2 (A 2 2))
(A 2 (A 1 (A 2 1)))
(A 2 (A 1 2))
(A 2 (A 0 (A 1 1)))
(A 2 (A 0 2))
(A 2 4)
(A 1 (A 2 3))
(A 1 (A 1 (A 2 2)))
(A 1 (A 1 (A 1 (A 2 1))))
(A 1 (A 1 (A 1 2)))
(A 1 (A 1 (A 0 (A 1 1))))
(A 1 (A 1 (A 0 2)))
(A 1 (A 1 4))
(A 1 (A 0 (A 1 3)))
(A 1 (A 0 (A 0 (A 1 2))))
(A 1 (A 0 (A 0 (A 0 (A 1 1)))))
(A 1 (A 0 (A 0 (A 0 2))))
(A 1 (A 0 (A 0 4)))
(A 1 (A 0 8))
(A 1 16)
(A 0 (A 1 15))
(A 0 (A 0 (A 1 14)))
(A 0 (A 0 (A 0 (A 1 13))))
(A 0 (A 0 (A 0 (A 0 (A 1 12)))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 1 11))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 10)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 9))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 8)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 7))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 6)))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 5))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 4)))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 3))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 2)))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 8)))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 16))))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 32)))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 64))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 128)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 256))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 512)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 1024))))))
(A 0 (A 0 (A 0 (A 0 (A 0 2048)))))
(A 0 (A 0 (A 0 (A 0 4096))))
(A 0 (A 0 (A 0 8192)))
(A 0 (A 0 16384))
(A 0 32768)
65536
```

`f` is a doubling function, so `(f n)` returns $2n$. `g` is a power-of-two function, so `(g n)` returns $2^n$. `h` is a power tower function, so `(h n)` returns ${2^{2^{2^{\cdots^{2}}}}}$.

### Exercise 1.11

```scheme
(define (fr n)
  (if (< n 3)
      n
      (+ (fr (- n 1)) (* 2 (fr (- n 2))) (* 3 (fr (- n 3))))))

(define (fi n)
  (f-iter n 2 1 0))

(define (f-iter n a b c)
  (if (< n 3)
      a
      (f-iter (- n 1) (+ a (* 2 b) (* 3 c)) a b)))
```

### Exercise 1.12

```scheme
(define (choose row col)
  (cond ((= row 0) 1)
        ((= col 0) 1)
        ((= row col) 1)
        (else (+
               (choose (- row 1) (- col 1))
               (choose (- row 1) col)))))
```

### Exercise 1.13

Let $\varphi = \left(1 + \sqrt5\right) / 2$ and $\psi = \left(1 - \sqrt5\right) / 2$. Let $\text{P}(n)$ be the proposition that $\text{Fib}(n) = \left(\varphi^n - \psi^n\right) / \sqrt5$. 

The base cases are $\text{P}(0)$ and $\text{P}(1)$. $\left(\varphi^0 - \psi^0\right) / \sqrt5 = 0 = \text{Fib}(0)$ so $\text{P}(0)$ holds. $\left(\varphi^1 - \psi^1\right) / \sqrt5 = \sqrt5 / \sqrt5 = 1 = \text{Fib}(1)$ so $\text{P}(1)$ holds.

Suppose that for some natural number, $k \ge 1$, propositions $\text{P}(k - 1)$ and $\text{P}(k)$ hold, so $\text{Fib}(k - 1) = \left(\varphi^{k - 1} - \psi^{k - 1}\right) / \sqrt5$ and $\text{Fib}(k) = \left(\varphi^k - \psi^k\right) / \sqrt5$. Then, by the definition of the Fibonacci numbers, we have:
$$
\begin{align}
\text{Fib}(k + 1) 
&= \text{Fib}(k) + \text{Fib}(k - 1) \\
&= \left(\varphi^k - \psi^k\right) / \sqrt5 + \left(\varphi^{k - 1} - \psi^{k - 1}\right) / \sqrt5 \\
&= \frac{\varphi^{k - 1}(1 + \varphi) - \psi^{k - 1}(1 + \psi)}{\sqrt5} \\
&= \frac{\varphi^{k-1}\varphi^2 - \psi^{k-1}\psi^2}{\sqrt5} \\
&= \left(\varphi^{k + 1} - \psi^{k + 1}\right) / \sqrt5 .
\end{align}
$$
So if $\text{P}(k - 1)$ and $\text{P}(k)$ are true, then $\text{P}(k + 1)$ is also true. Since $\text{P}(0)$ and $\text{P}(1)$ are true, by mathematical induction, $\text{P}(n)$ is true for all natural numbers, $n$.

So we now have that $\text{Fib}(n) = \left(\varphi^n - \psi^n\right) / \sqrt5$. Since $\psi = \left(1 - \sqrt5\right) / 2$, and $2 < \sqrt5 < 3$, we have that $-1 > 1 - \sqrt5 > -2$ and so $-0.5 > \psi > -1$. Hence we have shown that $|\psi| < 1$, meaning that for all $n > 0$, $\left|\psi^n\right| < 1$ and since $\sqrt5 > 2$, we can deduce that $\left|\psi^n / \sqrt5\right| < 0.5$. This means that the difference between $\varphi^n / \sqrt5$ and $\text{Fib}(n)$ is always less than $0.5$, meaning that $\text{Fib}(n)$ must be the closest integer to $\varphi^n / \sqrt5$.

### Exercise 1.14

![image-20210824132947215](SICPAnswers/assets/image-20210824132947215.png)

The order of growth of space is $\Theta(a + n)$ and the order of growth of number of steps used in the process is ???.

### Exercise 1.15

```scheme
(sine 12.15)
(p (sine 4.05))
(p (p (sine 1.35)))
(p (p (p (sine 0.45))))
(p (p (p (p (sine 0.15)))))
(p (p (p (p (p (sine 0.05))))))
(p (p (p (p (p 0.05)))))
```

The procedure `p` is applied 5 times.

Suppose there are $n$ steps in the process to evaluate `(sine a)`. Then after $n$ divisions by $3$, we achieve a number that is less than or equal to $0.1$. So we have $a / 3^n \le 0.1$ and therefore $10a \le 3^n$. Hence $n \ge \log_3(10a)$. Therefore the order of growth in space and number of steps is $\Theta(\log_3(n))$.

### Exercise 1.16

```scheme
(define (iter-expt base power ans)
  (define (even? n) (= (remainder n 2) 0))
  (define (square n) (* n n))
  (cond ((= power 0) ans)
        ((even? power) (iter-expt (square base) (/ power 2) ans))
        (else (iter-expt base (- power 1) (* ans base)))))

(define (expt base power) (iter-expt base power 1))
```

### Exercise 1.17

```scheme
(define (* a b)
  (define (double n) (+ n n))
  (define (halve n) (/ n 2))
  (define (even? n) (= (remainder n 2) 0))
  
  (cond ((= b 0) 0)
        ((even? b) (double (* a (halve b))))
        (else (+ a (* a (- b 1))))))
```

### Exercise 1.18

```scheme
(define (* a b)
  (define (miter a b ans)
  (define (double n) (+ n n))
  (define (halve n) (/ n 2))
  (define (even? n) (= (remainder n 2) 0))
  
  (cond ((= b 0) ans)
        ((even? b) (miter (double a) (halve b) ans))
        (else (miter a (- b 1) (+ ans a)))))
  (miter a b 0))
```

### Exercise 1.19

Let $a_n$ and $b_n$ be the values of $a$ and $b$ respectively after $n$ applications of $T_{pq}$. Then we have $a_1 = b_0q + a_0q + a_0p$ and $b_1 = b_0p + a_0q$ after one application of the transformation. After the second application, we have:
$$
\begin{align}
a_2 &= b_1q + a_1q + a_1p \\
&= (b_0p + a_0q)q + (b_0q + a_0q + a_0p)q + (b_0q + a_0q + a_0p)p \\
&= b_0pq + a_0q^2 + b_0q^2 + a_0q^2 + a_0pq + b_0pq + a_0pq + a_0p^2 \\
&= 2b_0pq + b_0q^2 + 2a_0q^2 + 2a_0pq + a_0p^2 \\
&= b_0(2pq + q^2) + a_0(2pq + q^2) + a_0(p^2 + q^2).
\end{align}
$$

$$
\begin{align}
b_2 &= b_1p + a_1q \\
&= (b_0p + a_0q)p + (b_0q + a_0q + a_0p)q \\
&= b_0p^2 + a_0pq + b_0q^2 + a_0q^2 + a_0pq \\
&= b_0p^2 + b_0q^2 + 2a_0pq + a_0q^2 \\
&= b_0(p^2 + q^2) + a_0(2pq + q^2).
\end{align}
$$

Therefore we have $p' = p^2 + q^2$ and $q' = 2pq + q^2$.

```scheme
(define (fib n)
  (fib-iter 1 0 0 1 n))

(define (fib-iter a b p q count)
  (cond ((= count 0) b)
        ((even? count)
         (fib-iter a
                   b
                   (+ (* p p) (* q q))   ; compute p'
                   (+ (* 2 p q) (* q q)) ; compute q'
                   (/ count 2)))
        (else (fib-iter (+ (* b q) (* a q) (* a p))
                        (+ (* b p) (* a q))
                        p
                        q
                        (- count 1)))))
```

### Exercise 1.20

```scheme
(gcd 206 40)

(if (= 40 0)
    206
    (gcd 40 (remainder 206 40)))

(gcd 40 (remainder 206 40))

(if (= (remainder 206 40) 0)
    40
    (gcd (remainder 206 40) (remainder 40 (remainder 206 40))))

(if (= 6 0)
    40
    (gcd (remainder 206 40) (remainder 40 (remainder 206 40))))

(gcd (remainder 206 40) (remainder 40 (remainder 206 40)))

(if (= (remainder 40 (remainder 206 40)) 0)
    (remainder 206 40)
    (gcd (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))

(if (= 4 0)
    (remainder 206 40)
    (gcd (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))

(gcd (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))

(if (= (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) 0)
    (remainder 40 (remainder 206 40))
    (gcd (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))))

(if (= 2 0)
    (remainder 40 (remainder 206 40))
    (gcd (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))))

(gcd (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))

(if (= (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))) 0)
    (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
    (gcd remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))) (remainder (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))))

(if (= 0 0)
    (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
    (gcd remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))) (remainder (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))))

(remainder (remainder 206 40) (remainder 40 (remainder 206 40)))

(remainder 6 (remainder 40 6))

(remainder 6 4)

2
```

`remainder` is called 14 times in if statements and then 4 more times to evaluate the final result and so it is called 18 times in total.

```scheme
(gcd 206 40)

(if (= 40 0)
    206
    (gcd 40 (remainder 206 40)))

(gcd 40 (remainder 206 40))

(gcd 40 6)

(if (= 6 0)
    40
    (gcd 6 (remainder 40 6)))

(gcd 6 (remainder 40 6))

(gcd 6 4)

(if (= 4 0)
    6
    (gcd 4 (remainder 6 4)))

(gcd 4 (remainder 6 4))

(gcd 4 2)

(if (= 2 0)
    4
    (gcd 2 (remainder 4 2)))

(gcd 2 (remainder 4 2))

(gcd 2 0)

(if (= 0 0)
    2
    (gcd 0 (remainder 2 0)))

2
```

In the applicative-order evaluation, only 4 `remainder` operations are actually performed.

### Exercise 1.21

The smallest divisors of $199$ and $1999$ are themselves, while the smallest divisor of $19999$ is $7$.

### Exercise 1.22

```scheme
(define (runtime) (current-milliseconds))

(define (timed-prime-test n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (prime? n)
      (report-prime n (- (runtime) start-time))
      (display "")))

(define (report-prime n elapsed-time)
  (display n)
  (display " *** ")
  (display elapsed-time)
  (newline))

(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b) (= (remainder b a) 0))

(define (prime? n)
  (= n (smallest-divisor n)))

(define (square n) (* n n))

(define (search-for-primes start end)
  (cond ((> start end) (display "\nDone."))
        ((= (remainder start 2) 0) (search-for-primes (+ start 1) end))
        (else (timed-prime-test start) (search-for-primes (+ start 1) end))))
```

The timing data does bear this out but only for larger numbers because my processor is too fast to show any time (i.e. 0 milliseconds) for numbers in the range of a million. This means that the results agree with the idea that the run time is proportional to the number of steps required for computation.

The three smallest primes larger than $1000$ are $1009$, $1013$ and $1019$. The three smallest primes larger than $10000$ are $10007$, $10009$ and $10037$. The three smallest primes larger than $100000$ are $100003$, $100019$ and $100043$. The three smallest primes larger than $1000000$ are $1000003$, $1000033$ and $1000037$. 

### Exercise 1.23

```scheme
(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (next test-divisor)))))

(define (next n)
  (if (= n 2)
      3
      (+ n 2)))
```

This modification does not halve the run time of the prime checking process. Before adding this modification, my computer took roughly $10$ milliseconds to check whether a number around $10000000000$ is prime or not, whereas now it takes roughly $6.5$ seconds, so the ratio is closer to $1.5$ than to $2$.

The difference comes form the fact that this version of the program has one more function call, as well as a predicate evaluation inside an if statement.

 ### Exercise 1.24

When increasing the size of the primes being tested for, the time taken by the process did not increase noticeably, and it more often than not decreased as primes became less frequent. Clearly the distribution of primes is having a larger effect on the time taken by the procedure than the constant increase in time as the size of the input is doubled.

### Exercise 1.25

This is not an optimal solution because it first calculates the exponential, whose result could be massive, and then takes a remainder. There are inefficiencies in working with massive numbers and so this would not be a good approach for a fast prime tester. The given approach ensures that the numbers being operated on are kept as small as possible.

Furthermore, it takes a lot more memory to store these massive numbers, which is being needlessly wasted given that there is a better solution.

### Exercise 1.26

When evaluating `(square (expmod base (/ exp 2) m))` the interpreter uses applicative-order evaluation and so first computes the value of `(expmod base (/ exp 2) m)` recursively and then substitutes that value for a parameter in the square procedure.

If the `square` procedure is changed to an explicit multiplication, the interpreter uses applicative-order evaluation and therefore evaluates the first argument, `(expmod base (/ exp 2) m)` and then evaluates the second argument, which is the same. This means that the recursive step is performed twice, meaning that the number of steps taken by the procedure calculated increases exponentially with the number of steps required to calculate the answer once, which has logarithmic growth - hence the procedure has an overall order of growth of number of steps required of $\Theta(2^{\log(n)}) = \Theta(n)$.

### Exercise 1.27

```scheme
(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder
          (square (expmod base (/ exp 2) m))
          m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))

(define (square n) (* n n))

(define (carmichael n)
  (define (try-it a)
    (= (expmod a n n) a))
  (define (car-iter i)
    (cond ((= i (- n 1)) true)
          ((try-it i) (car-iter (+ i 1)))
          (else false)))
  (car-iter 0))
```

### Exercise 1.28

```scheme
(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (check-square (remainder
          (square (expmod base (/ exp 2) m))
          m) m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))

(define (check-square n m)
  (cond ((= n 1) n)
        ((= n (- m 1)) n)
        ((= (remainder (* n n) m) 1) 0)
        (else n)))

(define (square n) (* n n))

(define (mr-test n)
  (define (try-it a)
    (= (expmod a (- n 1) n) 1))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((mr-test n) (fast-prime? n (- times 1)))
        (else false)))
```

### Exercise 1.29

```scheme
(define (sum term a next b)
  (if (> a b)
      0
      (+ (term a)
         (sum term (next a) next b))))

(define (simpson f a b n)
  (define (inc x) (+ x 1))
  (define (f-modified x)
    (cond ((= x 0) (f a))
          ((even? x) (* 2 (f (+ a (* x (/ (- b a) n))))))
          (else (* 4 (f (+ a (* x (/ (- b a) n))))))))
  (* (/ (/ (- b a) n) 3) (sum f-modified 0 inc n)))
```

With $n = 100$ the result is $0.25333333333333324$ and with $n = 1000$ the result is $0.2503333333333336$.

### Exercise 1.30

```scheme
(define (sum term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (+ (term a) result))))
  (iter a 0))
```

### Exercise 1.31

```scheme
(define (product-iter term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (* (term a) result))))
  (iter a 1))

(define (product-recursive term a next b)
  (if (> a b)
      1
      (* (term a) (product-recursive term (next a) next b))))

(define (factorial product x)
  (define (identity x) x)
  (define (inc x) (+ x 1))
  (product identity 1 inc x))

(define (factorial-iter x) (factorial product-iter x))
(define (factorial-recursive x) (factorial product-recursive x))

(define (approx-pi n)
  (define (odd-f x)
    (if (= (remainder x 2) 0) (+ x 3) (+ x 2)))
  (define (even-f x)
    (if (= (remainder x 2) 0) (+ x 2) (+ x 3)))
  (define (inc x) (+ x 1))
  (* 4.0 (/ (product-iter even-f 0 inc n)
            (product-iter odd-f 0 inc n))))
```

### Exercise 1.32

```scheme
(define (accumulate-recursive combiner null-value term a next b)
  (if (> a b)
      null-value
      (combiner (term a)
                (accumulate-recursive combiner null-value term (next a) next b))))

(define (accumulate-iter combiner null-value term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (combiner (term a) result))))
  (iter a null-value))

(define (sum term a next b)
  (accumulate-iter + 0 term a next b))

(define (product term a next b)
  (accumulate-iter * 1 term a next b))
```

### Exercise 1.33

```scheme
(define (filtered-accumulate combiner null-value predicate term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (if (predicate a)
            (iter (next a) (combiner (term a) result))
            (iter (next a) result))))
  (iter a null-value))
```

```scheme
(define (sum-prime-squares a b)
  (define (inc x) (+ x 1))
  (filtered-accumulate + 0 prime? square a inc b))
```

```scheme
(define (product-relative-prime n)
  (define (inc x) (+ x 1))
  (define (relative-prime m) (= (gcd n m) 1))
  (define (identity x) x)
  (filtered-accumulate * 1 relative-prime identity 1 inc (- n 1)))
```

### Exercise 1.34

When evaluating `(f f)`, the interpreter uses the definition of `f` to evaluate `(f 2)`, and then uses the same definition again to evaluate `(2 2)`, but since `2` is not a procedure, this will result in an error.

### Exercise 1.35

We know that the golden ratio $\varphi$ is a solution to the equation $x^2 = x + 1$, and by dividing both sides of this equation by $x$ (which is valid because $0$ is not a solution), we obtain the equation $x = 1 + 1/x$, which can be used in a fixed point search.

Using `(fixed-point (lambda (x) (+ 1 (/ 1 x))) 1.0)` I calculated $\varphi$ as $1.6180327868852458$.

### Exercise 1.36

```scheme
(define tolerance 0.00001)
(define (fixed-point f first-guess)
  (define (close-enough? v1 v2)
    (< (abs (- v1 v2))
       tolerance))
  (define (finish-guess guess)
    (display guess)
    (newline)
    (try guess))
  (define (try guess)
    (let ((next (f guess)))
      (if (close-enough? guess next)
          next
          (finish-guess next))))
  (try first-guess))

(fixed-point (lambda (x) (/ (log 1000) (log x))) 5.0)
(fixed-point (lambda (x) (/ (+ x (/ (log 1000) (log x))) 2)) 5.0)
```

Without average damping this takes $28$ steps. With average damping it takes only $8$ steps.

### Exercise 1.37

```scheme
(define (cont-frac-recursive n d k)
  (define (iter i)
    (if (= i k)
        (/ (n i) (d i))
        (/ (n i) (+ (d i) (iter (+ i 1))))))
  (iter 1))

(define (cont-frac-iter n d k)
  (define (iter result i)
    (if (= i 0)
        result
        (iter (/ (n i) (+ (d i) result)) (- i 1))))
  (iter 0 k))

(cont-frac-iter (lambda (i) 1.0)
           (lambda (i) 1.0)
           13)
```

When $k$ is $12$ or greater, the approximation is accurate to $4$ decimal places.

### Exercise 1.38

```scheme
(define (approx-e k)
  (define (n i) 1.0)
  (define (d i)
    (if (= (remainder i 3) 2)
        (* 2 (/ (+ i 1) 3))
        1))
  (+ 2 (cont-frac-iter n d k)))

(approx-e 10)
```

### Exercise 1.39

```scheme
(define (tan-cf x k)
  (define (iter i result)
    (if (= i 0)
        result
        (iter (- i 1) (/ (* x x) (- (- (* 2.0 i) 1) result)))))
  (/ (iter k 0) x))
```

### Exercise 1.40

```scheme
(define (cubic a b c)
  (lambda (x) (+ (* x x x) (* a x x) (* b x) c)))
```

### Exercise 1.41

```scheme
(define (double proc)
  (lambda (x) (proc (proc x))))
```

`(((double (double double)) inc) 5)` evaluates to 21 because `inc` is applied 16 times.

### Exercise 1.42

```scheme
(define (compose f g)
  (lambda (x) (f (g x))))
```

### Exercise 1.43

```scheme
(define (compose f g)
  (lambda (x) (f (g x))))

(define (repeated f n)
  (if (= n 1)
      f
      (compose f (repeated f (- n 1)))))
```

### Exercise 1.44

```scheme
(define dx 0.01)

(define (smooth f)
  (lambda (x) (/ (+ (f (- x dx)) (f x) (f (+ x dx))) 3)))

(define (n-smooth f n)
  ((repeated smooth n) f))
```

### Exercise 1.45

```scheme
(define (nth-root x n)
  (fixed-point ((repeated average-damp (floor (log n 2)))
               (lambda (y) (/ x (exp (* (- n 1) (log y))))))
               1.0))
```

### Exercise 1.46

```scheme
(define (iterative-improve good-enough? improve)
  (define (iterate guess)
    (if (good-enough? guess)
        guess
        (iterate (improve guess))))
  (lambda (x) (iterate x)))

(define (sqrt x)
  (define (good-enough? guess)
    (< (abs (- (* guess guess) x)) 0.001))
  (define (improve guess)
    (/ (+ guess (/ x guess)) 2.0))
  ((iterative-improve good-enough? improve) x))

(define tolerance 0.00001)
(define (fixed-point f first-guess)
  (define (close-enough? guess)
    (< (abs (- guess (f guess)))
       tolerance))
  (define (improve guess)
    (f guess))
  ((iterative-improve close-enough? improve) first-guess))
```