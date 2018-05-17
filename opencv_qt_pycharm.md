# Инструкция по сборке библиотеки OpenCV c Qt для Python2.7

Инструкция по сборке и установке библиотеки OpenCV c Qt для python 2.7

## Установка git 

Для этого сначала нужно обновить списки пакетов

```
$ sudo apt update 
```

А затем загрузить и установить программу 

```
$ sudo apt install git  
```
После этого git будет установлен и можно переходить к его настройке. А именно, нужно глобально указать имя пользователя и электронный адрес 

```
$ git config --global user.name <username>
$ git config --global user.email <useremail>
```

#### Пример 

```
user@user:~$ git config --global user.name  User
user@user:~$ git config --global user.email usermail@gmail.com 
```

Если установлен прокси, то нужно также указать и его 

```
$ git config --global http.proxy "http://<proxy>:<port>"
```

#### Пример 

```
user@user:~$ git config --global http.proxy "http://10.128.0.100:8080"
```

## Сборка и установка библиотеки OpenCV

Склонировать в свою папку [репозиторий OpenCV](https://github.com/opencv/opencv.git). Это можно сделать с помощью команды git

```
$ git clone https://github.com/opencv/opencv.git
```

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig1_clone.png "git clone")

Перейти в папку opencv. Потом перейти на ветку 3.4.0

```
$ git checkout 3.4.0 
```

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig2_branch340.png "branch 3.4.0")

Установить все дополнительные компоненты системы. Ввести пароль, чтобы дать разрешение на установку и согласиться на предоставления необходимого места пакетам. 

```
$ sudo apt install git cmake qt5-default libeigen3-dev libtbb-dev python-numpy
```

Создать в папке opencv папку с именем build. Перейти в неё. 

```
$ mkdir build 
$ cd build 
```

В папке build вызвать команду cmake с опциями: 

```
$ cmake   -D WITH_TBB=ON \
          -D WITH_EIGEN=ON \
          -D WITH_QT=ON \
          -D CMAKE_INSTALL_PREFIX=~/opencv_bin \
          -D CMAKE_BUILD_TYPE=RELEASE \
          ..
```

При успешной сборке, конфигурация покажет примерно такое строки 

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig3_conf.png "cmake conf")

После окончания сборки пакетов нужно вызвать установку 

```
$ make install   
```

Для корректной работы необходимо приписать в переменную PYTHONPATH путь до модуля Python. Указывается такое местоположение из-за того, что в cmake команде был указан путь установки ~/opencv_bin.

```
$ export PYTHONPATH=~/opencv_bin/lib/python2.7/dist-packages
```

## Проверка "успешности" сборки и установки библиотеки

Чтобы проверить, что сборка и установка прошли успешно, нужно в какой-нибудь папке запустить python2.7 и попробовать запустить тестовую программу, убедитесь, что в папке есть какая-нибудь картинка и вы знаете её имя и расширение.

```
$ python
$ print(“Hello”)
$ im = cv2.imread(‘test_pic.jpg’)
$ cv2.imshow(“test”, im)
$ cv2.waitKey(0)
```

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig4_testpr.png "test programm")

Чтобы завершить работу с python2.7 следует использовать сочетание клавиш Ctrl + D. 

## Установка IDE PYCharm 

Установите IDE для программирования на python2.7 с использованием собранных и установленных библиотек. 

```
$ sudo snap install pycharm-community --classic 
```

Создаём новый проект File – New Project..., указываем расположение проекта и его имя. 

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig5_py_crpr.png "create project")

После этого щёлкаем правой кнопкой мыши по появившемуся проекту во вкладке Project и выбираем New – Python File, указываем имя файла с расширением .py 

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig6_addpyf1.png "add new file 1")

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig7_addpyf2.png "add new file 2")

После этого переходим в File – Settings – Project – Project Interpreter, в графе Project Interpreter должно быть Python 2.7/usr/bin/python2.7 

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig8_conf_py.png "pycharm config")

Далее в файле main.py можно написать тестовую программу по выводу картинки в новое окно. При этом картинка должна находится там же, где и файл main. 

```python
import cv2

print("Hello")
im = cv2.imread('test_pic.jpg')
cv2.imshow("test", im)

cv2.waitKey(0)
```

Запуск программы можно осуществить несколькими способами: 
* из панели инструментов Run – Run main
* из консоли нажав зелёную стрелочку ![alt text]()
* с помощью горячих клавиш Shift + F10

В результате в консоли должно быть выведено слово Hello и в окне с именем test должна появиться картинка.

![alt text](https://github.com/serykhelena/Guides/blob/linux_os/pics_opencv_qt_pycharm/fig9_console1.png "success")

### Готово. Вы восхитительны (=