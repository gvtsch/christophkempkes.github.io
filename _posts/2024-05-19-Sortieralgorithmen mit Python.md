---
title: Sortieralgorithmen mit Python
date: 2024-04-01 13:00:00 +/-TTTT
categories: [Programmieren]
tags: [Python]     # TAG names should always be lowercase
toc: true
img_path: /assets/images/2024-05-19-Sortieralgorithmen_files/
---

# Sortieralgorithmen mit Python

Ich habe mich neulich gefragt, was es für gängige Sortieralgorithmen gibt. Wie funktionieren sie? Wie schnell sind sie? Gibt es koriose Algorithmen usw.
Was ich dabei gelernt habe, möchte ich hier teilen. Ich werde verschiedene Algorithmen in Python umsetzen und versuchen den Vorgang grafisch darszustellen.
Dazu brauche ich zunächst einen Datensatz.

## Der zu sortierende Datensatz

Es wird ein einfacher Datensatz sein, den man hoffentlich gut visualisieren kann. Und zwar eine gewisse Anzahl von Integerwerten, die jedes Mal gleich randomisiert sein sollten.


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
# Anzahl der ELemente im Datensatz
n = 50

# Datensatz generieren
dataset = random.sample(range(1, n+1), n)
```

Dann definieren eine Funktion die wir noch häufiger einsetzen werden, um das Sortieren zu visualisieren.


```python
# Farbverlaufsfunktion
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

    if name == "Start":
      plt.title(f'Startkonfiguration')
      plt.savefig(f'{folder_name}/{name}.png')
    else:
      plt.title(f'Sortierung {name} - Schritt {iteration}')
      plt.savefig(f'{folder_name}/{name}_Iteration_{iteration:04d}.png')  # Speichere den Plot als PNG-Datei
    
    plt.close()
