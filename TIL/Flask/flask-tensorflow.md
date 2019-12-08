# Flask에서 TensorFlow 사용

Flask에서 tensorflow의 keras를 사용하는데 학습된 모델이 계속 오류가 나는 문제가 발생하였다.

```
‘Tensor (“something”) is not an element of this graph.’ Error in Keras using Tensorflow backend on Flask Web Server.
```

이 오류는 Flask가 처리하는 모든 웹 요청은 모델에 로드된 기본 세션이 아닌 자체 Tensorflow의 세션을 생성하는 새 스레드를 만들기 때문이다.

Flask는 여러 스레드를 사용합니다. tensorflow 모델이 로드되지 않고 동일한 스레드에서 사용되기 때문이다.
한 가지 해결 방법은 tensorflow가 gloabl 기본 그래프를 사용하도록 강제하는 것입니다.

이 문제를 해결하기 위해서 모델과 함께 로드된 기본 세션을 사용하도록 해야 한다.

```python
graph = tf.get_default_graph()
```

그리고 학습한 모델이 예측을 할 때, 그래프를 사용해야 한다.

```python
with graph.as_default():
```

하지만 또 오류가 났다.

```
Error while reading resource variable conv2d_9/kernel from Container: localhost. This could mean that
the variable was uninitialized. Not found: Resource localhost/conv2d_9/kernel/class tensorflow::Var does not exist.
[[{{node conv2d_9/Conv2D/ReadVariableOp}}]]
```

```python
from tensorflow.python.keras.backend import set_session
from tensorflow.python.keras.models import load_model

tf_config = some_custom_config
sess = tf.Session(config=tf_config)
graph = tf.get_default_graph()

# IMPORTANT: models have to be loaded AFTER SETTING THE SESSION for keras! 
# Otherwise, their weights will be unavailable in the threads after the session there has been set
set_session(sess)
model = load_model(...)
```

이렇게 학습된 모델로 예측할 때 아래와 같이 사용한다.

```python
with graph.as_default():
    set_session(sess)
    model.predict(...)
```

이후 정상적으로 작동한다.

---
#### 참고

https://stackoverflow.com/questions/51127344/tensor-is-not-an-element-of-this-graph-deploying-keras-model

https://kobkrit.com/tensor-something-is-not-an-element-of-this-graph-error-in-keras-on-flask-web-server-4173a8fe15e1

https://github.com/tensorflow/tensorflow/issues/28287