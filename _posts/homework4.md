---
category: doc
tags: [homework]
---

## 과제 1번: 조기 종료를 사용한 배치 경사 하강법으로 로지스틱 회귀를 구현하라. 단, 사이킷런을 전혀 사용하지 않아야 한다.

```\# 파이썬 ≥3.5 필수
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
```