```  


```python
update_chart(dataset, 1, xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen", name="Start")
```


```python

# Load the image
image = plt.imread('Sortieralgorithmen/Start.png')

# Show the image
plt.imshow(image)
plt.axis('off')
plt.show()
```


    
![png](Start.png)
    


## Sortieralgorithmen

Im folgenden werde ich ein paar Sortieralgorithmen vorstellen, die so weit ich weiß, zu den gängigsten gehören. Ich werde zu jedem Algorithmus ein paar Worte verlieren und den Algorithmus anschließend implementieren. Da ich das Sortieren auch visualisieren möchte, werde ich ein paar zusätzliche, für den Algorithmus unnötige Zeilen programmieren. Ich habe versucht das alles konsistent zu halten, aber weil dieser Beitrag mit einigen Pausen entstanden ist, gibt es hier und da vermutlich ein paar Ausreißer. 

### Bubble Sort

Bubble Sort ist ein einfacher Vergleichssortieralgorithmus, der wiederholt benachbarte Elemente vergleicht und sie vertauscht, wenn sie in der falschen Reihenfolge sind. Dieser Prozess wird so lange fortgesetzt, bis keine Vertauschungen mehr nötig sind, was bedeutet, dass das Array sortiert ist. Der Name "Bubble Sort" kommt daher, dass kleinere Elemente wie Blasen im Wasser nach oben steigen, genau wie die zu sortierenden Elemente aufsteigen.
* **Funktionsweise**: In jedem Durchlauf wird das Array von Anfang bis Ende durchlaufen. Dabei werden jeweils zwei benachbarte Elemente verglichen. Wenn das linke Element größer ist als das rechte, werden sie vertauscht. Am Ende des ersten Durchlaufs befindet sich das größte Element am Ende des Arrays. Im zweiten Durchlauf wird das zweitgrößte Element gefunden und so weiter.
* **Zeitkomplexität**: $O(n²)$ im Durchschnitt und im schlimmsten Fall. Das bedeutet, dass die Anzahl der Vergleiche und Vertauschungen quadratisch mit der Anzahl der Elemente im Array zunimmt. Für große Datensätze wird Bubble Sort daher sehr langsam.
* **Besonderheiten**: Bubble Sort ist einfach zu verstehen und zu implementieren, aber ineffizient für große Datensätze. Er ist ein stabiler Sortieralgorithmus, d.h., die relative Reihenfolge gleicher Elemente bleibt erhalten.

![Bubble Sort]("bubble_sort.gif")

```python
# Bubble Sort Algorithmus
def bubble_sort(data, ):
    sorted_datasets = []
    for i in range(len(data)-1):
        for j in range(len(data)-1-i):
            # Vergleiche benachbarte Elemente und tausche sie, falls sie in der falschen Reihenfolge sind
            if data[j] > data[j+1]:
                data[j], data[j+1] = data[j+1], data[j]
            # Speichere den sortierten Datensatz nach jedem Sortierschritt
            sorted_datasets.append(data.copy())

    return sorted_datasets
```


```python
bubble_sorted_datasets = bubble_sort(dataset,)
```


```python
for i, data in enumerate(bubble_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/bubble_sort", name="Bubble Sort")
```

### Insertion Sort

Insertion Sort baut das sortierte Array schrittweise auf, indem er jedes Element aus dem unsortierten Teil nimmt und es an der richtigen Stelle im sortierten Teil einfügt.
* **Funktionsweise**: Der Algorithmus beginnt mit dem zweiten Element und vergleicht es mit dem ersten Element. Wenn es kleiner ist, wird es vor dem ersten Element eingefügt. Dann wird das dritte Element genommen und mit den beiden ersten verglichen und so weiter. Am Ende jedes Schritts ist der linke Teil des Arrays sortiert.
* **Zeitkomplexität**: $O(n²)$ im Durchschnitt und im schlimmsten Fall, aber effizienter als Bubble Sort für teilweise sortierte Daten. Bei fast sortierten Arrays nähert sich die Zeitkomplexität $O(n)$.
* **Besonderheiten**: Insertion Sort ist ein In-place-Algorithmus, d.h., er benötigt keinen zusätzlichen Speicherplatz. Er ist einfach zu implementieren und effizient für kleine Datensätze oder fast sortierte Daten.


```python
# Erzeuge einen zufälligen Datensatz
dataset = random.sample(range(1, n+1), n)
```


```python
# Insertion Sort Algorithmus
def insertion_sort(data, ):
    sorted_datasets = []
    for i in range(1, len(data)):
        key = data[i]  # Das aktuell zu sortierende Element
        j = i - 1
        while j >= 0 and data[j] > key:
            data[j+1] = data[j]  # Verschiebe Elemente nach rechts
            j -= 1
            # Speichere den sortierten Datensatz nach jedem Sortierschritt
            sorted_datasets.append(data.copy())
        data[j+1] = key  # Füge das Element an der richtigen Position ein
        
    return sorted_datasets
```


```python
insertion_sorted_datasets = insertion_sort(dataset)
```


```python
for i, data in enumerate(insertion_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/insertion_sort", name="Insertion Sort")
```

### Selection Sort

Selection Sort findet in jedem Durchlauf das kleinste Element im unsortierten Teil des Arrays und tauscht es mit dem ersten Element des unsortierten Teils.
* **Funktionsweise**: Der Algorithmus durchläuft das Array und findet das kleinste Element. Dieses wird mit dem ersten Element vertauscht. Dann wird der Vorgang für den restlichen, unsortierten Teil des Arrays wiederholt.
* **Zeitkomplexität**: $O(n²)$ in allen Fällen. Die Anzahl der Vergleiche ist unabhängig von der Anordnung der Elemente immer gleich.
* **Besonderheiten**: Selection Sort ist einfach zu verstehen, aber ineffizient für große Datensätze. Er führt weniger Vertauschungen durch als Bubble Sort, was bei teuren Vertauschungsoperationen von Vorteil sein kann.


```python
# Erzeuge einen zufälligen Datensatz
dataset = random.sample(range(1, n+1), n)
```


```python
def selection_sort(data):
    sorted_datasets = []
    for i in range(len(data)-1):
        min_index = i
        for j in range(i+1, len(data)):
            if data[j] < data[min_index]:
                min_index = j
                sorted_datasets.append(data.copy())
        data[i], data[min_index] = data[min_index], data[i]
        
    return sorted_datasets
```


```python
selection_sorted_datasets = selection_sort(dataset)
```


```python
for i, data in enumerate(selection_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/selection_sort", name="Selection Sort")
```

### Merge Sort

Merge Sort ist ein "Teile und Herrsche"-Algorithmus, der das Array rekursiv in zwei Hälften teilt, jede Hälfte sortiert und dann die sortierten Hälften zusammenführt.
* **Funktionsweise**: Das Array wird solange halbiert, bis nur noch einzelne Elemente übrig sind. Diese sind trivialerweise sortiert. Dann werden die einzelnen Elemente zu sortierten Paaren zusammengeführt, dann Paare zu Vierergruppen und so weiter, bis das gesamte Array sortiert ist.
* **Zeitkomplexität**: $O(n \cdot log(n))$ in allen Fällen. Die Anzahl der Vergleiche wächst logarithmisch mit der Anzahl der Elemente, was ihn effizient für große Datensätze macht.
* **Besonderheiten**: Merge Sort ist ein stabiler Sortieralgorithmus und effizient für große Datensätze. Er benötigt jedoch zusätzlichen Speicherplatz für die temporären Arrays beim Zusammenführen.


```python
# Erzeuge einen zufälligen Datensatz
dataset = random.sample(range(1, n+1), n)
```


```python
def merge_sort(data):
    steps = []

    def merge(left, right, start):
        result = []
        i = j = 0
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
            data[start:start+len(result)] = result
            steps.append(list(data))
        while i < len(left):
            result.append(left[i])
            i += 1
            data[start:start+len(result)] = result
            steps.append(list(data))
        while j < len(right):
            result.append(right[j])
            j += 1
            data[start:start+len(result)] = result
            steps.append(list(data))
        return result

    def sort(data, start=0):
        if len(data) <= 1:
            return data
        mid = len(data) // 2
        left = data[:mid]
        right = data[mid:]
        return merge(sort(left, start), sort(right, start + mid), start)

    sort(data)
    return steps
```


```python
merge_sorted_datasets = merge_sort(dataset)
```


```python
for i, data in enumerate(merge_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/merge_sort", name="Merge Sort")
```

### Quick Sort

Quick Sort ist ein weiterer "Teile und Herrsche"-Algorithmus. Er wählt ein Element als "Pivot" und partitioniert das Array in zwei Teilbereiche: Elemente kleiner als das Pivot und Elemente größer als das Pivot. Anschließend werden die Teilbereiche rekursiv sortiert.
* **Funktionsweise**: Die Wahl des Pivots ist entscheidend für die Effizienz. Im Idealfall teilt das Pivot das Array in zwei gleich große Hälften. Im schlimmsten Fall ist das Pivot das kleinste oder größte Element, was zu einer Zeitkomplexität von $O(n²)$ führt.
* **Zeitkomplexität**: $O(n \cdot log(n))$ im Durchschnitt, $O(n²)$ im schlimmsten Fall (selten).
* **Besonderheiten**: Quick Sort ist ein In-place-Algorithmus und oft schneller als Merge Sort in der Praxis. Er ist jedoch nicht stabil.


```python
# Erzeuge einen zufälligen Datensatz
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
quick_sorted_datasets = quick_sort_visualized(dataset)
```


```python
for i, data in enumerate(quick_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/quick_sort", name="Quick Sort")
```

### Heap Sort

Heap Sort nutzt eine spezielle Datenstruktur namens Heap, um das Array zu sortieren. Ein Heap ist ein binärer Baum, in dem jeder Knoten größer (oder kleiner, je nach Implementierung) ist als seine Kinder.
* **Funktionsweise**: Zuerst wird das Array in einen Heap umgewandelt. Dann wird wiederholt das größte Element (die Wurzel des Heaps) entfernt und an das Ende des Arrays gestellt. Nach jedem Entfernen wird der Heap wiederhergestellt.
* **Zeitkomplexität**: $O(n log n)$ in allen Fällen.
* **Besonderheiten**: Heap Sort ist ein In-place-Algorithmus und garantiert eine Zeitkomplexität von $O(n log n)$, unabhängig von der Anordnung der Elemente. Er ist jedoch etwas komplexer zu implementieren als andere Algorithmen.


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
    l = 2 * i + 1  # Linkes Kind
    r = 2 * i + 2  # Rechtes Kind

    # Überprüfen, ob das linke Kind größer ist als die Wurzel
    if l < n and arr[i] < arr[l]:
        largest = l

    # Überprüfen, ob das rechte Kind größer ist als das größte bisher gefundene Element
    if r < n and arr[largest] < arr[r]:
        largest = r

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
heap_sorted_datasets = heap_sort(dataset)
```


```python
for i, data in enumerate(heap_sorted_datasets):
    update_chart(data, i+1, xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/Heap_sort", name="Heap Sort")
```

### Radix Sort

Radix Sort sortiert Zahlen nach ihren einzelnen Ziffern, beginnend mit der niedrigsten Stelle (Einerstelle).
* **Funktionsweise**: In jedem Durchlauf werden die Zahlen in "Buckets" einsortiert, je nachdem, welche Ziffer sie an der aktuellen Stelle haben. Dann werden die Buckets in der richtigen Reihenfolge wieder zusammengefügt. Dieser Vorgang wird für jede Stelle wiederholt, bis alle Stellen sortiert sind.
* **Zeitkomplexität**: $O(nk)$, wobei n die Anzahl der Elemente und k die maximale Anzahl von Stellen ist.
* **Besonderheiten**: Radix Sort ist effizient für ganze Zahlen mit begrenzter Stellenanzahl. Er ist kein vergleichsbasierter Algorithmus, sondern nutzt die Verteilung der Ziffern.


```python
# Erzeuge einen zufälligen Datensatz
n=50
dataset = random.sample(range(1, n+1), n)
```


```python
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
            # Flatten the buckets and fill the missing values with zeros
            current_step = [item for sublist in buckets for item in sublist] + [0] * (len(arr) - len([item for sublist in buckets for item in sublist]))
            snapshots.append(current_step)  # Save the snapshot

        arr = [num for bucket in buckets for num in bucket]
        exp *= 10

    return snapshots
```


```python
radix_sorted_datasets = radix_sort(dataset)
```


```python
for i, data in enumerate(radix_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/radix_sort", name="Radix Sort")
```

### Bogo Sort

Bogo Sort ist ein ineffizienter und nicht-deterministischer Sortieralgorithmus. Er funktioniert, indem er das Array zufällig mischt und dann überprüft, ob es sortiert ist. Dieser Vorgang wird wiederholt, bis das Array zufällig in die richtige Reihenfolge gebracht wird. Aus diesem Grund habe ich die größe des Datensatzes verkleinert.
* **Funktionsweise**: Der Algorithmus mischt das Array wiederholt, bis es zufällig sortiert ist. Es gibt keine Garantie dafür, wie lange dies dauern wird. Im schlimmsten Fall kann es unendlich lange dauern.
* **Zeitkomplexität**: Im Durchschnitt und im schlimmsten Fall unendlich, da keine Garantie für eine Beendigung besteht.
* **Besonderheiten**: Bogo Sort ist extrem ineffizient und unpraktisch. Er dient nur als humorvolles Beispiel für einen schlechten Algorithmus.


```python
# Erzeuge einen zufälligen Datensatz
n = 5
dataset = random.sample(range(1, n+1), n)
print(dataset)
```

    [2, 3, 5, 1, 4]
    


```python
def is_sorted(data):
    for i in range(len(data) - 1):
        if data[i] > data[i+1]:
            return False
    return True

def bogo_sort(data):
    steps = []
    while not is_sorted(data):
        steps.append(list(data))  # Add the current state of the array to the steps
        random.shuffle(data)
    steps.append(list(data))  # Add the final sorted array to the steps
    return steps
```


```python
bogo_sorted_datasets = bogo_sort(dataset)
```


```python
for i, data in enumerate(bogo_sorted_datasets):
    update_chart(data, i+1,  xlim=[0, n+1], ylim=[0, n+.5], folder_name="Sortieralgorithmen/bogo_sort", name="Bogo Sort")
```

### Sleep Sort

Sleep Sort ist ein unkonventioneller und ineffizienter Sortieralgorithmus, der auf der Idee basiert, dass jeder Thread für eine Zeit "schläft", die proportional zum Wert des Elements ist. Auch hier verzichte darauf, einen großen Datensatz zu verwenden.
* **Funktionsweise**: Für jedes Element im Array wird ein Thread gestartet. Jeder Thread schläft für eine Zeit, die dem Wert des Elements entspricht. Wenn ein Thread aufwacht, gibt er sein Element aus. Da die Threads mit kleineren Werten zuerst aufwachen, werden die Elemente in sortierter Reihenfolge ausgegeben.
* **Zeitkomplexität**: $O(n + max(arr))$, wobei $max(arr)$ das größte Element im Array ist.
* **Besonderheiten**: Sleep Sort ist nicht deterministisch, da die Reihenfolge der Ausgabe bei gleichen Elementen variieren kann. Er ist ineffizient und dient eher als Kuriosität.


```python
# Erzeuge einen zufälligen Datensatz
n = 50
dataset = random.sample(range(1, n+1), n)
```


```python
import time
import threading

def sleep_sort(data):
    
    sorted_data = [0] * len(data)  # Initialize sorted_data with zeros
    all_steps = []  # Initialize the list to store all steps
    index = 0  # Initialize index
    
    def sleep_func(x):
        nonlocal index  # Declare index as nonlocal so it can be modified inside this function
        time.sleep(x/10)
        sorted_data[index] = x  # Insert the element at the current index
        index += 1  # Increment the index
        all_steps.append(list(sorted_data))  # Append the current state of sorted_data to all_steps
    
    threads = []
    for num in data:
        t = threading.Thread(target=sleep_func, args=(num,))
        threads.append(t)
        t.start()
    
    for t in threads:
        t.join()
    
    return all_steps  # Return the list of all steps
```


```python
sleep_sorted_datasets = sleep_sort(dataset)
```


```python
for i, data in enumerate(sleep_sorted_datasets):
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


# Save the frames as an animated GIF
def calculate_duration(num_images, min_images=25, min_duration=50, max_duration=500, max_images=5000):
    if num_images < min_images:
        duration = max_duration
    elif num_images >= max_images:
        duration = min_duration
    else:
        slope = (min_duration - max_duration) / (max_images - min_duration)
        duration = max_duration + slope * (num_images - min_duration)
    return duration

def create_gif_from_pngs(folder_path, output_path):
    images = []
    for filename in sorted(os.listdir(folder_path)):  # Use sorted to ensure the images are in the correct order
        if filename.endswith(".png"):
            file_path = os.path.join(folder_path, filename)
            images.append(Image.open(file_path))
    
    duration = calculate_duration(len(images), min_images=5, min_duration=100, max_duration=200, max_images=5000)
    images[0].save(output_path, save_all=True, append_images=images[1:], loop=0, duration=duration)
```


```python
folder_path = "Sortieralgorithmen"
folder_list = [folder for folder in os.listdir(folder_path) if os.path.isdir(os.path.join(folder_path, folder))]

for folder in folder_list:
    output_path = os.path.join("Sortieralgorithmen", f"{folder}.gif")
    create_gif_from_pngs(os.path.join("Sortieralgorithmen", folder), output_path)

```
