# week002:python虚拟环境sc310的安装与jupyter notebook配置

根据`jupyter`的官网介绍https://jupyter.org/install，早期经典的`jupyter notebook`现已逐渐被`jupyterlab`替代，最新版的安装命令很简单，直接执行`pip install jupyterlab`或`conda install -c conda-forge jupyterlab`即可，启动命令（注意中间有空格）：`jupyter lab`

如果习惯于使用经典的`jupyter notebook`，则参考如下教程：

1. 创建sc310虚拟环境

   ```shell
   conda create -n sc310 python=3.10
   ```

   ![image-20250723113950472](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202507231525994.png)

2. 进入虚拟环境sc310中，并安装notebook

   ```shell
    conda install notebook
   ```

3. 查看python内核

   ```shell
    jupyter kernelspec list
   ```

4. 将sc310虚拟环境与python内核进行绑定，并将内核命名为同名的sc310

   ```shell
    python -m ipykernel install --user --name sc310 --display-name sc310
   ```

5. 删除内核

   ```shell
   jupyter kernelspec remove kernel_name
   #  jupyter kernelspec list 重新验证是否已删除
   ```

   

6. 再次查看python内核，验证是否绑定成功

   ![image-20250723113520060](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202507231135178.png)

7. 启动jupyter notebook

   ![image-20250723113715788](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202507231137867.png)

![image-20250723114308756](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202507231143859.png)
7. 安装中文支持

   ```shell
   pip install jupyterlab-language-pack-zh-CN
   ```

   ![image-20250724160203495](https://gitee.com/jikeyang/typera_picgo/raw/master/sias/202507241602931.png)

