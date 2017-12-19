# ������ �������� �������

���� ���� �������� �������� � ���������� ���� ����������� ���� �����. ������
�����������:
_������ ������� � ���� ������, 401-�_

## ���������

- [����������� ��������� �������������](#�����������-���������-�������������)
  - [K ��������� ������� (KNN)](#k-���������-�������-knn)
  - [K ���������� ��������� ������� (kwKNN)](#k-����������-���������-�������-kwknn)
  - [������������ ���� (PW)](#������������-����-pw)
  - [������������� ������� (PF)](#�������������-�������-pf)
  - [STOLP](#stolp)
- [����������� ��������������](#�����������-��������������)
  - [�������������� �������� (Plug-in)](#��������������-��������-plug-in)

## ����������� ��������� �������������

__�������� ������������__ ����������, ��� ������ �������� ������������� ������ ������.

����������� ��������� ������������� �������� �� __�������� ������������__,
������� ������� � ���, ��� <u>������ �������� ������������� ������ ������</u>.

�������, ������������ "��������" �������� �������� __����� ��������__.
��� ������� ������������ ��������� �������:

![](http://latex.codecogs.com/svg.latex?%5Clarge%20%5Crho%3A%20%28X%20%5Ctimes%20X%29%20%5Crightarrow%20%5Cmathbb%7BR%7D)
(������� ����������)

_����������� ��������� �������������_ �������� �� ������� �������� �������� � �������
����� ��������� ������� _����������_. ����������, ��� ���������� ������, ��� ������
������� ������ ���� �� �����.

<u>����������</u>. ����� ��������� �������� ������������� �� �������� ������� ����������
������� ������� `test()`.

### K ��������� ������� (KNN)

��� ������ �������� ������� _u_ � ������ _y_ �������� ���������� ���������
�������:
![](http://latex.codecogs.com/svg.latex?%5Clarge%20W%28i%2C%20u%29%20%3D%20%5Bi%20%5Cleq%20k%5D)
, ��� _i_ ���������� ������� ������ �� ���������� � ����� _u_.

����������, ������� ���� ����������� ��������� �������:
```
mc.KNN.w = function(i, k) +(i <= k)
```

������� �������, �������� �������� _k_ ��������� ������� � ����������
��� �����, ������� ����� ��������� ����������� ������� ���������� ���.

���������� �������� ����������� ��������� �������:
```
mc.KNN = function(sortedDistances, k) {
    orderedDistances = 1:length(sortedDistances)
    names(orderedDistances) = names(sortedDistances)

    weights = mc.KNN.w(orderedDistances, k)
    weightsByClass = sapply(unique(names(weights)), mc.sumByClass, weights)

    bestClass = names(which.max(weightsByClass))
}
```
��� `sortedDistances` � ������ ���������� �� ����������� `u` �� ���� ����� ���������
�������. `names(sortedDistances)` � ������������ ������� ��� ������ ����� �������.

���������� ��������� �������� ��
[������](oRRRlova/KNN.R)

#### ������ ���������

������������ �������� �� ������� ������ ������:

![LOO ��� KNN...](IMG/KNN.png)

�������� ���� ��������� ����������� (5 ������) ��� ��������� _k_, ������ ��� ����� _k_, �������
 � _k > 95_, ������
������������ ������. ��� ����������� ���, ��� ������� ������ ��������
![](http://latex.codecogs.com/svg.latex?%5Clarge%20W%28i%2C%20u%29%20%3D%20%5Bi%20%5Cleq%20k%5D)
����� �� ��������� ������� ���������, � ��������� ���� �� �������. ��-��
����� �� �������, ��� ��������� ����� ������ �� ����� _u_ ������ ��
������������� � ����� �� �����, ��� � �������, ����������� �
���������������� ��������.

__�����:__
- ����� � ����������
- �������� ���������� ��� ��������� ���������� _k_

__������:__
- ���������� ������� ��� ������� �������
- ������������� ����� ��������
![](http://latex.codecogs.com/svg.latex?%5Clarge%20O%28n%20%5Clog%20n%29)
, ��� ��� ������� ���������� ����� �� ����������
- ������ ����� ����������
- ����������� ������ ��������
- � ������ ���������� ����� ������� �������� �������� �����
- �� ��� ����� � ���������� ����������� ����� �������

### K ���������� ��������� ������� (kwKNN)

��� ������ �������� ������� _u_ � ������ _y_ �������� ���������� ���������
�������:
![](http://latex.codecogs.com/svg.latex?%5Clarge%20W%28i%2C%20u%29%20%3D%20%5Bi%20%5Cleq%20k%5D%20w%28i%29)
, ��� _i_ ���������� ������� ������ �� ���������� � ����� _u_, � 
![](http://latex.codecogs.com/svg.latex?%5Clarge%20w%28i%29) � ������
��������� ������� ����. ��������� �������� __����������� KNN__ ����������
�� __KNN__.

�� �� ����� ��������� ��������� ������� ����:
![](http://latex.codecogs.com/svg.latex?%5Clarge%20w%28i%29%20%3D%20%5Cfrac%7Bk%20&plus;%201%20-%20i%7D%7Bk%7D)

���������� ������� ���� ����������� ��������� �������:
```
mc.kwKNN.w = function(i, k) +(i <= k) * (k + 1 - i) / k
```

��� �� �������� �����, ����� ������� ����, �� __KNN__ �� ����������.
��������� `mc.kwKNN.w` ������ `mc.KNN.w` ������� ����������� ��������.

���������� ��������� �������� ��
[������](oRRRlova/kwKNN.R)

#### ������ ���������

������������ �������� �� ������� ������ ������:

![LOO ��� ����������� KNN...](IMG/kwKNN.png)

�������� ������������� ��������� ����������� ��� ����� _k_, ���������� ������������ ������
� ����������� ����� ���� � ���� ��������.

�������� __���������� k �������__ ����������� ���������� �� ��������
__KNN__ ���, ��� <u>��������� ������� ��������</u> ��� �������������. ���
�����, ������� � ����� _u_ ������� ����� ������ �� ��� ������� �������, ���
�������. ������ ��-�� ���� �����������, ��� ������� _k_ ������� �����
������ ��������������, ������� �������
![](http://latex.codecogs.com/svg.latex?%5Clarge%20w%28i%29)
������� �������� ���������.

__�����:__
- ����� � ����������
- �������� ���������� ��� ����� _k_

__������:__
- ���������� ������� ��� ������� �������
- ������������� ����� ��������
![](http://latex.codecogs.com/svg.latex?%5Clarge%20O%28n%20%5Clog%20n%29)
, ��� ��� ������� ���������� ����� �� ����������
- ������ ����� ����������
- � ������ ���������� ����� ������� �������� �������� �����
(������ ����� ��������, ��� ��� ������ ����� ����������� ������ �����)
- �� ��� ����� � ���������� ����������� ����� �������

### ������������ ���� (PW)

��� ������ �������� ������� _u_ � ������ _y_ �������� ���������� ���������
�������:

![](http://latex.codecogs.com/svg.latex?%5Clarge%20W%28i%2C%20u%29%20%3D%20K%28%5Cfrac%7B%5Crho%28u%2C%20x%5Ei_u%29%7D%7Bh%7D%29)
, ��� 
![](http://latex.codecogs.com/svg.latex?%5Clarge%20K%28z%29) � ������� ����.

���� ����� ����������� 5 ����� ����:
- ������������� ![](http://latex.codecogs.com/svg.latex?%5Clarge%20R%28z%29%20%3D%20%5Cfrac%7B1%7D%7B2%7D%20%5B%7Cz%7C%20%5Cleq%201%5D)
- ����������� ![](http://latex.codecogs.com/svg.latex?%5Clarge%20T%28z%29%20%3D%20%281%20-%20%7Cz%7C%29%20%5Ccdot%20%5B%7Cz%7C%20%5Cleq%201%5D)
- ������������ ![](http://latex.codecogs.com/svg.latex?%5Clarge%20Q%28z%29%20%3D%20%5Cfrac%7B15%7D%7B16%7D%20%281%20-%20z%5E2%29%5E2%20%5Ccdot%20%5B%7Cz%7C%20%5Cleq%201%5D)
- ������������ ![](http://latex.codecogs.com/svg.latex?%5Clarge%20E%28z%29%20%3D%20%5Cfrac%7B3%7D%7B4%7D%20%281%20-%20z%5E2%29%20%5Ccdot%20%5B%7Cz%7C%20%5Cleq%201%5D)
- ����������� (���������� �������������)

����������� ���������� ����:
```
mc.kernel.R = function(r) 0.5 * (abs(r) <= 1) #�������������
mc.kernel.T = function(r)(1 - abs(r)) * (abs(r) <= 1) #�����������
mc.kernel.Q = function(r)(15 / 16) * (1 - r ^ 2) ^ 2 * (abs(r) <= 1) #������������
mc.kernel.E = function(r)(3 / 4) * (1 - r ^ 2) * (abs(r) <= 1) #������������
mc.kernel.G = function(r) dnorm(r) #�����������
```

� ��������� ���� ����� ����������� ����������.
������ �� ������� � �������� ������������� ��� ������ �����.
���������� ���� _����������� ����_ (��. ����).

__�������� �������:__ �������� ��� ���������������� ����� _u_ ������
����������, �������� _h_. ��� �����, �� �������� � ��� ����������,
����������� (����� ������������). ��� ���������, ����������� ���,
�����������, � ����� � ���������� ����� ��������� �����������.

����������� ���������� ���������:
```
mc.PW = function(distances, u, h) {
    weights = mc.PW.kernel(distances / h)
    classes = unique(names(distances))

    weightsByClass = sapply(classes, mc.sumByClass, weights)

    if (max(weightsByClass) == 0) return("") #�� ���� ����� �� ������ � ����

    return(names(which.max(weightsByClass)))
}
```
��� `distances` � ���������� �� ����� `u` �� ���� ����� �������,
`names(distances)` � ������������ ������� ����� �������.

���������� ��������� �������� ��
[������](oRRRlova/PW.R)

#### ������ ���������

������������ �������� �� ������� ������ ������:

_������������� ����:_
![LOO ��� PW...](IMG/PW_R.png)

_����������� ����:_
![LOO ��� PW...](IMG/PW_T.png)

_������������ ����:_
![LOO ��� PW...](IMG/PW_Q.png)

_���������� ����:_
![LOO ��� PW...](IMG/PW_E.png)

�������� ������ ���� ���������� ��� _h_ ���� � ��������� ���������.
���� �������� ������� �� ��������� ���������������� �����. ���� _h_ �������
������� ���������, ���������� �����, ��������� � ������������� ������� ����������.
���� _h_ ������� ������� �������, �� ������ ��������� ���
������������� ������� ������� �����. ������ ����� ��������, ��� � ��������� �����
���� ������� ����� ������� ������, � �������� ��������� ��� ������� _h_ ������ �� ���
������. ��������� ����� (�� ������ ������� ����������� ����������)
�������� _�������������_, ��� ��� ����
���� ����� ������ ���� ����������.

��� ������������� ���� ����� ������� ���������� � ��� �� ��������
���������������� �����, �� �������� �� � ���� ����. ���� ����������
��������� ����������� ����.

_����������� ����:_
![LOO ��� PW...](IMG/PW_G.png)

�������� ������������� ���� ����� � ��������� � ���������� (���� �����, ���
__KNN__). �� ����� ������������������ �������� �������������,
 ������ ����� � ��� �����������, �������� __KNN__ � __����������
KNN__ �� ��������.

__�����:__
- ����� � ����������
- ������� �������� ������������� ��� ��������� ���������� _h_
- ��� ����� � ���������� ����������� ����� �������
- ������������� ����� ��������
![](http://latex.codecogs.com/svg.latex?%5Clarge%20O%28n%29)
, ��� ��� �� ������� ����������

__������:__
- ���������� ������� ��� ������� �������
- ������ ����� ����������
- � ������ ���������� ����� ������� �������� �������� �����
(������ ����� ��������, ��� ��� ������ ����� ����������� �����)
- �������� ��������� _h_ ���������� ��������� ��������������, ��������
��������� ������������ �����
- ���� �� ���� ����� �� ������ � ������ _h_, �������� �� �������� ��
���������������� (�� ��������� ��� ������������ ����)

### ������������� ������� (PF)

��� ������ �������� ������� _u_ � ������ _y_ �������� ���������� ���������
�������:

![](http://latex.codecogs.com/svg.latex?W_y%28i%2C%20u%29%20%3D%20%5Cgamma_i%20%5Ccdot%20K%28%5Cfrac%7B%5Crho%28u%2C%20x_u%5Ei%29%7D%7Bh_i%7D%29%2C%20%5Cgamma_i%20%5Cgeq%200%2C%20h_i%20%3E%200)
, ��� 
![](http://latex.codecogs.com/svg.latex?%5Clarge%20K%28z%29) � ������� ����.

� ����������� ���������� ����������� ������������ ����.
�������� ��������� ������ ���� ����������
![](http://latex.codecogs.com/svg.latex?%5Cgamma_i), ������� �����������
_h_ �������� �������.

__�������� �������:__ �������� ��� ������� ���������� ������� _x_ ������
����������, ������� _h_ � ���� ����������� (����������)
![](http://latex.codecogs.com/svg.latex?%5Cgamma_i).

����������� ���������� ������� �������������:
```
mc.PF = function(distances, potentials, h) {
    weights = potentials * mc.PF.kernel(distances / h)
    classes = unique(names(distances))

    weightsByClass = sapply(classes, mc.sumByClass, weights)

    if (max(weightsByClass) == 0) return("") #�� ���� ����� �� ������ � ����

    return(names(which.max(weightsByClass)))
}
```
��� `potentials` � `h` � ������� ����������� � �������� ��� ������ ����� �������
��������������.

������, ������, ��� ������������ ����������, �� ���������� ���������. ���� �������
����� ������ � ����������� _PF_.

<u>��������</u>:
��������� ���������� ����������� ������. �����, ���� ���������� ������ �������������
�� ��������� ������� �������, �������� �������� ����� _x_ �� �������. ���� ��� ���
������������� ����������� �������, ����������� ��������� �� 1 � �������������
����� ���������� ������.

������� ���������� ��������� �������:
```
mc.PF.potentials(points, classes, h, maxMistakes)
```
��� `points` � `classes` � ������ ����� � �� ������� ��������������,
`h` � ������ ��������������� �������� ������ �����,
`maxMistakes` � ������������ ���������� ���������� ������.

���������� ��������� �������� ��
[������](oRRRlova/PF.R)

#### ������ ���������

������������ �������� �� ������� ������ ������. .��� ��� ����� ������ ��������
������
![](http://latex.codecogs.com/svg.latex?%5Cgamma_i), � �� _h_, ��
������� �� ����� ������� (������ ��� ������� ����� _h = 1_, ��� ��� ���
�������� �� ��������� ��������� �������; ��������� �������� _h = 0.25_).

�������� �������� ����������� ��������� (4 ������) �� 16 ��������:

![���������� PF...](IMG/PF.png)

�������� ������� ������� � ��������� � ����������, ��� ��������������
���������. � ���� �� ������ ���� ����������� �������� ������ �����. �
���������, ��� ����� ������� ����������, ��� ��� �������� ��������
��������� (��������� _x_ � �������� ������� ��������� �������). ���
������� ��������� �� ����� � ��� �� �������, �� ���������� ������ �����
������, ������ �������� ������, � ����� ������ ���������� ��������.

__�����:__
- ��������� ������� �� _2n_ ����������

__������:__
- ���������� ������� ��� ������� �������
- ��������� _h_ ���������� ��������� ��������������, ��������
� �� ������� �� ��������� �������
- ���� �� ���� ����� �� ������ � ������ _h_, �������� �� �������� ��
���������������� (�� ��������� ��� ������������ ����)
- �������� ��������
- ������� ����� ����������� ���������
- �������������� ����� ������ (��� ��������� ������ ������ ����� ������
����������� ����������)

### STOLP

�������� ��������� ����� �������� ��������:

- _���������_ � �������� ������������� �������. ���� ����������������
������ ������ � �������, ��, ������ �����, �� ����������� ���� �� ������.
- _���������������_ � ������ ��������
������� ��������� ���� �� ������. ���� �� ������� �� �������, ��� �����������
�� ��������� �� �������� �������������.
- _�������_ � ��������� � ��������� �������� ������ ������. ��� �������,
- �� �������� ������ �������� �������� �������������.

������� **STOLP** ��������� �� ������� ������� � ���������������
�������, �������� ���� ������ ���������� ���������. ����� �������
���������� �������� �������������, ����������� ����� ������ � �����������
����� ������������� ��������. ������� ������� **STOLP** � ��������
������ ������.

�� ���������� ������� �������:

![](http://latex.codecogs.com/svg.latex?M%28x_i%29%20%3D%20W_%7By_i%7D%28x_i%29%20-%20%5Cunderset%7By%20%5Cin%20Y%20%5Csetminus%20y_i%7D%7Bmax%7DW_y%28x_i%29%29%29)

![](http://latex.codecogs.com/svg.latex?W_y%28x_i%29) �������� �������
�������� � ������� �� ���������� ��������� �������������.

���������� ������� ���� ����������� ��������� �������:
```
mc.STOLP.w = function(sortedDistances, indecies, k) {
    if (length(indecies) == 0) return(0)

    orderedDistances = 1:length(sortedDistances)
    names(orderedDistances) = names(sortedDistances)
    weights = mc.kwKNN.w(orderedDistances, k)

    weights = weights[indecies]
    if (length(weights) == 0) return(0)

    weightsByClass = sapply(unique(names(weights)), mc.sumByClass, weights)
    max(weightsByClass)[1]
}
```
��� `indecies` � ������� �����, ��� ������� �� ���������� ����������, ����
`indecies < 0` � ������� �����, ������� ���������� �������� ����� ���������.
������� �������� �� ������� ���� __KNN__, ������� ���������� ��������������
�������� `k` � ������������ `mc.kwKNN.w`. ���������� __STOLP__ ��� ������
���������� ������������� ����� ������� ����������.

����������� ���������� ������� �������:
```
mc.STOLP.M = function(points, classes, u, class) {
    k = 4
    dist = mc.distances(points, u)
    names(dist) = classes
    sortedDistances = sort(dist)

    #������� ������������ ������� ������
    uIndecies = which(sapply(names(sortedDistances), function(v) any(v == class)))
    m1 = mc.STOLP.w(sortedDistances, uIndecies, k)

    #������ ������������ ������ �������
    m2 = mc.STOLP.w(sortedDistances, - uIndecies, k)

    #������
    m1 - m2
}
```
��� `class` � ����� _Xi_-� ����� �������.

��� ������� ������������ ��� ��������������� ��� ��������� ��������� __STOLP__.

<u>��������</u>: 
�������� ������� ��� ������� �� ��������� �������.
����� �� ������� ������ ���������� �� ���������� �������
(� ����� ������� ��������) � ��� ������������ � ������.
�����, ���� ���������� ������ �� ���� ������� �� ����� ������������� �������,
�������� �� ���������� �������� ����� � ����� ��������� �������� � ���������
� ������.

�������� ���������� ��������� ��������:
```
mc.STOLP = function(points, classes, mistakes)
```
��� `mistakes` � ���������� ���������� ������.

������� �� ������� ������������ ������������� � ��������� � ������ ���������.
������������ ��� ��������� �������: �������� �������� ��� ������� �
������������� ��������, ��������� � ����� ���� �������� ���, � ��������
���������� ����� ������� ������ ����. ���� ������ � ����������
������� ����� ��������� _������� ��������_.

��� �������� ����������, ��� ���������� �������� ��
[������](oRRRlova/STOLP.R)

#### ������ ���������

������������ �������� �� ������� ������ ������. � �������� ���������
������������� ���������� **���������� KNN** � ��� ������� �������:
![](http://latex.codecogs.com/svg.latex?W%28i%2C%20u%29%20%3D%20%5Bi%20%5Cleq%20k%5D%20%5Ccdot%20%5Cfrac%7Bk%20&plus;%201%20-%20i%7D%7Bk%7D).

����� ��������� ������ �������� �� ���������� ������ �������������, �������
������� _��������_ ��������� ����. ����� �������� �������� ������, ������
�������� `< -1`.

��� �� ����� �� ���������� �����, �������� **kwKNN** ���� ������ ���������
(6 ������) ��� ����������� ����� `k` (�������, `k = 30`).

������������� �������� �������� ��������� ������� (������� ����� - ������):
![STOLP Margin](IMG/STOLP_M.png)
������� �������� �������� ����������
_���������_ ��������, � ��� ��������, ��� � ������� ����� ����� "��������" �������������
��������, �, �������������, �� ����� ������� �� �������� �������������.

���������� �������� �������� � ������� **STOLP**, ��������
���������� ������ � 2 ����.

� ������������ ������� �������� ��������� �� 10 ��������,
�������� ����� _2 ������_ �� ��������� �������:

![STOLP...](IMG/STOLP.png)

��� ����� ��������, ������� ����������� �� 150-�� �� 8-�� �������� (� 18 ���),
��� ����� ������� ����� �������������.

__�����:__
- � ���� ��������� ������ �������
- �������� �������� �������� �������������

__������:__
- ������� � ����������

## ����������� ��������������

����������� ������ � ������������� ������� �� �������, ������������,
��� ���� ��������� ������������� ������� �� ������� ��������,
�� ������� �������� ����� �������� � ����� ����.
����� ����, ���� �������� ���������,
�� ���� �������� ����������� ������������ ������.

��� ����������������� ������� ����������� ������� ������������� �������
�� �������, �� ��� ����������� ������������� ����������� �������.
������ ��������� � ���� ������, ��� �������� __������������� �����������
�����������__.

![](https://latex.codecogs.com/svg.latex?a%28x%29%20%3D%20%5Carg%20%5Cmax_%7By%20%5Cin%20Y%7D%20%5Clambda_y%20P_y%20p_y%28x%29)

�� �������� _��������� ������������� �������_, ��� �������, �� ��������.
�� ���������� ��������� (���������������) �� ��������� �������.
� ���������� ����������� �������� �������� ���� �����������,
��� ��� ������������ ��������� �� ������� ����� ������ � ��������� ������������.

__����������� �����������__ ����� �������� _t_ � _s_ � ��� ��������������
����� �����
![](https://latex.codecogs.com/svg.latex?x%20%5Cin%20X)
�����, ��� _�������� ������������� �����������_ ����������� ������������
��� _y = s_ � _y = t_.

![](https://latex.codecogs.com/svg.latex?%5Clambda_t%20P_t%20p_t%28x%29%20%3D%20%5Clambda_s%20P_s%20p_s%28x%29).

��������� ��������� �� �������� �������� ��������������� _��������� �������������
�������_ � ������� ������ ���� �� ����� ��� ������
_����������� �����������_.

### �������������� �������� (Plug-in)

__�������������� ��������__ � �������� ������� ����������������� ����������
������������� ����������� ���������� ���������, ������� ����������� �� �������:

![](http://latex.codecogs.com/svg.latex?N%28x%3B%5Cmu%2C%5CSigma%29%20%3D%20%5Cfrac%7B1%7D%7B%5Csqrt%7B%282%5Cpi%29%5En%20%7C%5CSigma%7C%7D%7D%20%5Ccdot%20exp%5Cleft%28-%5Cfrac%7B1%7D%7B2%7D%28x%20-%20%5Cmu%29%5ET%20%5CSigma%5E%7B-1%7D%20%28x%20-%20%5Cmu%29%5Cright%29),
� �������

![](http://latex.codecogs.com/svg.latex?x%20%5Cin%20%5Cmathbb%7BR%7D%5En)
� ������, ��������� �� *n* ���������,

![](http://latex.codecogs.com/svg.latex?%5Cmu%20%5Cin%20%5Cmathbb%7BR%7D%5En)
� �������������� ��������,

![](http://latex.codecogs.com/svg.latex?%5CSigma%20%5Cin%20%5Cmathbb%7BR%7D%5E%7Bn%20%5Ctimes%20n%7D)
� �������������� �������.

����� ������ _��������� ������������� �������_, �������� ���������������
����������� ���������
![](http://latex.codecogs.com/svg.latex?%5Cmu%2C%20%5CSigma)
�� ��������� �������� ��� ������� ������ _y_ :

![](http://latex.codecogs.com/svg.latex?%5Chat%7B%5Cmu%7D%20%3D%20%5Cfrac%7B1%7D%7Bl_y%7D%20%24%24%5Csum_%7Bi%20%3D%201%7D%5E%7Bl_y%7D%20x_i%24%24)

```
estimateMu = function(points) {
    rows = dim(points)[1]
    cols = dim(points)[2]
    mu = matrix(NA, 1, cols)
    for (col in 1:cols) {
        mu[1, col] = mean(points[, col])
    }
    return(mu)
}
```

![](http://latex.codecogs.com/svg.latex?%5Chat%7B%5CSigma%7D%20%3D%20%5Cfrac%7B1%7D%7Bl_y%20-%201%7D%20%24%24%5Csum_%7Bi%20%3D%201%7D%5E%7Bl_y%7D%20%28x_i%20-%20%5Chat%7B%5Cmu%7D%29%28x_i%20-%20%5Chat%7B%5Cmu%7D%29%5ET).

```
estimateCovarianceMatrix = function(points, mu) {
    rows = dim(points)[1]
    cols = dim(points)[2]
    covar = matrix(0, cols, cols)
    for (i in 1:rows) {
        covar = covar + (t(points[i,] - mu) %*% (points[i,] - mu)) / (rows - 1)
    }
    return(covar)
}
```

#### ������ ���������

������������ �������� �� ���� ��������������� �� ������ ����������� �������������
��������, ���������, ��������� ��������������� �������� ���������� �� ��������,
� ����� ����������� ����������� ����� �������� �� ��������.

��������� �������� �� ������:
[shinyapps.io](https://orlova-tatiana.shinyapps.io/plug-in/).

�������� �������� ����� ��������������� _�������������� �������_ �
_���. ��������_ �������� �������.

![](IMG/plug-in-1.png)

������ ��� ����� ���������� ���������� ��������, �������� ������:

![](IMG/plug-in-2.png)