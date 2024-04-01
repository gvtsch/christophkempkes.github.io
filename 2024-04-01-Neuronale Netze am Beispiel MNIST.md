---
title: Neuronale Netze am Beispiel MNIST
date: 2024-04-01 11:13:00 +/-TTTT
categories: [Maschinelles Lernen]
tags: [ML, DL, AI]     # TAG names should always be lowercase
toc: true
math: true
---

## Wie kann man einem Computer beibringen, eine Ziffer zu erkennen? 

In diesem Beitrag versuche ich, euch zu zeigen oder näher zu bringen, wie ein künstliches Neuronales Netz funktioniert. Weil das Thema sehr umfangreich ist, habe ich einen roten Faden einbauen wollen. An Hand eines Beispiels, dem roten Faden, hangel ich mich durch das Thema.

Wir werden das **Hallo Welt!** des maschinellen Lernens betrachten: Der **MNIST** Datensatz.

### **Der MNIST Datensatz**

Der MNIST-Datensatz ist eine bekannte Sammlung von handgeschriebenen Ziffern, die für die Entwicklung von Algorithmen zur Erkennung von Schriftzeichen und zur Bildverarbeitung von großer Bedeutung ist. In diesem Kapitel werde ich den MNIST-Datensatz genauer untersuchen und zeigen, wie man neuronale Netze trainieren kann, um handgeschriebene Ziffern zu erkennen.

Das Alles werde ich *live* mit Python-Code begleiten.
Am Ende werde ich euch eine kleine Python-Applikation zeigen, in der man mit der Maus Ziffern schreiben und die Klassifizierung durch das Neuronale Netz lesen kann.

Kurze Anmerkung zum Python-Code. Das geht alles wesentlich eleganter und effizienter. Allerdings glaube ich, dass der folgende Code für Nicht-Programmierer einfacher zu verstehen ist.

Als erstes tätigen wir ein paar Imports. In Python ermöglichen die Imports, auf bereits vorhandene Codebibliotheken zuzugreifen, anstatt alles von Grund auf neu schreiben zu müssen. Dies spart Zeit und ermöglicht es mir, auf bewährte und getestete Funktionen zurückzugreifen.


```python
# Necessary Imports
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
import matplotlib.pyplot as plt
import random
import numpy as np
```



Dann setzen wir ein paar Seeds. Seeds werden im Maschinenlernen verwendet, um die Zufälligkeit in den Algorithmen zu kontrollieren und reproduzierbare Ergebnisse zu erzielen. Es ist ratsam, verschiedene Seeds auszuprobieren, um die Robustheit des Modells zu gewährleisten.


```python
# Set seeds to make results reproduceable
seed = 42
random.seed(seed)
np.random.seed(seed)
tf.random.set_seed(seed)
```

In Keras ist der MNIST-Datensatz, der aus handgeschriebenen Ziffern besteht, bereits abgelegt. Es gibt eine Load-Funktion in Keras, mit der Sie diesen Datensatz einfach in Ihr Modell laden können. Davon mache ich hier Nutzen.


```python
# Load Dataset
mnist = tf.keras.datasets.mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()
```

Es folgt ein Codeblock, der die ersten 20 Bilder des Datensatzes darstellt.


```python
# Plot some (20) of the digits
fig = plt.figure(figsize=(12.5, 3))
for idx in np.arange(20):
  ax = fig.add_subplot(2, 10, idx + 1, xticks = [], yticks = [])
  ax.imshow(x_train[idx], cmap="gray")
  ax.set_title(str(y_train[idx]))

fig.suptitle("20 der Ziffern unseres Datensatzes")
plt.show()
```

