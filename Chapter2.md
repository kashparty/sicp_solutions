# Chapter 2

## Exercise 2.1

```scheme
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))

(define (make-rat n d)
  (let ((g (gcd (abs n) (abs d))))
    (cond ((and (not (< n 0)) (not (< d 0)))
           (cons (/ n g) (/ d g)))
          ((and (not (< n 0)) (< d 0))
           (cons (/ (- 0 n) g) (/ (- 0 d) g)))
          ((and (< n 0) (not (< d 0)))
           (cons (/ n g) (/ d g)))
          ((and (< n 0) (< d 0))
           (cons (/ (- 0 n) g) (/ (- 0 d) g))))))

(define (numer x) (car x))
(define (denom x) (cdr x))

(define (add-rat x y)
  (make-rat (+ (* (numer x) (denom y))
               (* (numer y) (denom x)))
            (* (denom x) (denom y))))

(define (sub-rat x y)
  (make-rat (- (* (numer x) (denom y))
               (* (numer y) (denom x)))
            (* (denom x) (denom y))))

(define (mul-rat x y)
  (make-rat (* (numer x) (numer y))
            (* (denom x) (denom y))))

(define (div-rat x y)
  (make-rat (* (numer x) (denom y))
            (* (denom x) (numer y))))

(define (equal-rat? x y)
  (= (* (numer x) (denom y))
     (* (numer y) (denom x))))

(define (print-rat x)
  (display (numer x))
  (display "/")
  (display (denom x)))
```

## Exercise 2.2

```scheme
(define (make-point x y) (cons x y))
(define (x-point p) (car p))
(define (y-point p) (cdr p))

(define (make-segment start-point end-point)
  (cons start-point end-point))
(define (start-segment segment) (car segment))
(define (end-segment segment) (cdr segment))

(define (midpoint-segment segment)
  (make-point (/ (+
                  (x-point (start-segment segment))
                  (x-point (end-segment segment)))
                 2)
              (/ (+
                  (y-point (start-segment segment))
                  (y-point (end-segment segment)))
                 2)))

(define (print-point p)
  (newline)
  (display "(")
  (display (x-point p))
  (display ", ")
  (display (y-point p))
  (display ")"))
```

## Exercise 2.3

```scheme
(define (square x) (* x x))
(define (sqrt x) (exp (/ (log x) 2)))

(define (distance p1 p2)
  (sqrt (+ (square
            (- (x-point p1)
               (x-point p2)))
           (square
            (- (y-point p1)
               (y-point p2))))))

(define (cosine-angle a middle b)
  (/ (+ (* (- (x-point a) (x-point middle))
           (- (x-point b) (x-point middle)))
        (* (- (y-point a) (y-point middle))
           (- (y-point b) (y-point middle))))
     (* (distance a middle)
        (distance b middle))))

(define (make-rectangle segment point)
  (cons segment point))

(define (main-length rect)
  (distance (start-segment (car rect))
            (end-segment (car rect))))

(define (other-length rect)
  (* (sin (acos (cosine-angle
                 (start-segment (car rect))
                 (end-segment (car rect))
                 (cdr rect))))
     (distance (end-segment (car rect))
               (cdr rect))))

(define (area-rectangle rect)
  (* (main-length rect)
     (other-length rect)))

(define (perimeter-rectangle rect)
  (* 2 (+ (main-length rect)
          (other-length rect))))
```

```scheme
(define (make-rectangle origin angle width height)
  (cons (cons width height) (cons origin angle)))

(define (main-length rect)
  (car (car rect)))

(define (other-length rect)
  (cdr (car rect)))

(define (area-rectangle rect)
  (* (main-length rect)
     (other-length rect)))

(define (perimeter-rectangle rect)
  (* 2 (+ (main-length rect)
          (other-length rect))))
```

## Exercise 2.4

```scheme
(car (cons x y))
((cons x y) (lambda (p q) p))
((lambda (m) (m x y)) (lambda (p q) p))
((lambda (p q) p) x y)
x
```

So the corresponding definition of `cdr` is

```scheme
(define (cdr z)
  (z lambda (p q) q))
```

## Exercise 2.5

```scheme
(define (square x) (* x x))
(define (is-divisible a b) (= (remainder a b) 0))

(define (exp base power)
  (cond ((= power 0) 1)
        ((is-divisible power 2) (square (exp base (/ power 2))))
        (else (* base (exp base (- power 1))))))

(define (cons a b)
  (* (exp 2 a) (exp 3 b)))

(define (num-divisions a b)
  (define (iter value ans)
    (if (not (is-divisible value b))
             ans
             (iter (/ value b) (+ ans 1))))
  (iter a 0))

(define (car pair) (num-divisions pair 2))
(define (cdr pair) (num-divisions pair 3))
```

