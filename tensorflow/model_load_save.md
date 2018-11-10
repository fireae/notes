# Tensorflow模型的加载与保存

## 1. 加载pb模型文件

```py
# 方式1
import tensorflow as tf
model_name = "model.pb"
graph = tf.get_default_graph()
graph_def = graph.as_graph_def()
graph_def.ParseFromString(tf.gfile.FastGFile(model_name, "rb").read())
tf.import_graph_def(graph_def, name="graph")
summary_writer = tf.summary.FileWriter("log/", graph)
```

```py
# 方式2
with tf.Graph().as_default():
    graph_def = tf.GraphDef()
    with open("model.pb", "wb") as f:
        graph_def.ParseFromString(f.read())
        tf.import_graph_def(graph_def, name="graph")
```

在终端执行：

```bash
tensorboard --logdir log
```

## 2. 保存pb模型文件

```py
# 方式1
from tensorflow.python.framework import graph_util
val_list = tf.global_variables()
with tf.Session() as sess:
    constant_graph = graph_util.convert_variables_to_constants(sess, 
        sess.graph_def, output_node_names=[var_list[i].name for i in range(len(var_list))])
    tf.train.write_graph(const_graph, "./output", "export_graph.pb", as_text=False)
```

```py
# 方式2
from tensorflow.python.framework import graph_util
val_list = tf.global_variables()
with tf.Session() as sess:
    constant_graph = graph_util.convert_variables_to_constants(sess, 
        sess.graph_def, output_node_names=[var_list[i].name for i in range(len(var_list))])
with tf.gfile.FastGFile("export_graph.pb", mode="wb") as f:
    f.write(constant_graph.SerializeToString())
```



