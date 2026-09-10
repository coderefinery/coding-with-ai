# Reference Datasets for the Workshop

This page lists ten classic, freely available datasets that can be loaded directly from the internet — no local files needed. Each entry includes the original academic source, a code snippet to load the data, a quick exploration command, and a basic visualisation.

:::{note}
The last three datasets (MNIST, Fashion-MNIST, CIFAR-10) are large image collections and may take several minutes to download. They are included here for reference but are not recommended for use during the timed hands-on exercises. Start with Titanic, Iris, Palmer Penguins, or Gapminder for the exercises.
:::

## Quick reference

| Dataset | Size | Good for |
|---|---|---|
| Titanic | 891 rows | Bar charts, survival analysis, categorical comparisons |
| Iris | 150 rows | Scatter plots, species comparison, classification basics |
| Palmer Penguins | 344 rows | Scatter plots, multi-species comparison, missing values |
| Wine Quality | 1 599 rows | Distribution plots, regression, continuous quality scores |
| Tips | 244 rows | Scatter, correlation, categorical colour coding |
| Diamonds | 53 940 rows | Large-scale exploration, overplotting, sampling strategies |
| Gapminder | Multi-year country data | Time series, bubble charts, storytelling with data |
| MNIST | 70 000 images (28×28 px) | Image classification, neural network basics |
| Fashion-MNIST | 70 000 images (28×28 px) | Image classification, drop-in MNIST replacement |
| CIFAR-10 | 60 000 images (32×32 px, colour) | Colour image classification |


---

## Titanic

Passenger survival data from the 1912 disaster of the RMS Titanic. Features include passenger class (`Pclass`), sex, age, fare paid, cabin, port of embarkation, and whether the passenger survived. The most widely used introductory dataset for classification and exploratory data analysis.