## Exercise 2.6

Definition of one:

```scheme
(add-1 zero)
(lambda (f) (lambda (x) (f ((zero f) x))))
(lambda (f) (lambda (x) (f (((lambda (f) (lambda (x) x)) f) x))))
(lambda (f) (lambda (x) (f ((lambda (x) x) x))))
(lambda (f) (lambda (x) (f x)))
```

```scheme
(define one (lambda (f) (lambda (x) (f x))))
```

Definition of two:

```scheme
(add-1 one)
(lambda (f) (lambda (x) (f ((one f) x))))
(lambda (f) (lambda (x) (f (((lambda (f) (lambda (x) (f x))) f) x))))
(lambda (f) (lambda (x) (f ((lambda (x) (f x)) x))))
(lambda (f) (lambda (x) (f (f x))))
```

```scheme
(define two (lambda (f) (lambda (x) (f (f x)))))
```

Definition of add:

```scheme
(define add (lambda a b)
  (lambda (f) (lambda (x) ((a f) ((b f) x)))))
```

This expands `b` first and then expands `a` on the result of `b`'s expansion, and then wraps the result in the correct format.

## Exercise 2.7

```scheme
(define (add-interval x y)
  (make-interval (+ (lower-bound x) (lower-bound y))
                 (+ (upper-bound x) (upper-bound y))))

(define (mul-interval x y)
  (let ((p1 (* (lower-bound x) (lower-bound y)))
        (p2 (* (lower-bound x) (upper-bound y)))
        (p3 (* (upper-bound x) (lower-bound y)))
        (p4 (* (upper-bound x) (upper-bound y))))
    (make-interval (min p1 p2 p3 p4)
                   (max p1 p2 p3 p4))))

(define (div-interval x y)
  (mul-interval
   x
   (make-interval (/ 1.0 (upper-bound y))
                  (/ 1.0 (lower-bound y)))))

(define (make-interval a b) (cons a b))
(define (upper-bound x) (cdr x))
(define (lower-bound x) (car x))
```

## Exercise 2.8

The lower bound of the difference of two intervals is the difference between the lower bound of the first interval and the upper bound of the second interval. The upper bound of the difference of two intervals is the difference between the upper bound of the first interval and the lower bound of the second interval. This leads to the following definition for `sub-interval`:

```scheme
(define (sub-interval x y)
  (make-interval (- (lower-bound x) (upper-bound y))
                 (- (upper-bound x) (lower-bound y))))
```

## Exercise 2.9

Let $w$ be a function which maps an interval to its width.

Suppose there are two intervals, $x = [a, b]$ and $y = [c, d]$. Then the width of the first interval, $x$, is $w(x) = (b - a) / 2$ and the width of the second interval, $y$, is $w(y) = (d - c) / 2$.

When the intervals $x$ and $y$ are summed, the resulting interval is $z = [a + c, b + d]$. This means that the width of the resulting interval, $z$, is $w(z) = (b + d - a - c) / 2$, which can be written as $(b - a) / 2 + (d - c) / 2 = w(x) + w(y)$.

When the intervals $x$ and $y$ are subtracted, the resulting interval is $z = [a - d, b - c]$. This means that the width of the resulting interval, $z$, is $w(z) = (b - c - a + d) / 2$, which can be written as $(b - a) / 2 + (d - c) / 2 = w(x) + w(y)$.

Suppose $x = [2, 5]$ and $y = [-3, 7]$. We have $w(x) = 3/2$ and $w(y) = 5$ When these intervals are multiplied, the resulting interval is $z = [-15, 35]$, and we have $w(z) = 25$. Suppose instead that $x = [-15, -12]$, so $w(x)$ remains $3/2$. Then we have $z = [-105, 45]$, and so $w(z) = 75$. So for two pairs of intervals with the same widths, the width of the product can be different, and so in multiplication, the width of the resulting interval is not a function only of the widths of the intervals being multiplied.

Since division is defined in terms of multiplication in this implementation, it is an obvious conclusion that, for division also, the width of the resulting interval is not a function only of the widths of the intervals being multiplied.

## Exercise 2.10

```scheme
(define (div-interval x y)
  (if (and (not (> (lower-bound y) 0))
           (not (< (upper-bound y) 0)))
      (error "Potential division by zero")
      (mul-interval
       x
       (make-interval (/ 1.0 (upper-bound y))
                      (/ 1.0 (lower-bound y))))))
```

## Exercise 2.11

The nine cases are as follows. The only case requiring more than two multiplications is the third case.

