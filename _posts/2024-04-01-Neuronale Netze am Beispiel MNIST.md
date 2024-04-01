---
title: Theorie und Formeln hinter dem Entscheidungsbaum
date: 2023-02-20 13:00:00 +/-TTTT
categories: [Maschinelles Lernen]
tags: [ML, DL, AI]     # TAG names should always be lowercase
toc: true
img_path: /images/2024-04-01-Neuronale Netze am Beispiel MNIST_files/
---


In diesem Beitrag versuche ich, euch zu zeigen oder näher zu bringen, wie ein künstliches Neuronales Netz funktioniert. Weil das Thema sehr umfangreich ist, habe ich einen roten Faden einbauen wollen. An Hand eines Beispiels, dem roten Faden, hangel ich mich durch das Thema.

Wir werden das **Hallo Welt!** des maschinellen Lernens betrachten: Der **MNIST** Datensatz.

### **Der MNIST Datensatz**

Der MNIST-Datensatz ist eine bekannte Sammlung von handgeschriebenen Ziffern, die für die Entwicklung von Algorithmen zur Erkennung von Schriftzeichen und zur Bildverarbeitung von großer Bedeutung ist. In diesem Kapitel werde ich den MNIST-Datensatz genauer untersuchen und zeigen, wie man neuronale Netze trainieren kann, um handgeschriebene Ziffern zu erkennen.

Das Alles werde ich *live* mit Python-Code begleiten.
Am Ende werde ich euch eine kleine Python-Applikation zeigen, in der man mit der Maus Ziffern schreiben und die Klassifizierung durch das Neuronale Netz lesen kann.

Kurze Anmerkung zum Python-Code. Das geht alles wesentlich eleganter und effizienter. Allerdings glaube ich, dass der folgende Code für Nicht-Programmierer einfacher zu verstehen ist.



Als erstes tätigen wir ein paar Imports. In Python ermöglichen die Imports, auf bereits vorhandene Codebibliotheken zuzugreifen, anstatt alles von Grund auf neu schreiben zu müssen. Dies spart Zeit und ermöglicht es mir, auf bewährte und getestete Funktionen zurückzugreifen.
