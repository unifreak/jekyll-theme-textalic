---
category: doc
tags: [homework]
---

## 과제 1번: 조기 종료를 사용한 배치 경사 하강법으로 로지스틱 회귀를 구현하라. 단, 사이킷런을 전혀 사용하지 않아야 한다.

### 시작하기에 앞서 아래와같이 기본적인 세팅을 합시다
```
\# 파이썬 ≥3.5 필수
import sys

assert sys.version_info >= (3, 5)



\# 사이킷런 ≥0.20 필수

import sklearn

assert sklearn.__version__ >= "0.20"



\# 공통 모듈 임포트

import numpy as np

import os



\# 노트북 실행 결과를 동일하게 유지하기 위해

np.random.seed(42)



\# 깔끔한 그래프 출력을 위해

%matplotlib inline

import matplotlib as mpl

import matplotlib.pyplot as plt

mpl.rc('axes', labelsize=14)

mpl.rc('xtick', labelsize=12)

mpl.rc('ytick', labelsize=12)



\# 그림을 저장할 위치

PROJECT_ROOT_DIR = "."

CHAPTER_ID = "training_linear_models"

IMAGES_PATH = os.path.join(PROJECT_ROOT_DIR, "images", CHAPTER_ID)

os.makedirs(IMAGES_PATH, exist_ok=True)



def save_fig(fig_id, tight_layout=True, fig_extension="png", resolution=300):

  path = os.path.join(IMAGES_PATH, fig_id + "." + fig_extension)

  print("그림 저장:", fig_id)

  if tight_layout:

​    plt.tight_layout()

  plt.savefig(path, format=fig_extension, dpi=resolution)

  

\# 어레이 데이터를 csv 파일로 저장하기

def save_data(fileName, arrayName, header=''):

  np.savetxt(fileName, arrayName, delimiter=',', header=header, comments='')
  
from sklearn import datasets
iris = datasets.load_iris()
```

그다음 이번 회귀에서 사용할 붓꽃 데이터를 로드합니다
``` 
from sklearn import datasets
iris = datasets.load_iris() 
```

데이터를 로드했다면 품종 여부를 판정하는데 사용되는 데이터셋을 지정해줍니다
```
X = iris["data"][:, 3:]                   # 1개의 특성(꽃잎 너비)만 사용
y = (iris["target"] == 2).astype(np.int)  # 버지니카(Virginica) 품종일 때 1(양성)
```

모든 샘플에 편향을 추가해줍니다
```
X_with_bias = np.c_[np.ones([len(X), 1]), X]
```

결과를 일정하게 유지하기 위해 랜덤 시드를 지정해줍시다
```
np.random.seed(2042)
```

데이터셋 분할

데이터셋을 훈련, 검증, 테스트 용도로 6대 2대 2의 비율로 무작위로 분할합니다
```
test_ratio = 0.2                                         # 테스트 세트 비율 = 20%
validation_ratio = 0.2                                   # 검증 세트 비율 = 20%
total_size = len(X_with_bias)                            # 전체 데이터셋 크기

test_size = int(total_size * test_ratio)                 # 테스트 세트 크기: 전체의 20%
validation_size = int(total_size * validation_ratio)     # 검증 세트 크기: 전체의 20%
train_size = total_size - test_size - validation_size    # 훈련 세트 크기: 전체의 60%
```

```
rnd_indices = np.random.permutation(total_size)
```

```
X_train = X_with_bias[rnd_indices[:train_size]]
y_train = y[rnd_indices[:train_size]]

X_valid = X_with_bias[rnd_indices[train_size:-test_size]]
y_valid = y[rnd_indices[train_size:-test_size]]

X_test = X_with_bias[rnd_indices[-test_size:]]
y_test = y[rnd_indices[-test_size:]]
```

그후 타겟변환을 하여 전처리과정을 해줍니다
```
def to_one_hot(y):
    n_classes = y.max() + 1                 # 클래스 수
    m = len(y)                              # 샘플 수
    Y_one_hot = np.zeros((m, n_classes))    # (샘플 수, 클래스 수) 0-벡터 생성
    Y_one_hot[np.arange(m), y] = 1          # 샘플 별로 해당 클래스의 값만 1로 변경. (넘파이 인덱싱 활용)
    return Y_one_hot
    
Y_train_one_hot = to_one_hot(y_train)
Y_valid_one_hot = to_one_hot(y_valid)
Y_test_one_hot = to_one_hot(y_test)
```

전처리과정까지 끝냈다면 이제 로지스틱 회귀의 기본이 되는 시그모이드 함수를 적용합니다
```
def sigmoid(logits):
  return 1 / (1 + np.exp(-logits))
```
```
n_inputs = X_train.shape[1]           # 특성 수(n) + 1, 붓꽃의 경우: 특성 2개 + 1
n_outputs = 2
#n_outputs = len(np.unique(y_train))   # 중복을 제거한 클래스 수(K), 붓꽃의 경우: 3개
```
```
Theta = np.random.randn(n_inputs, n_outputs)
```
