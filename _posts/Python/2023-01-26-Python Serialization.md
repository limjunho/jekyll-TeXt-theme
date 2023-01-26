---
title: Python Serialization(pickle, dill)
tags: Python
---

## Summry

본 문서에서는 Python 직렬화에 대해 설명하고 관련 모듈을 소개한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 직렬화(Serialization)와 역직렬화(Deserialization)

Serial 은 '연속된' 이라는 뜻을 가지고 있다. 즉, Serialization은 객체를 '연속된' Byte 나 String 으로 변환하는 과정을 의미한다. Object는 메모리에 올라와 있지만 Byte 나 String 은 파일로 저장될 수 있거나 통신에 사용 될 수 있다.

즉, **직렬화는 객체를 파일의 형태 등으로 저장하거나, 통신하기 쉬운 포맷으로 변환하는 과정**
을 의미한다. 특정 포맷으로 직렬화된 데이터는 역직렬화라는 과정을 통해 다시 객체로 변환될 수 있다.

직렬화는 많은 상황에서 사용될 수 있다.  
ex) 훈련 단계 후 신경망의 상태를 저장하여 나중에 훈련을 다시 할 필요 없이 사용할 수 있도록 함

## pickle

파이썬 객체 구조의 직렬화와 역 직렬화를 위한 모듈로 **객체 자체를 바이너리로 저장하고 필요할 때 불러오는것이 가능**하다.

데이터 분석 과정에서 만들어지는 데이터나 모델들을 텍스트 형식이 아닌 binary 형식으로 파일에 저장하고, 이후 이 파일을 읽어 작업을 이어서 할 수 있다.

아래 목록들을 pickle할 수 있다.

- `None`, `True`, and `False`;
- integers, floating-point numbers, complex numbers;
- strings, bytes, bytearrays;
- tuples, lists, sets, and dictionaries containing only picklable objects;
- functions (built-in and user-defined) accessible from the top level of a module (using `def`, not `lambda`);
- classes accessible from the top level of a module;
- instances of such classes whose the result of calling `__getstate__()` is picklable (see section Pickling Class Instances for details).

</br>

- pickle 은 잘못되거나 악의적으로 구성된 데이터로부터 안전하지 않다. unpickle한 데이터가 신뢰할 수 있는 출처에서 온 것인지 여부를 결정하는 것은 사용자에게 맡겨져 있다.

### how to use

```python
import pickle
my_list = ['a','b','c']

## Save pickle
with open("data.pickle","wb") as fw:
    pickle.dump(my_list, fw)

## Load pickle
with open("data.pickle","rb") as fr:
    data = pickle.load(fr)
```

---

## dill

dill 은 파이썬 객체를 직렬화 및 역직렬화하기 위한 python Pickle 라이브러리의 확장으로 pickle과 달리 serializable 하지 않은 값들도 손쉽게 binary 로 저장이 가능하다.

- **dill 은 단일 명령으로 인터프리터 세션의 상태를 저장하는 기능을 제공한다. 따라서 인터프리터 세션을 저장하고, 인터프리터를 닫고, 피클된 파일을 다른 컴퓨터로 전송하고, 새 인터프리터를 열고, 세션을 피클 해제하여 원래 인터프리터 세션의 '저장된' 상태에서 계속할 수 있다.**

</br>

아래 목록들을 pickle할 수 있다.

- none, type, bool, int, float, complex, bytes, str,
- tuple, list, dict, file, buffer, builtin,
- python classes, namedtuples, dataclasses, metaclasses,
- instances of classes,set, frozenset, array, functions, exceptions

dill은 pickle과 달리 더 많은 표준 유형을 pickle할 수 있다.

- functions with yields, nested functions, lambdas,
- cell, method, unboundmethod, module, code, methodwrapper,
- methoddescriptor, getsetdescriptor, memberdescriptor, wrapperdescriptor,
- dictproxy, slice, notimplemented, ellipsis, quit

dill은 아직 아래 유형들은 pickle하지 못한다.

- frame, generator, traceback

dill은 다음과 같은 기능을 제공한다.

- save and load python interpreter sessions
- save and extract the source code from functions and classes
- interactively diagnose pickling errors

</br>

- dill은 잘못되거나 악의적으로 구성된 데이터로부터 안전하지 않다. unpickle한 데이터가 신뢰할 수 있는 출처에서 온 것인지 여부를 결정하는 것은 사용자에게 맡겨져 있다.

### how to use

- dill은 파이썬 인터프리터의 표준 라이브러리에 포함되지 않아 설치가 필요하다.

```bash
pip3 install dill
```

```python
# 저장
import dill
dill.dump_session('notebook_env.db')

# 복원
import dill
dill.load_session('notebook_env.db')
```

---

## pickle vs dill

**dill은 pickle 모듈과 동일한 인터페이스를 사용자에게 제공하며 몇 가지 추가 기능도 포함한다** . 피클링 Python 개체 외에도 dill은 단일 명령으로 인터프리터 세션의 상태를 저장하는 기능을 제공한다.

pickle은 모든 객체를 피클링할 수 있는 건 아니다. 데이터베이스 연결, 열린 네트워크 소켓, 실행 중인 스레드 등은 불가능하다. 피클링할 수 없는 물체에 직면 했다면 dill 사용을 고려할 수 있다. dill 모듈은 pickle의 기능을 확장한다. 공식 문서에 따르면 yields가 포함된 함수, 중첩 함수, 람다 등 일반적이지 않은 타입을 직렬화할 수 있다.

- 다만 dill은 파이썬 인터프리터의 표준 라이브러리에 포함되지 않으며, 일반적으로 pickle 보다 느리다.

### 속도

dill은 pickle보다 더 견고하지만 pickle보다 느리다. 하지만 pickle또한 속도가 느리고 더 빠른 직렬화 방법을 찾는다면 C로 작성된 cPickle이나 JSON으로 직렬화 하는 방법을 고려해야 할 것이다. 참고로 cPickle 또한 JSON이나 Thrift의 1/4정도라고 한다.

### 보안

직렬화 프로세스는 객체의 상태를 디스크에 저장하거나, 네트워크를 통해 전송해야 할 때 매우 편리하다. 그러나 파이썬 pickle 모듈은 안전하지 않다. unpickling 프로세스 중에 임의의 코드를 실행하는 데 사용할 수도 있다.

이러한 위험을 줄이기 위한 다음과 같은 방법이 존재한다.

1. 신뢰할 수 없는 소스에서 가져오거나 안전하지 않은 네트워크를 통해 전송되는 데이터를 해제하지 않는다.

2. 중간자 공격(man in the middle attack)을 방지하기 위해 hmac과 같은 라이브러리를 사용하여 데이터에 서명하고 변조되지 않았는지 확인한다.

## Reference

[직렬화(Serialization)와 역직렬화(Deserialization)](https://hudi.blog/serialization/)

[Saving/Restoring a Jupyter Notebook Session](https://medium.com/ds4n6-tips-and-tricks/saving-restoring-a-jupyter-notebook-session-e115e122947e)

[pickle - Python object serialization](https://docs.python.org/3/library/pickle.html)

[dill - pypi](https://pypi.org/project/dill/)

[Python dill 로 class definition 까지 binary 로 저장하기](https://lovit.github.io/analytics/2019/01/15/python_dill/)

[Is Dill Faster Than Pickle? - Sweetish Hill](https://sweetishhill.com/is-dill-faster-than-pickle/)
