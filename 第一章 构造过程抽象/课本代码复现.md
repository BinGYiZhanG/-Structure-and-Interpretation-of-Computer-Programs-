### 埋下一个伏笔：正则序也可以成为非常有用的工具，我们将在第3章和第4章研究它的某些内在性质(P11,关于正则序的叙述)
### 前两章的所有过程应用，都是应用序，
### 关于正则序，应用序不是很懂,
```scheme
#lang racket
(define (square x)(* x x))

(define (sum-of-squares x y)
  (+ (square x)(square y)))

(define (f a)
  (sum-of-squares(+ a 1)(* a 2)))
```

#### 2，绝对值
##### a,
```scheme
(define (abs x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```
##### b,
```scheme
(define (abs x)
  (cond ((< x 0)(- x))
        (else x)))
```
#### 3，逻辑符合运算符
```scheme
(define x 8)
(and (> x 5) (< x 10))

out:
#t

(define x 10)
(and (> x 5) (< x 10))

out:
#f
```
##### b,定义逻辑复合运算符
```scheme
(define (>= x y)
  (or (> x y) (= x y)))
(define x 10)
(define y 8)
(>= x y)

(define (>= x y)
  (or (> x y) (= x y)))
  
(define (>= x y)
  (not (< x y)))

out:
#t
```
##### 注意：如何调用:(>= x y)


#### 1.1.7 实例：采用牛顿法求平方根
```scheme
#lang racket
;参数：一个被开方数，一个猜测值
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x)
                 x)))
;改进猜测的方式：就是求出他与被开方数以上一个猜测的平均值:
(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess x)
  (< (abs (- (square guess) x)) 0.001))

(define (square x)(* x x))

(define (sqrt x)
  (sqrt-iter 1.0 x))
  
> (sqrt 9)
3.00009155413138
> (square (sqrt 1000))
1000.000369924366
```
#### 斐波那契数列
##### a,
```scheme
(define (fib n)
  (cond ((= n 0) 0)
        ((= n 1) 1)
        (else (+ (fib (- n 1))
                 (fib (- n 2))))))
```
##### b,常数时间
```scheme
(define (fib n)
  (fib-iter 1 0 n))

(define (fib-iter a b count)
  (if (= count 0)
      b
      (fib-iter (+ a b) a (- count 1))))
 ```
      
#### 1.2.4 求幂
##### a.递归法
```scheme
(define (expt b n)
  (if (= n 0)
      1
      (* b (expt b (- n 1)))
  )
)
```
##### b.迭代法
```scheme
(define (expt b n)
  (expt-iter b n 1))

(define (expt-iter b counter product)
  (if (= counter 0)
      product
      (expt-iter b
                 (- counter 1)
                 (* b product))))
```
##### c.二分法   
```scheme
(define (fast-expt b n)
  (cond ((= n 0) 1)
        ((even? n)(square (fast-expt b (/ n 2))))
        (else (* b (fast-expt b (- n 1))))
  )
)

(define (square x)(* x x))
```
#### 1.2.5最大公约数
```scheme
(define (gcd a b)
  (if (=  b 0)
      a
      (gcd b (remainder a b))))
```

#### 1.2.6 素数检测
```scheme
(define (prime n)
  (= n (smallest-divisor n)))

(define (smallest-divisor n)
  (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b)
  (= (remainder b a) 0))

(define (square x)(* x x))
```
## 注:(prime n)不能写在最后，要写到程序开头才行，应该和c语言调用函数写在被调用函数之后一样
## 即:调用函数要写在被调函数之前

##### 费马检查
```scheme
(define (fast-prime n times)
  (cond ((= times 0) true)
        ((fermat-test n) (fast-prime n (- times 1)))
        (else false)))

(define (fermat-test n)
  (define (try-it a)
    (= (expmod a n n) a));检查a的n次幂取模是否等于a
  (try-it (+ 1 (random (- n 1)))));随机数a的选取通过过程random完成


(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder (square (expmod base (/ exp 2) m))
                    m))
        (else
         (remainder (* base (expmod base (- exp 1) m))
                    m)
         )
  )
)

> (fast-prime 6 2)
#t
```
