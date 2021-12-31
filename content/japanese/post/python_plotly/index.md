---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "PythonのPlotlyで遊んだ"
subtitle: ""
summary: ""
authors: []
tags: [Python]
categories: [PC]
date: 2021-09-17T22:28:21+09:00
lastmod: 2021-09-20T21:53:21+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
### 0. はじめに
Plotly[^1] をご存知だろうか．統計や金融，科学等の幅広い分野で用いられている，データの可視化のためのライブラリであり，Python や R などで用いることができる．

[^1]: [plotly Graphing Libraries](https://plotly.com/graphing-libraries/), accessed on 2021.9.17.

Plotly を用いると，3d plot をとてもきれいにできる．今回は，Plotly を用いて適当なグラフを描画して遊ぼうと思う．

### 1. <a name="section1">準備</a>
#### 1.1. インストール
Python にインストールするのであれば，`pip install` すればよい．

```bash
pip install plotly
```

#### 1.2. プロットするもの
最適化のためのテスト関数[^2] の一部をプロットする．

[^2]: [Test function for optimization (en: Wikipedia)](https://en.wikipedia.org/wiki/Test_functions_for_optimization), accessed on 2021.9.17.

ここでは，次の2つの関数をプロットする．

##### Rastrigin function
- Definition
$$f(\boldsymbol{x}) = An + \sum_{i = 1}^n (x_i^2 - A \cos (2 \pi x_i)), \ \ \text{where} \ \ A = 10.$$

- Search domain
$$ -5.12 \leq x_i \leq 5.12, \ \ i = 1, \ldots, n. $$

- Global minimum
$$ f(0, \ldots, 0) = 0. $$

##### Ackley function
- Definition
$$ f(x, y) = -20 \exp \left(-0.2 \sqrt{0.5(x^2 + y^2)} \right)$$$$\hspace{45mm} - \exp \left(0.5 (\cos 2 \pi x + \cos 2 \pi y)\right) + e + 20.$$

- Search domain
$$ -5 \leq x, y \leq 5. $$

- Global minimum
$$ f(0, 0) = 0. $$


### 2. プロット
ここでは，3D Surface Plot を行う．コードの書き方は，[^3]を参考にした．

[^3]: [plotly Graphing Libraries, 3D Surface Plots in Python](https://plotly.com/python/3d-surface-plots/), accessed on 2021.9.17.

<details>
<summary>実装した Python のコード</summary>

```python
########## Packages ##########
import numpy as np
import plotly.graph_objects as go 
from math import e 
##############################

class Test_function():
    """ 
    The class of test functions
    """
    def rastrigin(self, x):
        """
        Rastrigin function
        """
        val = 10.0*x.size 
        for i in range(x.size):
            val += x[i]**2 - 10*np.cos(2*np.pi*x[i])
        return val 

    def ackley(self, x):
        """
        Ackley function
        """
        val = -20*np.exp(-0.2*(np.sqrt(0.5*(x[0]**2 + x[1]**2))))
        val += -np.exp(0.5*(np.cos(2*np.pi*x[0]) + np.cos(2*np.pi*x[1])))
        val += e + 20 
        return val 

class Plot_func():
    """
    The class to plot functions
    """
    def func_value(self, func, x_range, y_range):
        """
        get the value of the function (2 dim)
        Input:
            func: function
            x_range, y_range: numpy linspace
        Output:
            X, Y: numpy meshgrid
            Z: corresponding func value
        """
        X, Y = np.meshgrid(x_range, y_range)
        Z = np.empty_like(X)
        for i in range(len(X)):
            for j in range(len(X[i])):
                vec_x = np.array([X[i][j], Y[i][j]])
                Z[i][j] = func(vec_x)
        return X, Y, Z
    
    def plotly_surface_contour(self, func, x_range, y_range):
        """
        plot the surface of the function using plotly,
        https://plotly.com/python/3d-surface-plots/
        """
        ##### get the value of the function #####
        X, Y, Z = self.func_value(func, x_range, y_range)
        #########################################

        ##### plot #####
        fig = go.Figure(data=[go.Surface(z=Z, x=X, y=Y)])
        fig.update_traces(contours_z=dict(show=True, usecolormap=True,
                            highlightcolor="limegreen", project_z=True))
        fig.update_layout(autosize=False,
                            scene_camera_eye=dict(x=1.87, y=0.88, z=-0.64),
                            width=500, height=500,
                            margin=dict(l=65, r=50, b=65, t=90))
        fig.show()
        ################

##### instance of the class #####
test = Test_function()
pf = Plot_func()
#################################

##### Plot Rastrigin function #####
x = np.linspace(-5.12, 5.12, 100)
y = np.linspace(-5.12, 5.12, 100)
pf.plotly_surface_contour(
    func=test.rastrigin, 
    x_range=x,
    y_range=y)
###################################

##### Plot Ackley function #####
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
pf.plotly_surface_contour(
    func=test.ackley, 
    x_range=x, 
    y_range=y)
################################
```
</details>

また，このコードを Jupyter notebook 形式にして html に変換したものを以下のリンクに置いている (表示に少し時間がかかる)．

- <a href="https://tachibana-ai.netlify.app/post/python_plotly/test_function_blog.html">変換した Jupyter Notebook</a>


関数の表面がどうなっているか可視化されていて面白いと思う．また，動かすこともできるのが良い．

### 3. まとめ
Plotly は面白いので，詳しくなりたい．