# Weath_classification

-----

# 一. 训练

可通过以下命令训练默认的第一个模型,其他模型的训练请参照1-5小节
```shell
source activate tensorflow_p36
cd Weath_classification_submit/src/
python train_submit.py
```
----
#### 在 train_submit.py 可修改一下内容 

1 因为我们是多模型的融合而成的结果,按照编号取消第一部分注释加载要训练的模型
```python
from classification_models.keras import Classifiers
## 1
# import efficientnet.keras as efn
# preprocess_input = efn.preprocess_input
## 2
# seresnext50, preprocess_input = Classifiers.get('seresnext50')
## 3
# xception, preprocess_input = Classifiers.get('xception')
## 4
# densenet201, preprocess_input = Classifiers.get('densenet201')
## 5
# inceptionresnetv2, preprocess_input = Classifiers.get('inceptionresnetv2')
```
2 取消对应的模型在函数model_fn()注释
```python
base_model = efn.EfficientNetB4(include_top=False,
#     base_model = seresnext50(include_top=False,
#     base_model = xception(include_top=False,
#     base_model = densenet201(include_top=False,
#     base_model = inceptionresnetv2(include_top=False,
```
3 按照编号取消模型保存目录的
```python
filepath = './model_snapshots/X0001/weights-improvement-{epoch:02d}-{val_acc:.4f}.h5'
# filepath = './model_snapshots/X0002/weights-improvement-{epoch:02d}-{val_acc:.4f}.h5'
# filepath = './model_snapshots/X0003/weights-improvement-{epoch:02d}-{val_acc:.4f}.h5'
# filepath = './model_snapshots/X0004/weights-improvement-{epoch:02d}-{val_acc:.4f}.h5'
# filepath = './model_snapshots/X0005/weights-improvement-{epoch:02d}-{val_acc:.4f}.h5'
```
4 如果需要对应的 tensorBoard 可取消下面的注释,并改写其对应的编号
```python
# log_local = './logs/X0001-effb4(380)-cutmix&mixup(1.0)-wp'
# tensorBoard = TensorBoard(log_dir=log_local)
```
----
#### 注意事项

*  时间紧迫的话建议 epochs = 50

*  batch_size 越大越好,如果不能满足,请尽可能大并且是 2 的倍数,默认 batch_size = 32


# 二. 预测

#### (1) 在默认(已提供的Test集下)测试 

可通过以下命令预测生成默认的第一个模型的预测文件,其他模型的预测请参照1-5小节
```shell
source activate tensorflow_p36
cd Weath_classification_submit/src/
python test_submit.py
```
----
#### 在 test_submit.py 可修改一下内容 

1 因为我们是多模型的融合而成的结果,按照编号取消第一部分注释加载要训练的模型
```python
from classification_models.keras import Classifiers
# 1
import efficientnet.keras as efn
preprocess_input = efn.preprocess_input
## 2
# seresnext50, preprocess_input = Classifiers.get('seresnext50')
## 3
# xception, preprocess_input = Classifiers.get('xception')
## 4
# densenet201, preprocess_input = Classifiers.get('densenet201')
## 5
# inceptionresnetv2, preprocess_input = Classifiers.get('inceptionresnetv2')
```
2 取消对应的模型在函数model_fn()注释
```python
base_model = efn.EfficientNetB4(include_top=False,
#     base_model = seresnext50(include_top=False,
#     base_model = xception(include_top=False,
#     base_model = densenet201(include_top=False,
#     base_model = inceptionresnetv2(include_top=False,
```
3 按照编号取消注释进行模型加载(如有需要,可更改加载的模型编号)
```python
model1.load_weights('./model_snapshots/X0001/weights-improvement-44-0.8930.h5')
# model1.load_weights('./model_snapshots/X0002/weights-improvement-49-0.8977.h5')
# model1.load_weights('./model_snapshots/X0003/weights-improvement-49-0.8874.h5')
# model1.load_weights('./model_snapshots/X0004/weights-improvement-48-0.9002.h5')
# model1.load_weights('./model_snapshots/X0005/weights-improvement-42-0.8846.h5')
```
4 修改保存csv文件的路径
```python
path = './submit/submit_1.csv'
# path = './submit/submit_2.csv'
# path = './submit/submit_3.csv'
# path = './submit/submit_4.csv'
# path = './submit/submit_5.csv'
```
5 当完成五个模型的训练后,修改次目录下Merge_model.py中五个预测模型的路径,进行融合,在shell窗口执行
```shell
python Merge_model.py
```
在result文件中即可找到提交的csv

-----
#### (2) 在新的Test集下测试
* 请将新的 Test 集和 test.csv 移动到 Weath_classification_submit
* 修改 Weath_classification_submit/src 目录下 test_data.py 中以下两句代码
```python
test_data_url = '../test_A.csv' # 指向新的 test.csv    
local_path = '../Test/'         # 指向新的 test 图片文件目录
```
* 重复(1)中内容即可
