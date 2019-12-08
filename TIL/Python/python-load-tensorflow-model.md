# 학습된 H5 모델 파일을 Python에서 사용하기

학습된 모델을 Python에서 불러오는데 오류가 발생했다.

```python
import flask
import tensorflow as tf
from keras.models import load_model

model = load_model('models/lstm_test.h5')
```

모듈을 저장하는 방법에 문제가 있어서 load할 때, kears의 models가 아닌 tensorflow의 keras의 models를 사용해야 한다.

해결방법

```python
import flask
import tensorflow as tf

model = tf.keras.models.load_model('models/lstm_test.h5')
```

오류 없이 정상적으로 작동한다.

---
#### 참고

https://copycoding.tistory.com/101