> **Source:** Various versions in circulation; this one comes from the [pandas documentation](https://github.com/pandas-dev/pandas/blob/main/doc/data/titanic.csv). Underlying records from [Encyclopedia Titanica](https://www.encyclopedia-titanica.org/).

**Load:**
```python
import pandas as pd
url = "https://raw.githubusercontent.com/pandas-dev/pandas/master/doc/data/titanic.csv"
titanic = pd.read_csv(url, index_col='Name')
titanic.head()
```

**Explore:**
```python
titanic.describe()
```

**Visualise:**
```python
import altair as alt
alt.Chart(titanic.reset_index()).mark_bar().encode(
    x=alt.X('Age:Q', bin=True),
    y='count()',
    color='Survived:N'
).properties(title='Survival by Age')
```


---

## Iris

Measurements of 150 iris flowers across three species (*Iris setosa*, *I. versicolor*, *I. virginica*). Four numeric features: sepal length, sepal width, petal length, petal width (all in cm). The most famous classification benchmark dataset in machine learning, introduced by statistician R. A. Fisher in 1936.

> **Source:** Fisher, R. A. (1936). "The use of multiple measurements in taxonomic problems." *Annals of Eugenics*, 7(2), 179–188. [DOI: 10.1111/j.1469-1809.1936.tb02137.x](https://doi.org/10.1111/j.1469-1809.1936.tb02137.x). Data originally collected by Edgar Anderson (1935).

**Load:**
```python
import pandas as pd
from sklearn.datasets import load_iris

iris_raw = load_iris()
iris = pd.DataFrame(iris_raw.data, columns=iris_raw.feature_names)
iris['species'] = [iris_raw.target_names[t] for t in iris_raw.target]
iris.head()
```

**Explore:**
```python
iris.describe()
```

**Visualise:**
```python
import altair as alt
alt.Chart(iris).mark_circle(size=60).encode(
    x='petal length (cm):Q',
    y='petal width (cm):Q',
    color='species:N',
    tooltip=iris.columns.tolist()
).properties(title='Iris: Petal Dimensions by Species')
```


---

## Palmer Penguins

Measurements of 344 penguins from three species (*Adélie*, *Chinstrap*, *Gentoo*) on three islands in the Palmer Archipelago, Antarctica. Features include bill length, bill depth, flipper length, body mass, island, and sex. Recommended as a modern, more intuitive replacement for the Iris dataset — and it has a more interesting story behind it.

> **Source:** Horst, A. M., Hill, A. P., & Gorman, K. B. (2020). *palmerpenguins: Palmer Archipelago (Antarctica) penguin data*. R package. [DOI: 10.5281/zenodo.3960218](https://doi.org/10.5281/zenodo.3960218). Original measurements: Gorman, K. B., Williams, T. D., & Fraser, W. R. (2014). *PLOS ONE*, 9(3), e90081. [DOI: 10.1371/journal.pone.0090081](https://doi.org/10.1371/journal.pone.0090081).

**Load:**
```python
import pandas as pd
url = "https://raw.githubusercontent.com/allisonhorst/palmerpenguins/main/inst/extdata/penguins.csv"
penguins = pd.read_csv(url)
penguins.head()
```

**Explore:**
```python
penguins.describe()
```

**Visualise:**
```python
import altair as alt
alt.Chart(penguins).mark_circle(size=60).encode(
    x='bill_length_mm:Q',
    y='flipper_length_mm:Q',
    color='species:N',
    tooltip=['species', 'island', 'bill_length_mm', 'body_mass_g']
).properties(title='Penguins: Bill Length vs Flipper Length')
```


---

## Wine Quality

Chemical analysis of 1 599 red Portuguese *Vinho Verde* wines, each rated for quality on a scale from 0 to 10 by wine experts. Features include fixed acidity, volatile acidity, citric acid, residual sugar, chlorides, free and total sulfur dioxide, density, pH, sulphates, and alcohol. Good for both regression (predicting the quality score) and classification (good vs. average vs. poor wine).

> **Source:** Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009). "Modeling wine preferences by data mining from physicochemical properties." *Decision Support Systems*, 47(4), 547–553. Available at the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/wine+quality).

**Load:**
```python
import pandas as pd
url = "https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-red.csv"
wine = pd.read_csv(url, sep=';')
wine.head()
```

**Explore:**
```python
wine.describe()
```

**Visualise:**
```python
import altair as alt
alt.Chart(wine).mark_bar().encode(
    x='quality:O',
    y='count()',
).properties(title='Distribution of Wine Quality Ratings')
```


---

## Tips

244 restaurant bills recorded by a waiter over several months. Features include total bill, tip amount, sex of the bill payer, smoker status, day of the week, meal time (Lunch/Dinner), and party size. A classic dataset for exploring correlations and building simple regression models.

> **Source:** Bryant, P. G. & Smith, M. A. (1995). *Practical Data Analysis: Case Studies in Business Statistics*. Richard D. Irwin Publishing. Popularised in Python by the [seaborn](https://seaborn.pydata.org/) library.

**Load:**
```python
import seaborn as sns
tips = sns.load_dataset('tips')
tips.head()
```

**Explore:**
```python
tips.describe()
```

**Visualise:**
```python
import altair as alt
alt.Chart(tips).mark_circle(size=60).encode(
    x='total_bill:Q',
    y='tip:Q',
    color='time:N',
    size='size:Q',
    tooltip=tips.columns.tolist()
).properties(title='Tip Amount vs Total Bill')
```


---

## Diamonds

Prices and physical attributes of 53 940 diamonds. Features include carat, cut (Fair/Good/Very Good/Premium/Ideal), color (D–J), clarity (I1–IF), depth percentage, table percentage, and xyz dimensions in mm. Large enough to illustrate overplotting and why you sometimes need to sample or aggregate data before visualising.

> **Source:** Wickham, H. (2016). *ggplot2: Elegant Graphics for Data Analysis*. Springer. Originally sourced from diamondse.info (2008). Bundled in the [seaborn](https://seaborn.pydata.org/) and [ggplot2](https://ggplot2.tidyverse.org/) libraries.

:::{callout} Overplotting with large datasets
With 53 940 rows, plotting every point creates a dense blob where nothing is visible. Use random sampling (`diamonds.sample(2000)`) or 2D binning (`mark_rect()` in Altair) when exploring this dataset.
:::

**Load:**
```python
import seaborn as sns
diamonds = sns.load_dataset('diamonds')
diamonds.head()
```

**Explore:**
```python
diamonds.describe()
```

**Visualise (sampled to avoid overplotting):**
```python
import altair as alt
alt.Chart(diamonds.sample(2000, random_state=42)).mark_circle(
    size=20, opacity=0.5
).encode(
    x='carat:Q',
    y='price:Q',
    color='cut:N',
    tooltip=['carat', 'cut', 'color', 'clarity', 'price']
).properties(title='Diamond Price vs Carat (random sample of 2 000)')
```


---

## Gapminder

Country-level life expectancy, GDP per capita, and population from 1952 to 2007 (every 5 years, 142 countries). Made famous worldwide by the late statistician and public health researcher Hans Rosling in his 2006 TED talk. The animated bubble chart Rosling used is one of the most influential data visualisations of the 21st century.

> **Source:** [Gapminder Foundation](https://www.gapminder.org/). Rosling, H. (2006). TED talk: ["The best stats you've ever seen"](https://www.ted.com/talks/hans_rosling_the_best_stats_you_ve_ever_seen). Data bundled in the [vega_datasets](https://github.com/altair-viz/vega_datasets) Python package.

**Load:**
```python
from vega_datasets import data
gapminder = data.gapminder()
gapminder.head()
```

**Explore:**
```python
gapminder.describe()
```

**Visualise (one year snapshot):**
```python
import altair as alt
alt.Chart(gapminder[gapminder.year == 2000]).mark_circle().encode(
    x=alt.X('fertility:Q', title='Fertility Rate'),
    y=alt.Y('life_expect:Q', title='Life Expectancy'),
    size=alt.Size('pop:Q', legend=None),
    color='cluster:N',
    tooltip=['country', 'life_expect', 'fertility', 'pop']
).properties(title='Life Expectancy vs Fertility Rate (Year 2000)')
```


---

## MNIST — Handwritten Digits

:::{note}
This dataset is approximately 55 MB. Downloading it for the first time may take a few minutes depending on your connection speed. Not recommended for the timed hands-on exercises.
:::

70 000 grayscale images (28 × 28 pixels) of handwritten digits 0–9, split into 60 000 training and 10 000 test images. The "hello world" benchmark of image classification and neural networks. Every machine learning practitioner has seen this dataset.

> **Source:** LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. (1998). "Gradient-based learning applied to document recognition." *Proceedings of the IEEE*, 86(11), 2278–2324. Dataset: [http://yann.lecun.com/exdb/mnist/](http://yann.lecun.com/exdb/mnist/).

**Load:**
```python
from sklearn.datasets import fetch_openml
mnist = fetch_openml('mnist_784', version=1, as_frame=True, parser='auto')
print(f"Images: {mnist.data.shape}")
print(f"Labels: {sorted(mnist.target.unique())}")
```

**Visualise a grid of sample digits:**
```python
import matplotlib.pyplot as plt
fig, axes = plt.subplots(2, 8, figsize=(12, 3))
for i, ax in enumerate(axes.flat):
    ax.imshow(mnist.data.iloc[i].values.reshape(28, 28), cmap='gray')
    ax.set_title(mnist.target.iloc[i])
    ax.axis('off')
plt.suptitle('MNIST: Sample Handwritten Digits')
plt.tight_layout()
plt.show()
```


---

## Fashion-MNIST

:::{note}
This dataset is approximately 30 MB. Downloading it for the first time may take a minute or two. Not recommended for the timed hands-on exercises.
:::

70 000 grayscale images (28 × 28 pixels) of clothing items across 10 categories: T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot. Drop-in replacement for MNIST (same format, same size) but considered harder and more visually interesting. Created by Zalando Research as a more realistic benchmark.

> **Source:** Xiao, H., Rasul, K., & Vollgraf, R. (2017). "Fashion-MNIST: a Novel Image Dataset for Benchmarking Machine Learning Algorithms." *arXiv:1708.07747*. Created by [Zalando Research](https://research.zalando.com/).

**Load:**
```python
from sklearn.datasets import fetch_openml
fashion = fetch_openml('Fashion-MNIST', version=1, as_frame=True, parser='auto')
label_names = ['T-shirt', 'Trouser', 'Pullover', 'Dress', 'Coat',
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']
print(f"Images: {fashion.data.shape}")
```

**Visualise a grid of sample items:**
```python
import matplotlib.pyplot as plt
fig, axes = plt.subplots(2, 8, figsize=(12, 3))
for i, ax in enumerate(axes.flat):
    ax.imshow(fashion.data.iloc[i].values.reshape(28, 28), cmap='gray')
    ax.set_title(label_names[int(fashion.target.iloc[i])], fontsize=8)
    ax.axis('off')
plt.suptitle('Fashion-MNIST: Sample Items')
plt.tight_layout()
plt.show()
```


---

## CIFAR-10 — Colour Images

:::{note}
This dataset is approximately 150 MB. Downloading it for the first time will take several minutes. Not recommended for the timed hands-on exercises.
:::

60 000 small colour images (32 × 32 pixels, RGB) across 10 real-world categories: airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck. The natural next step from MNIST — images are in colour and represent real-world objects. A standard benchmark for convolutional neural networks.

> **Source:** Krizhevsky, A. (2009). "Learning Multiple Layers of Features from Tiny Images." Technical Report, University of Toronto. Dataset: [https://www.cs.toronto.edu/~kriz/cifar.html](https://www.cs.toronto.edu/~kriz/cifar.html).

**Load:**
```python
from sklearn.datasets import fetch_openml
cifar = fetch_openml('CIFAR_10_small', version=1, as_frame=True, parser='auto')
label_names = ['airplane', 'automobile', 'bird', 'cat', 'deer',
               'dog', 'frog', 'horse', 'ship', 'truck']
print(f"Images: {cifar.data.shape}")
```

**Visualise a grid of sample images:**
```python
import matplotlib.pyplot as plt
import numpy as np
fig, axes = plt.subplots(2, 8, figsize=(12, 3))
for i, ax in enumerate(axes.flat):
    img = cifar.data.iloc[i].values.reshape(3, 32, 32).transpose(1, 2, 0)
    ax.imshow(img.astype('uint8'))
    ax.set_title(label_names[int(cifar.target.iloc[i])], fontsize=8)
    ax.axis('off')
plt.suptitle('CIFAR-10: Sample Images')
plt.tight_layout()
plt.show()
```
