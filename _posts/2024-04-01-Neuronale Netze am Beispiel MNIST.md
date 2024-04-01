---
title: Neuronale Netze am Beispiel MNIST
date: 2024-04-01 13:00:00 +/-TTTT
categories: [Maschinelles Lernen]
tags: [ML, DL, AI]     # TAG names should always be lowercase
toc: true
img_path: /assets/images/2024-04-01-Neuronale Netze am Beispiel MNIST_files/
---

# Wie kann man einem Computer beibringen, eine Ziffer zu erkennen? Oder Schriftzeichen? Handynummern? Schilder?
## Oder: Wie funktionieren Neuronale Netze? 

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


    
![png](2024-04-01-Neuronale Netze am Beispiel MNIST_12_0.png)

#### **Überblick über den MNIST Datensatz**

Der MNIST-Datensatz besteht aus insgesamt **70.000** Bildern von handgeschriebenen Ziffern. Davon sind **60.000** Bilder für das Training und **10.000** Bilder für das Testen vorgesehen. Die Bilder sind **schwarz-weiß** und haben eine Größe von **28x28** Pixeln.

Der Datensatz wurde in den 1990er Jahren von Yann LeCun, Corinna Cortes und Christopher J.C. Burges am Courant Institute of Mathematical Sciences der New York University erstellt. Er wurde entwickelt, um Algorithmen zur Erkennung von handgeschriebenen Ziffern zu trainieren und zu testen.

Die handgeschriebenen Ziffern im Datensatz stammen von einer Vielzahl von Personen und wurden auf Standardformularen geschrieben. Die Ziffern sind in einer zufälligen Reihenfolge angeordnet und besitzen keine besonderen Merkmale oder Muster, die die Erkennung beeinflussen könnten.

Der Datensatz ist ein Standardbenchmark-Datensatz und wird häufig verwendet, um die Leistungsfähigkeit von Algorithmen im Bereich des maschinellen Lernens zu vergleichen. Beim MNIST-Datensatz handelt es sich um ein einfaches und leicht verfügbares Datenset, weshalb es sehr häufig in Tutorials, Schulungen oder ähnlichem genutzt wird. Das **Hallo Welt!** des maschinellen Lernens eben.

Als erstes schauen wir uns ein einzelnes Bild genauer an. Dazu wählen wir eines zufällig aus und plotten es. Zum einen das Bild als solches, zum anderen wie es im Speicher abgelegt ist und wie Computer es nutzen, als Zahlen.


```python
# Show a random image
plt.rcParams.update({'font.size':8})

rnd = random.randint(0, random.randint(0, len(x_train)))
img = x_train[rnd]
ground_truth = y_train[rnd]

plt.figure(figsize = (20, 10))
plt.subplot(1,2,1)
plt.imshow(img, cmap="gray")

fig = plt.subplot(1,2,2)
fig.imshow(img, cmap="gray")
width, heigth = img.shape
thrs = img.max()/2.5
for x in range(width):
  for y in range(heigth):
    val = round(img[x][y], 2) if img[x][y] != 0 else 0
    ax.annotate(str(val), xy = (y, x),
                horizontalalignment = "center",
                verticalalignment = "center",
                color = "white" if img[x][y] < thrs else "black")

print(f"Label des zufällig gewählten Bildes: {ground_truth}")
```

    Label des zufällig gewählten Bildes: 4
    


![png](2024-04-01-Neuronale Netze am Beispiel MNIST_16_1.png)

Das zufällige Bild des Datensatzes hat die erwähnten $28x28$ Pixel und ist in Graustufen abgelegt. Als Bild ist es links dargestellt.
Die einzelnen Werte der Graustufen, also die Zahlen die ein jedes Pixel repräsentieren, sind in der rechten Ausgabe dargestellt. Es handelt sich dabei ursprünglich um Werte zwischen $0$ und $255$, also $8$ Bit je Pixel.  Der Wert gibt an, wie hell dieses Pixel ist.

Diese Zahlenwerte sind jene, die in das noch zu beschreibende Input-Layer eingespielt werden.

#### **Feature Maps**

Während das obige Bild des MNIST Datensatzes eine Feature Map hat, die Graustufenwerte, sieht das bei farbigen Bildern anders aus. Farbige Bilder haben für jeden Farbkanal eine eigene Feature Map: Rot, Grün, Blau. Gebräuchlich sind 8 Bit pro Kanal, was in Summe $16.777.216$ mögliche Farben ergibt. Bei der Erkennung von Ziffern können wir natürlich auf die Farbkanäle verzichten. Es genügen Grauwertstufen.

  <img src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*xGj2NQwOpsLpx1Ji.png", width="600">
  <figcaption>Die drei Feature Maps eines RGB-Bildes.</figcaption>
