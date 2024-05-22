---
title: Sortieralgorithmen
date: 2024-05-19 11:00:00 +/-TTTT
categories: [PROGRAMMING]
tags: [python, programming]     # TAG names should always be lowercase
toc: true
img_path: /assets/images/2024-05-19-Sortieralgorithmen_files/
---

# Sortieralgorithmen mit Python

Ich habe mich neulich gefragt, was es für gängige Sortieralgorithmen gibt. Wie funktionieren sie? Wie schnell sind sie? Gibt es koriose Algorithmen usw.
Was ich dabei gelernt habe, möchte ich hier teilen. Ich werde verschiedene Algorithmen in Python umsetzen und versuchen den Vorgang grafisch darszustellen.
Dazu brauche ich zunächst einen Datensatz.

## Der zu sortierende Datensatz

Es wird ein einfacher Datensatz sein, den man hoffentlich gut visualisieren kann. Und zwar eine gewisse Anzahl von Integerwerten, die jedes Mal gleich randomisiert sein sollten. Eine Visualisierung forgt gleich.


```python
# Zunächst werden alle benötigten Bibliotheken importiert
import matplotlib.pyplot as plt
import random
import matplotlib.cm as cm
import os
import time

# Random Seed setzen
random.seed(42)
```


```python
# Anzahl der Elemente im Datensatz
n = 50

# Datensatz generieren
dataset = random.sample(range(1, n+1), n)
```

Dann definiere ich eine Funktion, die ich noch häufiger einsetzen werden, um das Sortieren zu visualisieren.


```python
cmap = cm.get_cmap('YlGnBu')  # Farbverlauf von Gelb über Grün zu Blau

# Funktion zum Aktualisieren des Balkendiagramms
def update_chart(data, iteration, xlim, ylim, folder_name, name = "Dataset",):
    i = len(data)
    colors = [cmap(x/i) for x in data]
    plt.bar(range(0, i), data, color=colors)
    plt.xlim(xlim)
    plt.ylim(ylim)
    plt.xticks([])
    plt.yticks([])
    
    if not os.path.exists(folder_name):
      os.makedirs(folder_name)

    if name == "Start": # Startkonfiguration
      plt.title(f'Startkonfiguration')
      plt.savefig(f'{folder_name}/{name}.png')
    else: # Sortierte Daten
      plt.title(f'Sortierung {name} - Schritt {iteration}')
      plt.savefig(f'{folder_name}/{name}_Iteration_{iteration:04d}.png')  # Speichere den Plot als PNG-Datei
    
    plt.close()
```  
Theoretisch muss man den Plot natürlich nicht abspeichern. Für meinen Workflow beim Erstellen dieses Beitrages ist das hingegen sehr hilfreich.
Und dann wird die Funktion auch schon das erste Mal aufgerufen. 

```python
update_chart(dataset, 1, xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen", name="Start")
```

Im Anschuss rufe ich den Plot wieder auf. Hier kannst du den unsortierten Datensatz erkennen, bestehend aus $n=50$ Integerwerten.

```python
# Load the image
image = plt.imread('Sortieralgorithmen/Start.png')

# Show the image
plt.imshow(image)
plt.axis('off')
plt.show()
```
    
![Startkonfiguration](Start.png)
    
Und dann soll es auch schon losgehen. 

## Sortieralgorithmen

Im folgenden werde ich ein paar Sortieralgorithmen vorstellen, die so weit ich weiß, zu den gängigsten gehören. Ich werde zu jedem Algorithmus ein paar Worte verlieren und den Algorithmus anschließend implementieren. Da ich den Sortiervorgang auch visualisieren möchte, werde ich ein paar zusätzliche, für den Algorithmus unnötige Zeilen programmieren. Ich habe versucht das alles konsistent zu halten, aber weil dieser Beitrag mit einigen Pausen entstanden ist, gibt es hier und da vermutlich ein paar Ausreißer. 

---

### Bubble Sort

