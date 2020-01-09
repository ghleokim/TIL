# AI/ML-different-types-of-AI-ML

## traditional algorithm vs machine learning

## Traditional

- rules are fixed.
- deterministic
- once written, it's finished.

## Machine Learning

- rules are not known ⇒ finding the rules: **Training**
- Trained Algorithm
- same input, same output
- deterministic
- whole process is non-deterministic

⇒ Main Problems that AI/ML is trying to solve: **Regression**, **Classification**

# Different types of AI/ML

1. Supervised Learning 지도학습
2. Unsupervised Learning 비지도학습
3. Reinforcement Learning 강화학습
4. Semi-supervised Learning 준지도학습

## 1. Supervised Learning

(`input data`, `expected output`)

Solving both **Regression** and **Classification**

ex) input data of image set - image classification between cat, dog, elephant and lion

```
input_data: image01, image02, ...
expected_output:
	cat dog ele lion
[[ 1,  0,  0,  0  ],
 [ 0,  1,  0,  0  ],
 [ 0,  0,  1,  0  ],
 ... ]
```

## 2. Unsupervised Learning

(`input data`)

Trying to understand the **data itself.**

**Principal Component Analysis(PCA)**

```
a set of correlated variables` ⇒ `a set of un-correlated variables
```

PCA is sensitive to the relative scaling of the original variables.

PCA is often used to visualize genetic distance and relatedness between populations.

PCA can be done by...

- Eigenvalue decomposition of data covariance(or correlation) matrix
- Singular Value Decomposition(SVD) of data matrix

Data should be normalized **before PCA.**

## 3.  Reinforcement Learning

(`input data`, `expected output`)

Use cases: Chess, Pac-man, etc.

**Training Process**

1. `input data` → [MODEL] → `predicted output`(from model)
2. Compare between predicted output and expected output (the difference becomes **loss-input**)
3. `input data + loss-input` → [MODEL] → `predicted output` : Feedback Loop
4. Repeat.

**Feedback Loop** is one of the most important thing in Reinforcement Learning.

**Reinforcement Learning vs Supervised Learning**

**The order of training data matters** in Reinforcement Learning.

## 4. Semi-supervised Learning

[ [ `input data`, `expected output` ], [ `input data`,             -                  ] ]

Used more frequently.

In reality, `the number of (input, expected) data` < `the number of (input) data`