# QT Notes

## tutorials
[Qt5 tutorial](http://zetcode.com/gui/qt5/)

## UI Libs
1. QML Graphical User Interfaces, (better for mobile application)
2. Widget-based User Interfaces, (desktop environments)
3. Displaying Web Content

* [User Interfaces](https://doc.qt.io/qt-5/topics-ui.html)

## QML and QtQuick
* QML is the name of the language
* QtQuick is a toolkit for QML, allowing to develop graphical interface in QML language
* QML engine was based on JsCore (JS engine of Webkit) in Qt4.x and was rebased on V8 (JS engine of Google Chrome) with 5.0 but this disallows to use it on mobiles and especially on iOS, so Qt5.2 introduced a new QML engine, named V4VM, created by/for Qt guys.
----------
* [Difference between qt qml and qt quick](https://stackoverflow.com/a/19837895)

## Signals and Slots
* [Signals & Slots](https://doc.qt.io/qt-5/signalsandslots.html)


## QT Examples
* [Qt Quick Examples - Views](https://doc.qt.io/qt-5/qtquick-views-example.html)
* [Qt Quick Examples - Window and Screen](https://doc.qt.io/qt-5/qtquick-window-example.html)
* [zhengtianzuo/QtQuickExamples](https://github.com/zhengtianzuo/QtQuickExamples)
* [Address Book Example](https://doc.qt.io/qt-5/qtwidgets-itemviews-addressbook-example.html)
* [SDI Example](https://doc.qt.io/qt-5/qtwidgets-mainwindows-sdi-example.html)
* [Main Window](https://doc.qt.io/qt-5/qtwidgets-mainwindows-mainwindow-example.html)
* [Coquillo - MP3 Taggers](https://github.com/probonopd/linuxdeployqt/)
* [ErwanLegrand/Traverso-DAW](https://github.com/ErwanLegrand/Traverso-DAW)
* [papyros/qml-material](https://github.com/papyros/qml-material)
* [rschiang/material](https://github.com/rschiang/material)
* [penk/terrarium-app](https://github.com/penk/terrarium-app)


## QT Application Deployment
* static linking
* Deploying a Qt5 application for Linux as a standalone bundle involves bundling it with the necessary Qt components that are needed for the application to run. These can be Qt libraries, Qt plugins, and especially the Qt platform plugin.
* Using deployment tool to make an AppImage
* Creating an installer using the [Qt Installer Framework](https://doc.qt.io/qtinstallerframework/)
* using `ldd` to verify the dependencies
----------
* [Qt for Linux/X11 - Deployment](https://doc.qt.io/qt-5/linux-deployment.html)
* [Deploying a Qt5 Application Linux](https://wiki.qt.io/Deploying_a_Qt5_Application_Linux)
* [Deploying Qt Applications on Linux and Windows](https://lemirep.wordpress.com/2013/06/01/deploying-qt-applications-on-linux-and-windows-3/)
* [probonopd/linuxdeployqt](https://github.com/probonopd/linuxdeployqt/)

## PyQT
* [Getting Started with PyQt](https://wiki.python.org/moin/PyQt/Tutorials)
* [PyQt Signals and Slots](http://benhoff.net/pyqt-signals-slots.html)
* [[Quick PyQt5 : 1] Signal and Slot Example in PyQt5](https://blog.manash.me/quick-pyqt5-1-signal-and-slot-example-in-pyqt5-bf502ccaf11d)

* [function of pyqtSlot [duplicate]](https://stackoverflow.com/questions/45841843/function-of-pyqtslot)
* [Why do I need to decorate connected slots with pyqtSlot?](https://stackoverflow.com/questions/40325953/why-do-i-need-to-decorate-connected-slots-with-pyqtslot)
* [PyQt: Connecting a signal to a slot to start a background operation](https://stackoverflow.com/a/20818401)

* [Using PyInstaller](https://pyinstaller.readthedocs.io/en/stable/usage.html)
* [CREATING AN EXECUTABLE WITH PYQT5, PYINSTALLER, AND MORE](https://blog.aaronhktan.com/posts/2018/05/14/pyqt5-pyinstaller-executable#33-creating-a-deb-file-for-ubuntu)
