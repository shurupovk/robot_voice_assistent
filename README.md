# Документация для обучения фразе «Привет Мисис»
Ниже приведены пошаговые действия, чтобы уставновить кастомный wake-word на microft
## Установка microft-precise
По умолчанию Ubuntu 18.04 поставляется с Python 3.6. Нам нужно обновить его до 3.7, но мы хотим убедиться, что Gnome по-прежнему может использовать 3.6. В терминале введите:
```
sudo apt install -y python3.7
sudo nano /usr/bin/gnome-terminal
In that gnome-terminal file, change
```
В этом gnom-термина извените ```#!/usr/bin/python3``` на ```#!/usr/bin/python3.6```
Сохраните и выйдите. Далее введите:
```
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
python3 --version
```
Вы должны увидеть, что `python3` теперь указывает на версию 3.7.
Теперь мы можем установить зависимости и точные инструменты обучения. В терминале введите:
```
sudo apt install -y git python3.7-dev
sudo add-apt-repository universe
mkdir ~/Projects
cd ~/Projects
git clone https://github.com/MycroftAI/mycroft-precise.git
cd mycroft-precise
nano setup.py
```
В этом setup.py файле измените:
``` h5py ``` на  ```h5py<3.0.0 ```
Сохраните и выйдите. Наконец, запустите:
```
./setup.sh
```
Подождите, пока все установится. Это займет некоторое время.
## Обучение модели
Убедитесь, что ваш набор данных находится в каталоге ваших проектов. Например, если вы тренируетесь с набором данных *privet-misis*, он должен быть расположен по адресу ``` ~/Projects/privet-misis ```.
Откройте терминал и включите точную виртуальную среду с помощью следующих действий:
``` cd ~/Projects
source mycroft-precise/.venv/bin/activate
```
Выполните обучение, введя:
```
precise-train -e 800 hey-jorvon.net hey-jorvon/ 
```
Вы можете поиграть с параметрами обучающего инструмента:
-	-e number of epochs
-	-s target sensitivity (default: 0.2)

Чтобы протестировать свою модель, вы можете воспользоваться инструментом *precise-test*:
```
precise-test privet-misis.net privet-misis/
```
Он будет использовать каталог test/, найденный в вашей папке privet-misis/ dataset, для тестирования вашей модели. Проверьте количество ложных срабатываний. Кажется ли это разумным? Точность более 0,90 в тестовом наборе - хорошее начало.
## Выгрузка кастомной wake-word модели

Теперь, когда вы готовы к развертыванию своей модели, введите следующую команду:
```
precise-convert privet-misis.net
```
В результате будут созданы файлы ``` privet-misis.pb ``` и ```privet-misis.pb.params ```
Полезные ссылки:
https://askubuntu.com/questions/1435918/terminal-not-opening-on-ubuntu-22-04-on-virtual-box-7-0-0
https://askubuntu.com/questions/314685/is-there-a-way-to-make-a-fullscreen-on-virtualbox
https://www.digikey.com/en/maker/projects/how-to-create-a-custom-wake-word-for-mycroft-ai/3f0b7486b9804687b6a9e5164ee4cf99
## Установка Mycroft-core
```
cd ~/
git clone https://github.com/MycroftAI/mycroft-core.git
cd mycroft-core
bash dev_setup.sh
```
Также надо установить поддержку русского языка
```
git clone https://github.com/your-username/lingua-franca/
source .venv/bin/activate
pip install /path/to/your/lingua-franca
mycroft-config set lang "ru-ru"
```
Установить скилл на вопросы про мисис
```
mycroft-msm install https://github.com/Yana999/about-misis-skill
в папку /opt/mycroft/skills/about-misis-skill.yana999/ 
```
Создать папку ``` lev_model ``` и положить туда файл ``` best_model_state.bin ``` (https://drive.google.com/file/d/1Lp4GGmM3td1cs6mzDSyLhEyHAs4SMgOL/view?usp=sharing)
Для запуска Mycroft-core
```
start-mycroft.sh debug
```
Полезные ссылки
https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/get-mycroft/linux
https://github.com/MycroftAI/lingua-franca
Обучение модели проводилось так: https://github.com/Izlaster/Question_Answering_RuBert_Classification
Скилл сформированный на основе этой модели: https://github.com/Yana999/about-misis-skill
