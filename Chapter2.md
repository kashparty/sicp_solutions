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

### Exercise 2.3

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

### Exercise 2.4

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

### Exercise 2.5

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

### Exercise 2.6

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