1. If all the endpoints are positive, the lower bound is the lower bounds of the two intervals multiplied together, and the upper bound is the upper bounds of the two intervals multiplied together.
2. If all the endpoints are negative, the lower bound is the upper bounds of the two intervals multiplied together, and the upper bound is the lower bounds of the two intervals multiplied together.
3. If both the lower endpoints are negative and both the upper endpoints are positive, then the lower bound is the smaller of the following two values: the upper bound of the first multiplied by the lower bound of the second and the lower bound of the first multiplied by the upper bound of the second. The upper bound is the larger of the following two values: the lower bounds multiplied together and the upper bounds multiplied together.
4. If the lower bound of the first is negative and the rest of the endpoints are positive, then the lower bound is the lower bound of the first multiplied by the upper bound of the second, and the upper bound is the two upper bounds multiplied together.
5. If the lower bound of the second is negative and the rest of the endpoints are positive, then the lower bound is the upper bound of the first multiplied by the lower bound of the second, and the upper bound is the two upper bounds multiplied together.
6. If the upper bound of the first is positive and the rest of the endpoints are negative, then the lower bound is the upper bound of the first multiplied by the lower bound of the second and the upper bound is the two lower bounds multiplied together.
7. If the upper bound of the second is positive and the rest of the endpoints are negative, then the lower bound is the lower bound of the first multiplied by the upper bound of the second and the upper bound is the two lower bounds multiplied together.
8. If the endpoints of the first are both negative and the endpoints of the second are both positive, then the lower bound is the lower bound of the first multiplied by the upper bound of the second and the upper bound is the upper bound of the first multiplied by the lower bound of the second.
9. If the endpoints of the first are both positive and the endpoints of the second are both negative, then the lower bound is the upper bound of the first multiplied by the lower bound of the second and the upper bound is the lower bound of the first multiplied by the upper bound of the second.

Translating this into code:

```scheme
(define (>= a b) (not (< a b)))
(define (<= a b) (not (> a b)))

(define (mul-interval x y)
  (cond ((and (>= (lower-bound x) 0)
              (>= (lower-bound y) 0))
         (make-interval (* (lower-bound x)
                           (lower-bound y))
                        (* (upper-bound x)
                           (upper-bound y))))
        ((and (<= (upper-bound x) 0)
              (<= (upper-bound y) 0))
         (make-interval (* (upper-bound x)
                           (upper-bound y))
                        (* (lower-bound x)
                           (lower-bound y))))
        ((and (<= (lower-bound x) 0)
              (>= (upper-bound x) 0)
              (<= (lower-bound y) 0)
              (>= (upper-bound y) 0))
         (make-interval (min (* (upper-bound x)
                                (lower-bound y))
                             (* (lower-bound x)
                                (upper-bound y)))
                        (max (* (lower-bound x)
                                (lower-bound y))
                             (* (upper-bound x)
                                (upper-bound y)))))
        ((and (<= (lower-bound x) 0)
              (>= (upper-bound x) 0)
              (>= (lower-bound y) 0))
         (make-interval (* (lower-bound x)
                           (upper-bound y))
                        (* (upper-bound x)
                           (upper-bound y))))
        ((and (<= (lower-bound y) 0)
              (>= (upper-bound y) 0)
              (>= (lower-bound x) 0))
         (make-interval (* (upper-bound x)
                           (lower-bound y))
                        (* (upper-bound x)
                           (upper-bound y))))
        ((and (>= (upper-bound x) 0)
              (<= (lower-bound x) 0)
              (<= (upper-bound y) 0))
         (make-interval (* (upper-bound x)
                           (lower-bound y))
                        (* (lower-bound x)
                           (lower-bound y))))
        ((and (>= (upper-bound y) 0)
              (<= (lower-bound y) 0)
              (<= (upper-bound x) 0))
         (make-interval (* (lower-bound x)
                           (upper-bound y))
                        (* (lower-bound x)
                           (lower-bound y))))
        ((and (<= (upper-bound x) 0)
              (>= (lower-bound y) 0))
         (make-interval (* (lower-bound x)
                           (upper-bound y))
                        (* (upper-bound x)
                           (lower-bound y))))
        ((and (>= (lower-bound x) 0)
              (<= (upper-bound y) 0))
         (make-interval (* (upper-bound x)
                           (lower-bound y))
                        (* (lower-bound x)
                           (upper-bound y))))))
```

That was fun.

## Exercise 2.12

```scheme
(define (make-center-width c w)
  (make-interval (- c w) (+ c w)))

(define (make-center-percent c p)
  (let ((w (* (/ p 100) c)))
    (make-interval (- c w) (+ c w))))

(define (center i)
  (/ (+ (lower-bound i) (upper-bound i)) 2))

(define (width i)
  (/ (- (upper-bound i) (lower-bound i)) 2))

(define (percent i)
  (* 100 (/ (width i) (center i))))
```

