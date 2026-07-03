### 安装flash attn过程中解决No module named torch 错误
Q：ModuleNotFoundError: No module named 'torch'
      [end of output]
A : 
**对于联网+GPU环境**
首先安装一些flash attn必要的依赖
pip download ninja einops packaging psutil
然后直接安装就可以了
pip install flash_attn --no-build-isolation（这个参数很重要！）

**对于非联网的GPU+联网的CPU环境**
安装必要依赖仍然在CPU进行
但是需要上网搜集wheel包
wheel包参考地址：（我找了半天.jpg）
*记住flash attn 2 和 flash attn 3不是简单的升级关系！3和2不兼容！如果只要flash attn默认版本是2（除非以后更新）
2.x：
https://github.com/Dao-AILab/flash-attention/releases/tag/v2.8.3
https://github.com/Dao-AILab/flash-attention/issues/2299
3.x：
https://windreamer.github.io/flash-attention3-wheels/

### 新版trl中可能遇到的问题：###

Q：TypeError: RewardTrainer.__init__() got an unexpected keyword argument 'tokenizer'

A：新版trl已经移除并且弃用了tokenizer,据说是因为这个名字无法承载它的作用……猎奇，兼容都不做，反正换成processing_class=……就行了

### RL的问题，输出头维度没对齐
Q：ValueError: The model has `num_labels=2`, but reward models require `num_labels=1` to output a single scalar reward per sequence. Please instantiate your model with `num_labels=1` or pass a model name as a string to have it configured automatically.
A：哥们你做个Reward model为什么要让label变多……很显然我们在做一个连续分类任务，label只能有一个，我们算得是他的logits哦