Bubble Sort ist ein einfacher Vergleichssortieralgorithmus, der wiederholt benachbarte Elemente vergleicht und sie vertauscht, wenn sie in der falschen Reihenfolge sind. Dieser Prozess wird so lange fortgesetzt, bis keine Vertauschungen mehr nötig sind, was bedeutet, dass das Array sortiert ist. Der Name "Bubble Sort" kommt daher, dass kleinere Elemente wie Blasen im Wasser nach oben steigen, genau wie die zu sortierenden Elemente aufsteigen.
* **Funktionsweise**: In jedem Durchlauf wird das Array von Anfang bis Ende durchlaufen. Dabei werden jeweils zwei benachbarte Elemente verglichen. Wenn das linke Element größer ist als das rechte, werden sie vertauscht. Am Ende des ersten Durchlaufs befindet sich das größte Element am Ende des Arrays. Im zweiten Durchlauf wird das zweitgrößte Element gefunden und so weiter.
* **Zeitkomplexität**: Die Zeitkomplexität von Bubble Sort ist im Durchschnitt und im schlimmsten Fall $O(n^2)$, wobei $n$ die Anzahl der Elemente im Array ist. Für große Datensätze kann Bubble Sort daher sehr ineffizient sein und wird in der Praxis oft durch schnellere Sortieralgorithmen wie Quick Sort, Merge Sort oder Heap Sort ersetzt.
* **Besonderheiten**: Bubble Sort ist einfach zu verstehen und zu implementieren, aber ineffizient für große Datensätze. Er ist ein stabiler Sortieralgorithmus, d.h., die relative Reihenfolge gleicher Elemente bleibt erhalten.

![Bubble Sort](bubble_sort.gif)

Theoretich existiert der zu sortierende Datensatz bereits. Der Vollständigkeit halber, werde ich ihn dennoch jedes Mal erzeugen.

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```

Als nächstes definiere ich die Funktion mit dem Sortieralgorithmus.

```python
# Bubble Sort Algorithmus
def bubble_sort(data):
    def swap(i, j):
        """Vertauscht die Elemente an den Positionen i und j."""
        data[i], data[j] = data[j], data[i]  # Tausche die Elemente an den Positionen i und j

    sorted_datasets = []  # Initialisiere eine leere Liste, um die sortierten Datensätze zu speichern
    n = len(data)  # Die Anzahl der Elemente im Datensatz
    for i in range(n):  # Für jedes Element im Datensatz
        for j in range(0, n-i-1):  # Für jedes Element, das noch nicht sortiert ist
            if data[j] > data[j+1]:  # Wenn das aktuelle Element größer als das nächste Element ist
                swap(j, j+1)  # Tausche die beiden Elemente
            sorted_datasets.append(data[:])  # Speichere den aktuellen Zustand des Datensatzes
    return sorted_datasets  # Gib die Liste der sortierten Datensätze zurück