## Exercise 2.13

Let $x$ be the interval with centre $c_x$ and percentage tolerance $p_x$ and $y$ be the interval with centre $c_y$ and percentage tolerance $p_y$, where $0 \le p_x, p_y, \le 1$. Then $x = [c_x - c_xp_x, c_x + c_xp_x]$ and, similarly, $y = [c_y - c_yp_y, c_y + c_yp_y]$. Since we are assuming that all the numbers are positive, the resulting interval will be $z = [(c_x - c_xp_x)(c_y - c_yp_y), (c_x + c_xp_x)(c_y + c_yp_y)]$, which simplifies to $[c_xc_y - c_xc_yp_y - c_xc_yp_x + c_xc_yp_xp_y, c_xc_y + c_xc_yp_y + c_xc_yp_x + c_xc_yp_xp_y]$. This interval obviously has a centre of $c_xc_y$, so the width of the interval is $c_xc_y(p_x + p_y + p_xp_y -1)$, and hence the percentage tolerance is just $p_x + p_y + p_xp_y - 1$.

## Exercise 2.14

An example that demonstrates that Lem is right is taking $R_1 = [2, 5]$ and $R_2 = [4, 7]$, where `par1` returns an interval of $[0.6666666666666666, 5.833333333333333]$ whereas `par2` returns an interval of $[1.3333333333333333, 2.9166666666666665]$.

The issue is that there is no way to tell which values are equal. Suppose there was a single defined value, $x = [c_x - c_xp_x, c_x + c_xp_x]$, then we should have $x/x = 1$, since, despite the uncertainty, the value itself is the same. This means that the answer should have no uncertainty, but since there is no way to tell whether one instance of $[c_x - c_xp_x, c_x + c_xp_x]$ is the same as another, uncertainty is introduced in the answer.

This is relevant since, when multiplying the equation from `par2` by $R_1R_2$ to get the equation for `par1`, the fractions in the denominator need to be cancelled out, but because there is no way to know whether those variables are identical, this introduces uncertainty where there should be none, making the results from `par1` and `par2` different.

## Exercise 2.15

As stated above, each mention of a variable in an equation is treated as a separate variable with a separate uncertainty, meaning that if a variable is repeated in the equation, the bounds will be less tight. This means that `par2` is "better" in the sense that it should produce tighter error bounds, because there are no repeated variables in the equation.

## Exercise 2.16

The general issue is that there is no concept of identity in the implementation, and it would be hard/impossible to link up seemingly separate variables to always have the same, and yet an unknown, value. In order to get around this problem, one could use equations which only mention each variable once, but this is again not possible since not all equations can be written in this form.

## Exercise 2.17

```scheme
(define (last-pair l)
  (if (null? (cdr l))
      l
      (last-pair (cdr l))))
```

## Exercise 2.18

```scheme
(define (reverse l)
  (define (reverse-iter current-rev current-l)
    (if (null? current-l)
        current-rev
        (reverse-iter (cons (car current-l)
                            current-rev)
                      (cdr current-l))))
  (reverse-iter (list) l))
```

## Exercise 2.19

```scheme
(define (cc amount coin-values)
  (cond ((= amount 0) 1)
        ((or (< amount 0) (no-more? coin-values)) 0)
        (else
         (+ (cc amount
                (except-first-denomination
                 coin-values))
            (cc (- amount
                   (first-denomination
                    coin-values))
            coin-values)))))

(define (no-more? l) (null? l))
(define (first-denomination l) (car l))
(define (except-first-denomination l) (cdr l))
```

The order of the list `coin-values` should have no impact on the answer produced by `cc` because it doesn't matter whether you start building the change from the lowest denomination or the highest, as all combinations of coins are considered.

## Exercise 2.20

```scheme
(define (same-parity x . rest)
  (define (filter-parity l p)
    (if (null? l)
        l
        (if (= (remainder (car l) 2) p)
            (cons (car l) (filter-parity (cdr l) p))
            (filter-parity (cdr l) p))))
  (filter-parity (cons x rest) (remainder x 2)))
```

## Exercise 2.21

```scheme
(define (square x) (* x x))

(define (square-list-1 items)
  (if (null? items)
      null
      (cons (square (car items)) (square-list-1 (cdr items)))))

(define (square-list-2 items)
  (map square items))
```

## Exercise 2.22

The first implementation produces the answer list in the reverse order since each next element in the list is paired with the previous answer using `cons`, putting the new element in the front, meaning that the last element must go at the front of the list and the first must go at the back.

The second method achieves nothing but to reverse the meaning of `car` and `cdr` when accessing the result. Even though, when written out, the list is now in order, the first element is still the most nested, and now the items of the list are on the right-hand side of their pairs, which means that the result is not a list.