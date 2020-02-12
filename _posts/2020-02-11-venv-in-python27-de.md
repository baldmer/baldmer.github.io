---
layout: post
title: Das alte virtualenv für das alte Python 2
tags: [Linux, Python, Deutsch]
---


Vor kurzem habe ich mit einer `python2.7` Anwendung gearbeitet. Ich hatte ein Problem wenn ich versucht habe die python Umgebung zu installieren. Ich bekam die folgende Ausgabe.

```
$ python2.7 -m venv pyenv
/usr/bin/python2.7: No module named venv
```

Das Problem liegt darin, dass das Modul [venv](https://docs.python.org/3/library/venv.html) nicht in `python2.7` verfüngbar ist. Ich habe eine Alternative gefunden. Man kann das Tool [virtualenv](https://virtualenv.pypa.io/en/latest) benutzen.


```
$ sudo pip install virtualenv
```

Der Befehl oben nimmt an, dass man `pip` für `python2.7` schon installiert hat. Falls nicht, nutzt man den folgenden Befehl.


```
$ sudo apt-get install python-pip
```

Man erstellt schließlich die virtuelle Umgebung.


```
$ python2.7 -m virtualenv myenv
```

Und man kann es testen mit den folgenden Befehlen.


```
$ . myenv/bin/activate
$ python -V
Python 2.7.12
$ pip install numpy
...
...
```

`Merke:` Python 2 is jetzt im Ruhestand. Manche weniger bekannten Anwendungen sind immer noch in python 2. Python 3 Entwicklung wird empfohlen.