```

Danach rufe ich, und das wird sich an alle Sortieralgorithmen in meinem Beitrag anschließen, den Sortieralgorithmus auf und rufe für jeden Sortierschritt die Visualisierungsfunktion auf und speichere den Plot als PNG ab. Am Ende dieses Beitrags erstelle ich dann, ebenfalls mit Python, ein GIF von jedem Sortiervorgang. Das esrte Gif ist oben bereits zu sehen.

```python
for i, data in enumerate(bubble_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/bubble_sort", name="Bubble Sort")
```

Das was ich hier für Bubble Sort ausgeführt habe, führe ich so auch für die anderen Sortiervorgänge durch, werde es aber nicht mehr entsprechend kommentieren.

---

### Insertion Sort

Insertion Sort baut das sortierte Array schrittweise auf, indem er jedes Element aus dem unsortierten Teil nimmt und es an der richtigen Stelle im sortierten Teil einfügt.
* **Funktionsweise**: Der Algorithmus beginnt mit dem zweiten Element und vergleicht es mit dem ersten Element. Wenn es kleiner ist, wird es vor dem ersten Element eingefügt. Dann wird das dritte Element genommen und mit den beiden ersten verglichen und so weiter. Am Ende jedes Schritts ist der linke Teil des Arrays sortiert.
* **Zeitkomplexität**: $O(n²)$ im Durchschnitt und im schlimmsten Fall. Allerdings ist Insertion Sort effizienter als Bubble Sort, wenn das Array bereits teilweise sortiert ist. In diesem Fall muss jedes Element nur eine kurze Distanz bewegt werden, um an die richtige Position zu gelangen. Daher kann die Zeitkomplexität von Insertion Sort in solchen Fällen nahe an $O(n)$ liegen. Das gilt nur für teilweise sortierte Arrays. Für zufällig sortierte Arrays ist die Zeitkomplexität von Insertion Sort immer noch $O(n^2)$.
* **Besonderheiten**: Insertion Sort ist ein In-place-Algorithmus, d.h., er benötigt keinen zusätzlichen Speicherplatz. Er ist einfach zu implementieren und effizient für kleine Datensätze oder fast sortierte Daten.

![Insertion Sort](insertion_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```

```python
# Insertion Sort Algorithmus
def insertion_sort(data):
    def insert(j, key):
        """Verschiebt Elemente nach rechts und fügt das Schlüsselelement an der richtigen Position ein."""
        while j >= 0 and data[j] > key:  # Solange wir nicht am Anfang des Arrays sind und das aktuelle Element größer als das Schlüsselelement ist
            data[j+1] = data[j]  # Verschiebe das aktuelle Element nach rechts
            j -= 1  # Gehe zum nächsten Element auf der linken Seite
        data[j+1] = key  # Füge das Schlüsselelement an der richtigen Position ein

    sorted_datasets = []  # Initialisiere eine leere Liste, um die sortierten Datensätze zu speichern
    for i in range(1, len(data)):  # Starte bei der zweiten Position im Array
        insert(i - 1, data[i])  # Füge das Element `data[i]` an der richtigen Position ein
        sorted_datasets.append(data[:])  # Speichere den aktuellen Zustand des Arrays
    return sorted_datasets  # Gib die Liste der sortierten Datensätze zurück
```


```python
for i, data in enumerate(insertion_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/insertion_sort", name="Insertion Sort")
```

---

### Selection Sort

Selection Sort findet in jedem Durchlauf das kleinste Element im unsortierten Teil des Arrays und tauscht es mit dem ersten Element des unsortierten Teils.
* **Funktionsweise**: Der Algorithmus durchläuft das Array und findet das kleinste Element. Dieses wird mit dem ersten Element vertauscht. Dann wird der Vorgang für den restlichen, unsortierten Teil des Arrays wiederholt.
* **Zeitkomplexität**: $O(n²)$ in allen Fällen. Die Anzahl der Vergleiche ist unabhängig von der Anordnung der Elemente immer gleich.
* **Besonderheiten**: Selection Sort ist einfach zu verstehen und implementieren, aber es ist nicht effizient für große Datensätze. Ein Vorteil von Selection Sort gegenüber einigen anderen Sortieralgorithmen wie Bubble Sort ist, dass es die Anzahl der Vertauschungen minimiert, was nützlich sein kann, wenn das Vertauschen von Elementen eine teure Operation ist.

![Selection Sort](selection_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```


```python
def selection_sort(data):
    def find_min_index(start_index):
        """Findet den Index des kleinsten Elements ab dem gegebenen Startindex."""
        min_index = start_index
        for j in range(start_index+1, len(data)):
            if data[j] < data[min_index]:
                min_index = j
        return min_index

    sorted_datasets = []  # Initialisiere eine leere Liste, um die sortierten Datensätze zu speichern
    for i in range(len(data)-1):  # Für jedes Element im Datensatz, außer dem letzten
        min_index = find_min_index(i)  # Finde den Index des kleinsten Elements ab dem aktuellen Index
        data[i], data[min_index] = data[min_index], data[i]  # Tausche das aktuelle Element mit dem kleinsten Element
        sorted_datasets.append(data[:])  # Speichere den aktuellen Zustand des Datensatzes
    return sorted_datasets  # Gib die Liste der sortierten Datensätze zurück
```


```python
for i, data in enumerate(selection_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/selection_sort", name="Selection Sort")
```

### Merge Sort

Merge Sort ist ein "Teile und Herrsche"-Algorithmus, der das Array rekursiv in zwei Hälften teilt, jede Hälfte sortiert und dann die sortierten Hälften zusammenführt.
* **Funktionsweise**: Das Array wird solange halbiert, bis nur noch einzelne Elemente übrig sind. Diese sind trivialerweise sortiert. Dann werden die einzelnen Elemente zu sortierten Paaren zusammengeführt, dann Paare zu Vierergruppen und so weiter, bis das gesamte Array sortiert ist.
* **Zeitkomplexität**: Die Zeitkomplexität von Merge Sort ist in allen Fällen $O(n \cdot log(n))$, da das Array in jedem Schritt halbiert wird und dann die beiden Hälften in $O(n)$ Zeit zusammengeführt werden. Dies macht Merge Sort effizient für große Datensätze.
* **Besonderheiten**: Merge Sort ist ein stabiler Sortieralgorithmus, was bedeutet, dass gleichwertige Elemente in der sortierten Ausgabe die gleiche relative Reihenfolge haben wie in der Eingabe. Ein Nachteil von Merge Sort ist, dass er zusätzlichen Speicherplatz benötigt, um die beiden Hälften beim Zusammenführen zu speichern.

![Merge Sort](merge_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```


```python
import itertools

def merge_sort(data):
    steps = []  # Liste zur Speicherung der Zwischenschritte

    def merge(left, right, start):
        result = []  # Ergebnisliste
        i = j = 0  # Initialisiere die Indizes für die linke und rechte Liste
        # Durchlaufe beide Listen und füge das kleinere Element zur Ergebnisliste hinzu
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
            # Aktualisiere den entsprechenden Teil des ursprünglichen Arrays und speichere den Schritt
            data[start:start+len(result)] = result
            steps.append(list(data))

        # Füge die verbleibenden Elemente von left oder right hinzu
        for value in itertools.chain(left[i:], right[j:]):
            result.append(value)
            data[start:start+len(result)] = result
            steps.append(list(data))

        return result  # Gib die sortierte Liste zurück

    def sort(data, start=0):
        if len(data) <= 1:  # Wenn die Liste nur ein Element enthält, ist sie bereits sortiert
            return data
        mid = len(data) // 2  # Finde den mittleren Index
        left = data[:mid]  # Teile die Liste in zwei Hälften
        right = data[mid:]
        # Sortiere beide Hälften und führe sie zusammen
        return merge(sort(left, start), sort(right, start + mid), start)

    sort(data)  # Starte den Sortierprozess
    return steps  # Gib die Liste der Zwischenschritte zurück
```


```python
for i, data in enumerate(merge_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/merge_sort", name="Merge Sort")
```

### Quick Sort

Quick Sort ist ein weiterer "Teile und Herrsche"-Algorithmus. Er wählt ein Element als "Pivot" und partitioniert das Array in zwei Teilbereiche: Elemente kleiner als das Pivot und Elemente größer als das Pivot. Anschließend werden die Teilbereiche rekursiv sortiert.
* **Funktionsweise**: Der Algorithmus wählt ein Element als "Pivot" und teilt das Array in zwei Teilbereiche: Elemente kleiner als das Pivot und Elemente größer als das Pivot. Diese Teilbereiche werden dann rekursiv sortiert. Die Wahl des Pivots kann die Effizienz des Algorithmus stark beeinflussen.
* **Zeitkomplexität**: Die durchschnittliche Zeitkomplexität von Quick Sort ist $O(n \cdot log(n))$, aber im schlimmsten Fall (wenn das kleinste oder größte Element als Pivot gewählt wird) kann sie auf $O(n^2)$ ansteigen.
* **Besonderheiten**: Quick Sort ist ein In-place-Algorithmus, was bedeutet, dass er keinen zusätzlichen Speicherplatz benötigt. Er ist in der Praxis oft schneller als Merge Sort, obwohl seine Zeitkomplexität im schlechtesten Fall höher ist. Ein Nachteil von Quick Sort ist, dass er nicht stabil ist, d.h., gleichwertige Elemente können ihre relative Reihenfolge während der Sortierung ändern.

![Quick Sort](quick_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```


```python
def quick_sort_visualized(arr):
    """
    Implementiert Quick Sort und gibt eine Liste von Arrays zurück,
    die den Sortierverlauf darstellen (nach jeder Änderung).
    """
    snapshots = [arr[:]]  # Initialer Snapshot vor Beginn der Sortierung

    def _quick_sort(arr, low, high):
        if low < high:
            pivot_index = partition(arr, low, high)
            snapshots.append(arr[:])  # Snapshot nach jeder Swap-Operation
            _quick_sort(arr, low, pivot_index - 1)
            _quick_sort(arr, pivot_index + 1, high)

    def partition(arr, low, high):
        pivot = arr[high]
        i = low - 1
        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
                snapshots.append(arr[:])  # Snapshot nach jedem Swap
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        snapshots.append(arr[:])  # Snapshot nach dem finalen Swap
        return i + 1

    _quick_sort(arr, 0, len(arr) - 1)
    return snapshots
```


```python
for i, data in enumerate(quick_sort_visualized(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/quick_sort", name="Quick Sort")
```

### Heap Sort

Heap Sort nutzt eine spezielle Datenstruktur namens Heap, um das Array zu sortieren. Ein Heap ist ein binärer Baum, in dem jeder Knoten größer (oder kleiner, je nach Implementierung) ist als seine Kinder.
* **Funktionsweise**: Der Heap Sort Algorithmus beginnt mit der Umwandlung des Arrays in einen Heap. Dann wird das größte Element (die Wurzel des Heaps) entfernt und an das Ende des Arrays gestellt. Dieser Prozess wird wiederholt, bis das gesamte Array sortiert ist. Nach jedem Entfernen wird der Heap wiederhergestellt, um die Heap-Eigenschaft zu erhalten.
* **Zeitkomplexität**: Die Zeitkomplexität von Heap Sort ist $O(n \cdot log(n))$ in allen Fällen. Dies liegt daran, dass das Erstellen des Heaps $O(n)$ Zeit benötigt und das Entfernen jedes Elements $O(log(n))$ Zeit benötigt.
* **Besonderheiten**: Heap Sort ist ein In-place-Algorithmus, was bedeutet, dass er keinen zusätzlichen Speicherplatz benötigt. Er garantiert eine Zeitkomplexität von $O(n \cdot log(n))$, unabhängig von der Anordnung der Elemente. Ein Nachteil von Heap Sort ist, dass er komplexer zu implementieren ist als andere Sortieralgorithmen wie Quick Sort oder Merge Sort.

![Heap Sort](heap_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
dataset = random.sample(range(1, n+1), n)
```


```python
def heapify(arr, n, i, snapshots):
    """
    Hilfsfunktion, um den Heap an der Position i wiederherzustellen.
    """
    largest = i  # Größtes Element initialisieren

    # Überprüfen, ob das linke oder rechte Kind größer ist als die Wurzel
    for j in [2 * i + 1, 2 * i + 2]:  # j ist das linke und dann das rechte Kind
        if j < n and arr[largest] < arr[j]:
            largest = j

    # Wenn das größte Element nicht die Wurzel ist, tauschen und rekursiv heapify aufrufen
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        snapshots.append(arr.copy())  # Snapshot speichern
        heapify(arr, n, largest, snapshots)

def heap_sort(arr):
    """
    Implementiert Heap Sort und gibt eine Liste von Arrays zurück,
    die den Sortierverlauf darstellen.
    """
    n = len(arr)
    snapshots = [arr.copy()]  # Initialen Zustand speichern

    # Heap erstellen (Bottom-up)
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i, snapshots)

    # Ein Element nach dem anderen aus dem Heap entfernen und an die richtige Stelle setzen
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # Tausche Wurzel mit letztem Element
        snapshots.append(arr.copy())  # Snapshot speichern
        heapify(arr, i, 0, snapshots)  # Heap wiederherstellen

    return snapshots
```

```python
for i, data in enumerate(heap_sort(dataset)):
    update_chart(data, i+1, xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/Heap_sort", name="Heap Sort")
```

### Radix Sort

Radix Sort sortiert Zahlen nach ihren einzelnen Ziffern, beginnend mit der niedrigsten Stelle (Einerstelle).
* **Funktionsweise**: Radix Sort sortiert Zahlen basierend auf ihren einzelnen Ziffern, beginnend mit der niedrigsten Stelle (Einerstelle). In jedem Durchlauf werden die Zahlen in "Buckets" einsortiert, basierend auf der Ziffer an der aktuellen Stelle. Dann werden die Buckets in der richtigen Reihenfolge wieder zusammengefügt. Dieser Prozess wird für jede Stelle wiederholt, bis alle Stellen sortiert sind.
* **Zeitkomplexität**: Die Zeitkomplexität von Radix Sort ist $O(nk)$, wobei n die Anzahl der Elemente und k die maximale Anzahl von Stellen ist. Dies macht Radix Sort sehr effizient, wenn die Anzahl der Ziffern begrenzt ist.
* **Besonderheiten**: Radix Sort ist besonders effizient für ganze Zahlen mit begrenzter Stellenanzahl. Es ist kein vergleichsbasierter Algorithmus, sondern nutzt die Verteilung der Ziffern, um die Zahlen zu sortieren. Dies unterscheidet Radix Sort von vielen anderen Sortieralgorithmen, die auf Vergleichen basieren.

![Radix Sort](radix_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```


```python
def flatten_and_fill(buckets, arr_length):
    """
    Flacht die Buckets ab und füllt fehlende Werte mit Nullen auf.
    """
    flattened = [item for sublist in buckets for item in sublist]
    return flattened + [0] * (arr_length - len(flattened))

def radix_sort(arr):
    """
    Implementiert den Radix Sort Algorithmus und gibt eine Liste von Arrays zurück,
    die jeden einzelnen Sortierschritt darstellen.
    """
    max_value = max(arr)
    exp = 1
    snapshots = [arr.copy()]  # Initialer Zustand

    while max_value // exp > 0:
        buckets = [[] for _ in range(10)]
        for num in arr:
            digit = (num // exp) % 10
            buckets[digit].append(num)
            snapshots.append(flatten_and_fill(buckets, len(arr)))  # Save the snapshot

        arr = [num for bucket in buckets for num in bucket]
        exp *= 10

    return snapshots
```


```python
for i, data in enumerate(radix_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/radix_sort", name="Radix Sort")
```

### Bogo Sort

Bogo Sort ist ein ineffizienter und nicht-deterministischer Sortieralgorithmus. Er funktioniert, indem er das Array zufällig mischt und dann überprüft, ob es sortiert ist. Dieser Vorgang wird wiederholt, bis das Array zufällig in die richtige Reihenfolge gebracht wird. Aus diesem Grund habe ich die größe des Datensatzes verkleinert.
* **Funktionsweise**: Bogo Sort mischt das Array zufällig und überprüft dann, ob es sortiert ist. Dieser Prozess wird so lange wiederholt, bis das Array zufällig in die richtige Reihenfolge gebracht wird. Es gibt keine Garantie dafür, wie lange dies dauern wird. Im schlimmsten Fall kann es unendlich lange dauern.
* **Zeitkomplexität**: Die Zeitkomplexität von Bogo Sort ist im Durchschnitt und im schlimmsten Fall unendlich, da es keine Garantie dafür gibt, dass der Algorithmus jemals endet. Dies macht Bogo Sort extrem ineffizient.
* **Besonderheiten**: Bogo Sort ist ein Beispiel für einen extrem ineffizienten und unpraktischen Sortieralgorithmus. Er wird oft als humorvolles Beispiel für einen schlechten Algorithmus verwendet.

![Bogo Sort](bogo_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 5 # Reduzierte Größe des Datensatzes
dataset = random.sample(range(1, n+1), n)
```

```python
import random

def bogo_sort(data):
    steps = []  # Initialisiere eine Liste, um die Schritte des Sortierprozesses zu speichern

    # Wiederhole den Prozess, bis das Array sortiert ist
    while not all(data[i] <= data[i+1] for i in range(len(data)-1)):
        steps.append(list(data))  # Füge den aktuellen Zustand des Arrays zu den Schritten hinzu
        random.shuffle(data)  # Mische das Array zufällig

    steps.append(list(data))  # Füge das endgültig sortierte Array zu den Schritten hinzu

    return steps  # Gebe die Liste der Schritte zurück
```


```python
for i, data in enumerate(bogo_sort(dataset)):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/bogo_sort", name="Bogo Sort")
```

### Sleep Sort

Sleep Sort ist ein unkonventioneller und ineffizienter Sortieralgorithmus, der auf der Idee basiert, dass jeder Thread für eine Zeit "schläft", die proportional zum Wert des Elements ist. Auch hier verzichte darauf, einen großen Datensatz zu verwenden.
* **Funktionsweise**: Sleep Sort startet für jedes Element im Array einen Thread. Jeder Thread "schläft" für eine Zeit, die proportional zum Wert des Elements ist. Wenn ein Thread aufwacht, gibt er sein Element aus. Da Threads mit kleineren Werten zuerst aufwachen, werden die Elemente in sortierter Reihenfolge ausgegeben.
* **Zeitkomplexität**: Die Zeitkomplexität von Sleep Sort ist $O(n + max(arr))$, wobei $max(arr)$ das größte Element im Array ist. Dies macht Sleep Sort ineffizient für große Datenmengen oder Arrays mit sehr großen Werten.
* **Besonderheiten**: Sleep Sort ist nicht deterministisch, da die Reihenfolge der Ausgabe bei gleichen Elementen variieren kann. Es ist eher eine Kuriosität und nicht für den praktischen Einsatz gedacht.


![Sleep Sort](sleep_sort.gif)

```python
# Erzeuge einen zufälligen Datensatz
n = 50 
dataset = random.sample(range(1, n+1), n)
```


```python
import time
import threading

def sleep_sort(data):
    sorted_data = [0] * len(data)  # Initialisiere sorted_data mit Nullen
    all_steps = []  # Initialisiere die Liste, um alle Schritte zu speichern
    index = 0  # Initialisiere den Index

    def sleep_func(x):
        nonlocal index  # Deklariere index als nonlocal
        time.sleep(x/10)  # Lasse den Thread für eine Zeit proportional zum Wert von x schlafen
        sorted_data[index] = x  # Füge das Element am aktuellen Index ein
        index += 1  # Erhöhe den Index
        all_steps.append(list(sorted_data))  # Füge den aktuellen Zustand von sorted_data zu all_steps hinzu

    threads = []
    for num in data:
        t = threading.Thread(target=sleep_func, args=(num,))  # Erstelle einen Thread für jede Zahl im Array
        threads.append(t)
        t.start()  # Starte den Thread

    for t in threads:
        t.join()  # Warte, bis alle Threads beendet sind

    return all_steps  # Gebe die Liste aller Schritte zurück
```


```python
for i, data in enumerate(sleep_sort(dataset)):
    update_chart(data, i+1, xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/sleep_sort", name="Sleep Sort")
```

Es gibt noch viele weitere Sortieralgorithmen. Und den einen oder anderen werde ich vielleicht noch ergänzen.

## Gifs erstellen


```python
import os
import imageio
```


```python
from PIL import Image
import os

def calculate_duration(num_images, min_images=25, min_duration=50, max_duration=500, max_images=5000):
    if num_images < min_images:  # Wenn die Anzahl der Bilder kleiner als min_images ist
        duration = max_duration
    elif num_images >= max_images:  # Wenn die Anzahl der Bilder größer oder gleich max_images ist
        duration = min_duration
    else:  # Wenn die Anzahl der Bilder zwischen min_images und max_images liegt
        slope = (min_duration - max_duration) / (max_images - min_duration)
        duration = max_duration + slope * (num_images - min_duration)
    return duration  # Gib die berechnete Dauer zurück

def create_gif_from_pngs(folder_path, output_path):
    # Erstelle eine Liste von Bildern aus den PNG-Dateien im angegebenen Ordner
    images = [Image.open(os.path.join(folder_path, filename)) for filename in sorted(os.listdir(folder_path)) if filename.endswith(".png")]

    # Berechne die Dauer für das GIF
    duration = calculate_duration(len(images), min_images=5, min_duration=100, max_duration=200, max_images=5000)

    # Speichere die Bilder als animiertes GIF
    images[0].save(output_path, save_all=True, append_images=images[1:], loop=0, duration=duration)
```


```python
folder_path = "Sortieralgorithmen"
# Erstelle eine Liste von Ordnern im angegebenen Pfad
folder_list = [folder for folder in os.listdir(folder_path) if os.path.isdir(os.path.join(folder_path, folder))]

for folder in folder_list:
    # Erstelle das GIF für jeden Ordner und speichere es im gleichen Ordner
    create_gif_from_pngs(os.path.join(folder_path, folder), os.path.join(folder_path, f"{folder}.gif"))

```
