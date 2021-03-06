# Tensorflow Study

> Tensor 조작 및 모델 설계를 자유롭게 하기 위해 Tensorflow의 함수들을 이용하여 딥러닝을 공부하는 연습입니다.

```python
import tensorflow as tf
```

## Basic Functions

#### Tensor constant

```python
t1= tf.constant([[[1,1,1],[2,2,2]],
                 [[3,3,3],[4,4,4]]],dtype= 'float64')
t2= tf.constant([[[4,4,4],[3,3,3]],
                  [[2,2,2],[1,1,1]]],dtype= 'float64')
```

dtype 설정 가능

#### Tensor Variable

```python
w= tf.Variable([[1,1],[2,2]],dtype= 'float64')
```

#### Tensor dtype change

```python
tf.dtypes.cast(t1, 'int32')
tf.dtypes.cast(t2, 'int32')
```

tf.dtypes class의 module들 활용 가능. 


#### Generate Tensor

```python
tf.zeros(shape=(3,2,2)) # zero tensor
tf.ones(shape=(3,2,2)) # one tensor
tf.one_hot(indices=[0,1,2], depth=3) # index는 1의 위치, depth는 전체 개수
```

#### Tensor shape

```python
t1.shape # 행, 렬, elements
tf.size(t1) # 총 size
```

#### 행렬곱

```python
tf.matmul(w,t2)
```

#### 축별 합

```python
tf.reduce_sum(t1, axis=0)
tf.reduce_sum(t1, axis=1)
tf.reduce_sum(t1, axis=2)
```

#### 축별 평균

```python
tf.reduce_mean(t1, axis=0)
```

#### sort

```python
tf.sort(t1, axis=1, direction='ASCENDING')
```

#### concatenate

```python
tf.concat(t1,t2)
```

#### where

```python
tf.where(t1==2)
```

#### slicing tensor by shape

```python
tf.slice(t1,begin=[1,0,0],size=[1,2,2])
```

#### clip_by_value

```python
tf.clip_by_value(t1,1.5,3.5)
```

> 해당 범위에서 벗어나는 값들을 모두 minimum값과 maximum값으로 치환함.

## tf.math
#### Tensor element-wise operations

```python
tf.add(t1,t2)
tf.subtract(t1,t2)
tf.multiply(t1,2)
tf.square(t1)
tf.sqrt(t1)
```

#### 몫, 나머지

```python
tf.truediv(t1,2) # 몫, dtype맞아야함
tf.mod()
```
#### 음수, 절대값

```python
tf.negative(t1)
tf.abs(t1)
```

#### top_k

```python
tf.math.top_k(t1)
```

#### 반올림

```python
tf.round(t1)
```

#### 최대, 최소, argmax, argmin

```python
tf.maximum(t1,t2)
tf.minimum(t1,t2)
tf.argmax(t1) # maximum값의 index 출력
tf.argmin(t2) # minimum값의 index 출력
```

#### log, exp, sin,cos

```python
tf.math.log(t1)
tf.exp(t1); tf.math.exp(t1)
tf.sin(t1); tf.math.sin(t1)
tf.cos(t1); tf.math.cos(t1)
```

#### other reduce_s

```python
tf.math.reduce_any(t1 < 3)
tf.math.reduce_all(t1 < 5)
tf.math.reduce_euclidean_norm(t1,t2)
tf.math.reduce_max(t1)
tf.math.reduce_min(t1)
tf.math.reduce_prod(t1)
tf.math.reduce_std(t1)
tf.math.reduce_variance(t1)
```

#### activation functions

```python
tf.math.sigmoid()
tf.math.softmax()
tf.math.tanh()
```

## tf.random Functions

```python
tf.random.normal(shape, name)
tf.random.uniform(shape)
tf.random.set_seed(seed)
tf.random.categorical(tf.math.log([[0.5, 0.5]]), 5) # 반반확률의 categorical 변수 생성
```

## tf.nn Functions

#### Convolutions

```python
tf.nn.conv1d()
tf.nn.conv1d_transpose()
tf.nn.conv2d()
tf.nn.conv2d_transpose()
```

#### Poolings

```python
tf.nn.avg_pool1d()
tf.nn.avg_pool2d()
tf.nn.avg_pool3d()
tf.nn.max_pool1d()
tf.nn.max_pool2d()
tf.nn.max_pool3d()
```

#### activation functions

```python
tf.nn.relu()
tf.nn.softmax()
tf.nn.sigmoid()
tf.nn.leaky_relu()
```

#### dropout / BN

```python
tf.nn.dropout()
tf.nn.batch_normalization()
tf.nn.batch_norm_with_global_normalization()
```



## Important Axis functions

#### expand_dims

> 크기가 1인 축을 하나 늘리는 역할을 한다.

```python
tf.expand_dims(Tensor, axis= 2) # axis가 2인 곳에 shape 1을 추가하여 차원을 늘리기
```

#### repeat_elements

> 열을 복제한다.

```python
import tensorflow.keras.backend as K

K.repeat_elements(Tensor, rep= 3, axis= 2) 
# 축이 2에 해당하는 차원의 원소를 3번 복사하여 늘어놓는다.
# 예를 들어 만약 Tensor의 크기가 (1,4,1,4)라면 (1,4,3,4)가 된다
```

#### Dot

> Tensor의 내적

```python
from tensorflow.keras.layers import Dot

Dot(axes= [2,1])([y,x])
# y Tensor, x Tensor의 각각 2축, 1축에 해당하는 부분을 서로 Dot product한다.
# 따라서 두 축에 해당하는 shape의 크기가 같아야 한다.
# 그리고 Dot 함수는 자동으로 Transpose하여 계산한다.
# 다만 수학적으로 행렬 이상의 개념에 Dot product라는 개념이 없기 때문에
# 곱하여 더하는 개념이라고 생각하면 된다.
# 예를 들어 y Tensor의 차원이 (1,4,3,4)이고
# x Tensor의 차원이 (1,4,3)인 경우 axes를 (3,1)로 지정하여 곱하면
# (1,4,3,3) 차원의 Tensor가 출력된다.
```

