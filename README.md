# ioT-lab-experiments
Описание работы с iot-lab.  
## Подготовка к эксперементу
### Связь компютера с учетной записью на веб-сервире 
Для взаимодействия интерфейса сайта IoT-LAB  с терминалом Ubuntu используется протокол SSH. Мы свзывали сгенирированный ключ SSH на компьютере с учетной записью.  
**Генерация SSH-ключа в терминале**
```
$ ssh-keygen -t rsa
```
**Копирования ключа в буфер обмена**
```
$ less ~/.ssh/id_rsa.pub
```
**Вывод ключа**
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC4/F0SIHKXaPdL7BHyjKmKW/zoMCOyXIjd9XIOqAS/vR1f9YsQIoKFgzwv5QTd3Kbdi8JGK+cKazhreMw3b5IjkrW7Heh6tT+ZAZNozo2roPDDgDaK4rJJ1ddP85OaCnsav6QARY1TdxbjKFZV8l5bKccdfUxFFcW0lyt6huXbJutyQO3ilBvVWgY42cUXgzLxAPZkNSKEc3AqXaq4aFueHjLhgurKQ/HVFRDOQvviFzTWxBWJoGUWUcqgSYKJ0PITFrRbCWujo9gAkH1Tk4SBBQYyDZXlWc3HYC9Ae/F6SfNWOufCgqjWgmM5fla4o0ZdK4FnauFuBdz+DBmPNNdf/kK3Gf91Mw8gNQzWW8R9agQTmem16WX11P0DG9duY8y5dJFPHmTvUwBDpq23Y4jvjX9YPrkx8Wk1pwGsMCsW63nK/ocRIzuQv0ADgGmPNKRhl34i1BuoQBx8y+R+7DC9cMOI1ov5FHYGSOZwVd9BEoLjG648BHjYfWfAtOTPcxU= oldest@oldest-Lenovo-ideapad-320-15IKB
```
**Апдейт ключа на сайте IoT-Lab**
<img width="1385" alt="image" src="https://user-images.githubusercontent.com/101215070/159986175-b76b3cc7-e637-4d0e-85da-d3a7a03a4e33.png">
**Проверка доступа**
Даллее нужно подключиться к внешнему интерфейсу SSH, чтобы использовать платы под управлением встроенной ОС Linux.  Мы использовали интерфейс Гренобля для проверки подключения. 
```
$ ssh sannikov@grenoble.iot-lab.info
```
**Вывод корректного подключения**
Нужно было потвердить подключение и ввести пароль своего профиля.
```
The authenticity of host 'grenoble.iot-lab.info (194.199.16.167)' can't be established.
ECDSA key fingerprint is SHA256:k47g928kkptEpPsc00ztE4v8lml0Ypv9L+SDIFifh/s.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'grenoble.iot-lab.info,194.199.16.167' (ECDSA) to the list of known hosts.
Linux grenoble 4.19.0-13-amd64 #1 SMP Debian 4.19.160-2 (2020-11-28) x86_64
Welcome FIT IoT-LAB users

Charter:
* FIT IoT-LAB is shared among several users, so make reasonable use of the platform
* Quote FIT IoT-LAB in your scientific papers. Usage of FIT IoT-LAB is free of charge.
  In return, you must quote FIT IoT-LAB in your publication if your experiments results
  are based on FIT IoT-LAB testbed:

  1. Add acknowledgements to FIT IoT-LAB in introduction or conclusion of the publication
  2. Add citation to the reference article of FIT IoT-LAB. See details here:
     https://www.iot-lab.info/charter/
  3. Send email to admin@iot-lab.info once your publication has been accepted in order
     to update hall of fame:
     https://www.iot-lab.info/publications/

Post your issues on:
* the user mailing-list: users@iot-lab.info
* or the bug-tracker: https://github.com/iot-lab/iot-lab/issues
```
**Сохраняем свои учетные данные**

```
iotlab-auth -u sannikov
```

## Проведение первого эксперемента "Достать с Githuba и скомпилировать прошивки для узлов M3 и A8-M3"
 Цель данного эксперемента - научиться использовать настроенную среду, скомпилировать прошивку и использовать и использовать Contiki (операционную систему, разработанную с учетом особых требований сценариев Интернета вещей (IoT).) с платой  M3 и A8-M3. 
 
Для этих узлов Contiki нужен инструментарий arm-none-eabi-gcc. Сайт внешних SSH-серверов предоставляет эту цепочку инструментов, поэтому мы можем ее использовать.

**Клонируем требуемый репрозирторий для нашей работы**
В данной части мы рассмотрим скрипт вывода значений различных датчиков со временем
```
sannikov@grenoble:~$ git clone https://github.com/iot-lab/iot-lab.git

 Клонирование в «iot-lab». .. 
... 
sannikov@grenoble:~$ cd iot-lab 
sannikov@grenoble:~$ make
```
**Вывод**
```
Welcome to the IoT-LAB development environment setup.

targets:
	setup-aggregation-tools
	setup-cli-tools
	setup-contiki
	setup-iot-lab-contiki-ng
	setup-iot-lab.wiki
	setup-oml-plot-tools
	setup-openlab
	setup-riot
	setup-wsn430
	setup-zephyr

	pull
  ```
  **Установка Contiki**

```
sannikov@grenoble:~/iot-lab$ make setup-contiki
```
  **Вывод**
```
Welcome to Contiki for IoT-LAB !
================================

This repository is a fork of the official Contiki repository, bringing support for the IoT-LAB platforms.

You may retrieve the last changes from the official repository with these commands:

    git remote add contiki https://github.com/contiki-os/contiki
    git fetch contiki
    git merge contiki/master

Supported platforms:
- iotlab-m3
- iotlab-a8-m3

Requirements:
- gcc-toolchain: https://launchpad.net/gcc-arm-embedded
- openlab (already checked-out if you used iot-lab)

See this [tutorial](https://www.iot-lab.info/tutorials/contiki-compilation/) for explanations on how to setup your environment.

Basic setup:
- ``$ make TARGET=iotlab-m3     savetarget ``  # for m3 nodes
- ``$ make TARGET=iotlab-a8-m3  savetarget ``  # for a8 nodes

Further doc:
- README-BUILDING.md
- README-EXAMPLES.md
```
 **Путь к нужной папке с описанием доступных датчиков**
 ```
 sannikov@grenoble:~/iot-lab$ cd parts/contiki
 ```
  ```
sannikov@grenoble:~/iot-lab/parts/contiki$ cd examples/iotlab/03-sensors-collecting/
 ```
 **Отобразить файл с результатами показаний датчиков**
 ```
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ cat README.md
 ```
   **Пример вывода описания доступных датчиков для эксперемента на узле M3 c выводом значений**
  
  ```
IoT-LAB Sensors Collecting
==========================

Prints all the available sensors once every second

Node M3
-------

    gyros: -8 -490 96 xyz m°/s
    light: 2.5552368E2 lux
    press: 9.8917236E2 mbar
    accel: -66 0 1080 xyz mg
    magne: -11 143 -438 xyz mgauss
    gyros: 1338 -385 -78 xyz m°/s
    light: 2.5463867E2 lux
    press: 9.891687E2 mbar
    magne: -15 148 -433 xyz mgauss
    accel: -61 -3 1070 xyz mg
    gyros: 840 -577 -367 xyz m°/s
    light: 2.542572E2 lux
    press: 9.891116E2 mbar
    magne: -9 150 -436 xyz mgauss
    gyros: 358 -332 -376 xyz m°/s
    accel: -60 -1 1076 xyz mg
    light: 2.538147E2 lux
    press: 9.8897876E2 mbar
    magne: -10 149 -435 xyz mgauss
    gyros: 105 236 -288 xyz m°/s
    light: 2.5352478E2 lux
    press: 9.890857E2 mbar
    accel: -62 -2 1072 xyz mg
    magne: -10 146 -439 xyz mgauss
    gyros: 778 -1015 201 xyz m°/s
    light: 2.565155E2 lux
    press: 9.8914575E2 mbar
    accel: -62 -3 1069 xyz mg
    magne: -9 148 -430 xyz mgauss
    gyros: 437 35 -87 xyz m°/s
    light: 2.4443054E2 lux
  ```
  **Вывод кода скрпта**
   ```
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ cat sensors-collecting.c
  ```
 **Скрипт**
   ```
#include "contiki.h"
#include <stdio.h>

#ifdef IOTLAB_M3
#include "dev/light-sensor.h"
#include "dev/pressure-sensor.h"
#endif
#include "dev/acc-mag-sensor.h"
#include "dev/gyr-sensor.h"

#include "dev/leds.h"

/*
 * Print the value of each available sensors once every second.
 */


PROCESS(sensor_collection, "Sensors collection");
AUTOSTART_PROCESSES(&sensor_collection);

#ifdef IOTLAB_M3

/* Light sensor */
static void config_light()
{
  light_sensor.configure(LIGHT_SENSOR_SOURCE, ISL29020_LIGHT__AMBIENT);
  light_sensor.configure(LIGHT_SENSOR_RESOLUTION, ISL29020_RESOLUTION__16bit);
  light_sensor.configure(LIGHT_SENSOR_RANGE, ISL29020_RANGE__1000lux);
  SENSORS_ACTIVATE(light_sensor);
}
static void process_light()
{
  int light_val = light_sensor.value(0);
  float light = ((float)light_val) / LIGHT_SENSOR_VALUE_SCALE;
  printf("light: %f lux\n", light);
}

/* Pressure */
static void config_pressure()
{
  pressure_sensor.configure(PRESSURE_SENSOR_DATARATE, LPS331AP_P_12_5HZ_T_1HZ);
  SENSORS_ACTIVATE(pressure_sensor);
}

static void process_pressure()
{
  int pressure;
  pressure = pressure_sensor.value(0);
  printf("press: %f mbar\n", (float)pressure / PRESSURE_SENSOR_VALUE_SCALE);
}


#endif

/* Accelerometer / magnetometer */
static unsigned acc_freq = 0;
static void config_acc()
{
  acc_sensor.configure(ACC_MAG_SENSOR_DATARATE,
      LSM303DLHC_ACC_RATE_1344HZ_N_5376HZ_LP);
  acc_freq = 1344;
  acc_sensor.configure(ACC_MAG_SENSOR_SCALE, LSM303DLHC_ACC_SCALE_2G);
  acc_sensor.configure(ACC_MAG_SENSOR_MODE, LSM303DLHC_ACC_UPDATE_ON_READ);
  SENSORS_ACTIVATE(acc_sensor);
}

static void process_acc()
{
  int xyz[3];
  static unsigned count = 0;
  if ((++count % acc_freq) == 0) {
    xyz[0] = acc_sensor.value(ACC_MAG_SENSOR_X);
    xyz[1] = acc_sensor.value(ACC_MAG_SENSOR_Y);
    xyz[2] = acc_sensor.value(ACC_MAG_SENSOR_Z);

    printf("accel: %d %d %d xyz mg\n", xyz[0], xyz[1], xyz[2]);
  }
}

static unsigned mag_freq = 0;
static void config_mag()
{
  mag_sensor.configure(ACC_MAG_SENSOR_DATARATE, LSM303DLHC_MAG_RATE_220HZ);
  mag_freq = 220;
  mag_sensor.configure(ACC_MAG_SENSOR_SCALE, LSM303DLHC_MAG_SCALE_1_3GAUSS);
  mag_sensor.configure(ACC_MAG_SENSOR_MODE, LSM303DLHC_MAG_MODE_CONTINUOUS);
  SENSORS_ACTIVATE(mag_sensor);
}

static void process_mag()
{
  int xyz[3];
  static unsigned count = 0;
  if ((++count % mag_freq) == 0) {
    xyz[0] = mag_sensor.value(ACC_MAG_SENSOR_X);
    xyz[1] = mag_sensor.value(ACC_MAG_SENSOR_Y);
    xyz[2] = mag_sensor.value(ACC_MAG_SENSOR_Z);

    printf("magne: %d %d %d xyz mgauss\n", xyz[0], xyz[1], xyz[2]);
  }
}

/* Gyroscope */
static unsigned gyr_freq = 0;
static void config_gyr()
{
  gyr_sensor.configure(GYR_SENSOR_DATARATE, L3G4200D_800HZ);
  gyr_freq = 800;
  gyr_sensor.configure(GYR_SENSOR_SCALE, L3G4200D_250DPS);
  SENSORS_ACTIVATE(gyr_sensor);
}

static void process_gyr()
{
  int xyz[3];
  static unsigned count = 0;
  if ((++count % gyr_freq) == 0) {
    xyz[0] = gyr_sensor.value(GYR_SENSOR_X);
    xyz[1] = gyr_sensor.value(GYR_SENSOR_Y);
    xyz[2] = gyr_sensor.value(GYR_SENSOR_Z);
    printf("gyros: %d %d %d xyz m°/s\n", xyz[0], xyz[1], xyz[2]);
  }
}

/*---------------------------------------------------------------------------*/
PROCESS_THREAD(sensor_collection, ev, data)
{
  PROCESS_BEGIN();
  static struct etimer timer;

#ifdef IOTLAB_M3
  config_light();
  config_pressure();
#endif

  config_acc();
  config_mag();
  config_gyr();

  etimer_set(&timer, CLOCK_SECOND);

  while(1) {
    PROCESS_WAIT_EVENT();
    if (ev == PROCESS_EVENT_TIMER) {
#ifdef IOTLAB_M3
      process_light();
      process_pressure();
#endif

      etimer_restart(&timer);
    } else if (ev == sensors_event && data == &acc_sensor) {
      process_acc();
    } else if (ev == sensors_event && data == &mag_sensor) {
      process_mag();
    } else if (ev == sensors_event && data == &gyr_sensor) {
      process_gyr();
    }
  }

  PROCESS_END();
}
/*---------------------------------------------------------------------------*/
  ```
  **Отправка кода на узел M3**
  Компилируем код работы датчками на платы, находящиеся на сервере 
   ```
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ make TARGET=iotlab-m3
  ```
   **Вывод компиляции на плату M3**
  ```  
mkdir -p obj_iotlab-m3/cortex-m3/
mkdir -p obj_iotlab-m3/iotlab-m3/
mkdir -p obj_iotlab-m3/isl29020/
mkdir -p obj_iotlab-m3/l3g4200d/
mkdir -p obj_iotlab-m3/lps331ap/
mkdir -p obj_iotlab-m3/lsm303dlhc/
mkdir -p obj_iotlab-m3/n25xxx/
mkdir -p obj_iotlab-m3/rf2xx/
mkdir -p obj_iotlab-m3/softtimer/
mkdir -p obj_iotlab-m3/stm32/
mkdir -p obj_iotlab-m3/stm32f1xx/
 ```
 **Отправка кода на узел M3-A8**
  Компилируем код работы датчками на платы, находящиеся на сервере 
 ```
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ make TARGET=iotlab-a8-m3
 ```
   **Вывод компиляции на плату M3-A8**
  ```  
mkdir -p obj_iotlab-a8-m3/cortex-m3/
mkdir -p obj_iotlab-a8-m3/iotlab-a8-m3/
mkdir -p obj_iotlab-a8-m3/l3g4200d/
mkdir -p obj_iotlab-a8-m3/lsm303dlhc/
mkdir -p obj_iotlab-a8-m3/rf2xx/
mkdir -p obj_iotlab-a8-m3/softtimer/
mkdir -p obj_iotlab-a8-m3/stm32/
mkdir -p obj_iotlab-a8-m3/stm32f1xx/
 ```
   **Проверка успешной кампиляции**
 ```  
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ ls
 ```
**Успешный успех!**
 ```  
contiki-iotlab-a8-m3.a  contiki-iotlab-m3.a  Makefile  obj_iotlab-a8-m3  obj_iotlab-m3  README.md  sensors-collecting.c  sensors-collecting.iotlab-a8-m3  sensors-collecting.iotlab-m3
 ```
 **Отправка эксперемента**
  ```
 sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ iotlab-experiment submit -n contiki -d 15 -l 1,archi=m3:at86rf231+site=grenoble+mobile=0,sensors-collecting.iotlab-m3
  ```
   **Эксперемент запущен!**
     
 ```
{
    "id": 306550
}
 ```
<img width="1281" alt="image" src="https://user-images.githubusercontent.com/101215070/159998929-a77f528f-bc0b-4a4f-a62b-12bfa254a239.png">


**Подключение к выходу запущенного узла и считывания данных датчика во времени**

 ```
sannikov@grenoble:~/iot-lab/parts/contiki/examples/iotlab/03-sensors-collecting$ ssh sannikov@grenoble.iot-lab.info
 ``` 
 В данном эксперементе использовался нод с номером 99
 
``` 
sannikov@grenoble:~$ nc m3-99 20000
 ``` 
 **Показания датчиков во времени**
 
``` 
gyros: 140 -892 61 xyz m°/s
magne: 50 283 -612 xyz mgauss
light: 0.061035 lux
press: 1003.351562 mbar
accel: -140 -7 -1010 xyz mg
gyros: 1330 -17 -306 xyz m°/s
magne: 47 281 -610 xyz mgauss
light: 0.061035 lux
press: 1003.365967 mbar
accel: -141 -10 -1002 xyz mg
gyros: 1365 262 -140 xyz m°/s
magne: 49 284 -615 xyz mgauss
light: 0.061035 lux
press: 1003.151611 mbar
accel: -139 -9 -999 xyz mg
magne: 45 283 -612 xyz mgauss
gyros: 1093 -297 -140 xyz m°/s
light: 0.061035 lux
press: 1003.330078 mbar
accel: -137 -10 -1004 xyz mg
magne: 48 285 -613 xyz mgauss
gyros: 1706 -411 78 xyz m°/s
light: 0.061035 lux
press: 1003.377930 mbar
accel: -139 -9 -1005 xyz mg
magne: 50 282 -609 xyz mgauss
light: 0.061035 lux
press: 1003.397217 mbar
gyros: 980 -70 288 xyz m°/s
magne: 50 283 -605 xyz mgauss
accel: -139 -12 -1004 xyz mg
light: 0.061035 lux
press: 1003.395996 mbar
gyros: 1723 -8 -393 xyz m°/s
magne: 47 280 -612 xyz mgauss
light: 0.061035 lux
press: 1003.291016 mbar
accel: -139 -11 -1006 xyz mg
gyros: 1198 -525 -148 xyz m°/s
magne: 46 281 -614 xyz mgauss
light: 0.061035 lux
press: 1003.445557 mbar
gyros: 1128 -918 52 xyz m°/s
accel: -142 -6 -1001 xyz mg
magne: 50 283 -609 xyz mgauss
light: 0.061035 lux
press: 1003.422852 mbar
gyros: 481 -857 227 xyz m°/s
accel: -138 -7 -1005 xyz mg
magne: 49 282 -612 xyz mgauss
light: 0.061035 lux
press: 1003.431396 mbar
gyros: 1128 227 717 xyz m°/s
accel: -136 -11 -1002 xyz mg
magne: 49 282 -615 xyz mgauss
light: 0.061035 lux
press: 1003.359863 mbar
gyros: 1408 -770 -297 xyz m°/s
magne: 50 284 -612 xyz mgauss
accel: -140 -10 -1005 xyz mg
light: 0.061035 lux
press: 1003.361572 mbar
gyros: 1601 0 -61 xyz m°/s
magne: 50 283 -612 xyz mgauss
accel: -135 -7 -1009 xyz mg
light: 0.061035 lux
press: 1003.430908 mbar
gyros: 376 -717 288 xyz m°/s
magne: 46 288 -613 xyz mgauss
light: 0.061035 lux
press: 1003.412842 mbar
accel: -141 -6 -1006 xyz mg
gyros: 708 105 437 xyz m°/s
magne: 50 283 -610 xyz mgauss
light: 0.061035 lux
press: 1003.351318 mbar
accel: -138 -10 -1008 xyz mg
magne: 47 282 -611 xyz mgauss
gyros: 1111 -236 -113 xyz m°/s
 ``` 
 <img width="476" alt="image" src="https://user-images.githubusercontent.com/101215070/159999330-3c2cba6b-e4a6-4e41-b0ce-861afb4dc129.png">

 **Вывод о эксперементе №1**
 В данной работе мы изучили работу в настройенной среде, взаимодейсвие с виртуальными узлами M3 и A8, метод их компеляции  и как запускать эксперемент на веб-сервере через терминал 
 ## Проведение эксперемента №2
Целью данной работы является создание профиля для мониторинга радиоактивности во время эксперимента, когда 2 узла обмениваются данными. Так как каждый узел эксперимента управляется своим узлом управления (недоступным для экспериментатора), который взаимодействует пассивно или активно. Он отслеживает потребление узла, мощность радиосигнала (RSSI) и выбирает источник питания (аккумулятор или постоянный ток с Poe). Сам профиль представляет собой конфигурацию узла управления во время эксперимента.

 **Изучение прошивки и установка радиоканала**
 
 На данном этаме нужно было изучить файлы прошивки и задать номер канала. В данной работе мы использовали для 1 радиоканала - 11 канал. 
  ``` 
sannikov@grenoble:~/iot-lab/parts/openlab$ cd appli/iotlab_examples/tutorial 
sannikov@grenoble:~/iot-lab/parts/openlab/appli/iotlab_examples/tutorial$ cat README.md
sannikov@grenoble:~/iot-lab/parts/openlab/appli/iotlab_examples/tutorial$ less main.c
``` 
**Компиляция прошивки**
Скомпилировали обучающую прошивку в директорию build.m3/ (сгенерируйте бинарные файлы *.elf с поддержкой радиочипсета at86rf231)
  ``` 
sannikov@grenoble:~/iot-lab/parts/openlab/build.m3$ make tutorial_m3 
sannikov@grenoble:~/iot-lab/parts/openlab/build.m3$ ls bin/tutorial_m3 .elf 
bin/tutorial_m3.elf
``` 
**Скачивание файла на компьютер**

Для этого мы открыли новый терминал
``` 
oldest@oldest-Lenovo-ideapad-320-15IKB:~$ scp sannikov@grenoble:~/iot-lab/parts/openlab/build.m3/bin/tutorial_m3.elf tutorial_m3.elf 
tutorial_m3.elf 100% 99 КБ 98,9 КБ/с 00:00 
oldest@oldest-Lenovo-ideapad-320-15IKB:~$ ls tutorial_m3.elf  
tutorial_m3.elf
``` 
tutorial_m3.elf - требуемый файл

**Производим необходимые настройки на портале**

<img width="1439" alt="image" src="https://user-images.githubusercontent.com/101215070/160003741-1201634f-d086-4268-af81-514aac85b5bb.png">

**Запсукаем наш эксперемент из профиля**
В настройках эксперемнта мы выбираем нужные узлы, сайт и количество узлов. А также добавляем наш файл с прошивкой и интегрируем нужные настройки созданные ранее

<img width="400" alt="image" src="https://user-images.githubusercontent.com/101215070/160004660-2f1f9667-f660-4546-94f3-9116c2b3fbf3.png">


<img width="1260" alt="image" src="https://user-images.githubusercontent.com/101215070/160004628-e9351a6b-de11-4b1a-bb2a-14963cca6cfe.png">
Ждем старта эксперемента 

**Подключаемся к внешнему сайту SSH с помощью X11Forwarding**

``` 
ssh -X sannikov@grenoble.iot-lab.info
``` 
**Подключаемся  последовательному порту первого узла для взаимодействия**
В нашем случае это 100 узел
``` 
sannikov@grenoble:~$ nc m3-100 20000
``` 
**Вывод подключения**
``` 
IoT-LAB Simple Demo program
 Type command
	h:	print this help
	t:	temperature measure
        l:	luminosity measure
        p:      pressure measure
	s:	send a radio packet
        b:      send a big radio packet
        e:      toogle leds blinking

 Type Enter to stop printing this help
 cmd >
 ``` 
**Открываем второй терминал**
Также подключаемся к внешнему интерфейсу SSH, а оттуда подключаемся к последовательныому порту другого узла - 101.
 ``` 
ssh sannikov@grenoble.iot-lab.info 
sannikov@grenoble:~$ nc m3-101 20000
 ``` 
 
 **Отправка  радиопакетов**
 На первом терминале мы отправляем радиопакеты, второй терминал их получает 
 <img width="935" alt="image" src="https://user-images.githubusercontent.com/101215070/160006094-94e2312d-0612-4620-a61d-d157e2d61cdf.png">
  **Вывод собранных данных**
  
 Измеренные значения RSSI можно хранить домашней папке с файлами oml.
   ``` 
sannikov@grenoble:~$ меньше ~/306845/radio/m3-100.oml 
 ``` 

 **Вывод о эксперементе №2**
 В данном эксперемнте мы создали область для мониторинга отправка и принятие данных от узлов. 
 
  ## Проведение эксперемента №3
  


В этом упражнении мы узнаем о протоколе MQTT* и его ограниченном варианте, называемом MQTT-SN, с использованием ОС RIOT на трех узлах A8-M3 в FIT/IoT-LAB. Сообщения MQTT, опубликованные в  хоста внешнего интерфейса, будут отображаться клиентом MQTT-SN, работающим на узле ОС RIOT. Узлы эксперимента будут иметь следующие функции:
- Первый узел будет использоваться в качестве граничного маршрутизатора для распространения префикса IPv6 через его беспроводной интерфейс.
- Второй узел будет использоваться как MQTT-брокер с помощью приложения mosquitto.rsmb .
- На третьем узле будет запущен клиент MQTT-SN для подключения к брокеру.

*MQTT (MQ Telemetry Transport) — это протокол подключения IoT между машинами (M2M), разработанный как чрезвычайно легкий протокол публикации/подписки для передачи сообщений между устройствами.


 **Подготовка**

Подключитесь к внешнему интерфейсу SSH сайта Saclay FIT/IoT-LAB
 ``` 
oldest@oldest-Lenovo-ideapad-320-15IKB:~/Desktop$ ssh sannikov@saclay.iot-lab.info
Linux saclay 4.19.0-13-amd64 #1 SMP Debian 4.19.160-2 (2020-11-28) x86_64
Welcome FIT IoT-LAB users

Charter:
* FIT IoT-LAB is shared among several users, so make reasonable use of the platform
* Quote FIT IoT-LAB in your scientific papers. Usage of FIT IoT-LAB is free of charge.
  In return, you must quote FIT IoT-LAB in your publication if your experiments results
  are based on FIT IoT-LAB testbed:

  1. Add acknowledgements to FIT IoT-LAB in introduction or conclusion of the publication
  2. Add citation to the reference article of FIT IoT-LAB. See details here:
     https://www.iot-lab.info/charter/
  3. Send email to admin@iot-lab.info once your publication has been accepted in order
     to update hall of fame:
     https://www.iot-lab.info/publications/

Post your issues on:
* the user mailing-list: users@iot-lab.info
* or the bug-tracker: https://github.com/iot-lab/iot-lab/issues
Last login: Wed Mar 23 22:09:01 2022 from 192.168.5.254
 ``` 
 **Аутенфикация и отправка эксперемент**

Пройдите аутентификацию на испытательном стенде и отправьте 60-минутный эксперимент с использованием трех узлов типа A8-M3 (прошивка узла будет загружена позже), затем дождитесь начала эксперимента

 ``` 
sannikov@saclay:~$ iotlab-auth -u sannikov
Password: 
"Written"
sannikov@saclay:~$ iotlab-experiment submit -n riot_mqtt -d 60 -l 3,archi=a8:at86rf231+site=saclay
{
    "id": 306864
}
sannikov@saclay:~$ iotlab-experiment wait
Waiting that experiment 306864 gets in state Running
"Running"
 ``` 
<img width="1263" alt="image" src="https://user-images.githubusercontent.com/101215070/160010176-fe5a4387-543b-4c04-ae9f-f39396caea00.png">

 **Использование индефекаторов**

Запишим отображаемый идентификатор эксперимента, а затем получим идентификаторы выделенных узлов после запуска эксперимента, используя приведенные ниже команды для получения дополнительной информации об эксперименте:
 ``` 
sannikov@saclay:~$ iotlab-experiment get -i 306864 -s
sys:1: DeprecationWarning: exp-state command is deprecated and will be removed in next release. Please use print instead.


{
    "state": "Running"
}

 ``` 
Ниже вы увидите информацию о трех запущенных узнал

 ``` 
sannikov@saclay:~$ iotlab-experiment get -i 306864 -r
sys:1: DeprecationWarning: resources command is deprecated and will be removed in next release. Please use nodes instead.


{
    "items": [
        {
            "archi": "a8:at86rf231",
            "camera": "0",
            "mobile": "0",
            "mobility_type": " ",
            "network_address": "a8-101.saclay.iot-lab.info",
            "power_consumption": "1",
            "power_control": "1",
            "production": "YES",
            "radio_sniffing": "1",
            "site": "saclay",
            "state": "Alive",
            "uid": "9964",
            "x": "11.5",
            "y": "45",
            "z": "2.5"
        },
        {
            "archi": "a8:at86rf231",
            "camera": "0",
            "mobile": "0",
            "mobility_type": " ",
            "network_address": "a8-102.saclay.iot-lab.info",
            "power_consumption": "1",
            "power_control": "1",
            "production": "YES",
            "radio_sniffing": "1",
            "site": "saclay",
            "state": "Alive",
            "uid": "b866",
            "x": "15.5",
            "y": "45",
            "z": "2.5"
        },
        {
            "archi": "a8:at86rf231",
            "camera": "0",
            "mobile": "0",
            "mobility_type": " ",
            "network_address": "a8-100.saclay.iot-lab.info",
            "power_consumption": "1",
            "power_control": "1",
            "production": "YES",
            "radio_sniffing": "1",
            "site": "saclay",
            "state": "Alive",
            "uid": "9458",
            "x": "8.5",
            "y": "45",
            "z": "2.5"
        }
    ]
}

 ``` 

**Настройка прошивки первого узнал**

Первый узел ( a8-1в нашем примере) будет действовать как граничный маршрутизатор с использованием gnrc_border_routerкода. Для прошивки нужно клонировать репозиторий ОС RIOT с GitHub:

 ``` 
sannikov@saclay:~$ git clone https://github.com/RIOT-OS/RIOT.git
Cloning into 'RIOT'...
remote: Enumerating objects: 304556, done.
remote: Total 304556 (delta 0), reused 0 (delta 0), pack-reused 304556
Receiving objects: 100% (304556/304556), 115.10 MiB | 17.84 MiB/s, done.
Resolving deltas: 100% (203569/203569), done.
Checking out files: 100% (14099/14099), done.

 ``` 
Долее собрать  прошивку "gnrc_border_router " с соответствующей скоростью передачи данных для узлов A8-M3, которая составляет 500 000

 ``` 
sannikov@saclay:~$ source /opt/riot.source
sannikov@saclay:~$ cd RIOT
sannikov@saclay:~/RIOT$ make ETHOS_BAUDRATE=500000 BOARD=iotlab-a8-m3 clean all -C examples/gnrc_border_router
 ``` 
**Вывод**

 ``` 
make: Entering directory '/senslab/users/sannikov/RIOT/examples/gnrc_border_router'
Building application "gnrc_border_router" for "iotlab-a8-m3" with MCU "stm32".

[INFO] cloning stm32cmsis
Cloning into '/senslab/users/sannikov/RIOT/build/stm32/cmsis/f1'...
remote: Enumerating objects: 193, done.
remote: Counting objects: 100% (193/193), done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 193 (delta 140), reused 189 (delta 136), pack-reused 0
Receiving objects: 100% (193/193), 269.09 KiB | 4.64 MiB/s, done.
Resolving deltas: 100% (140/140), done.
HEAD is now at 71ad5b3 Release v4.3.3
[INFO] updating stm32cmsis /senslab/users/sannikov/RIOT/build/stm32/cmsis/f1/.pkg-state.git-downloaded
echo 71ad5b3bf5cbb4d35cf8c8726c1b343871f0df0a   > /senslab/users/sannikov/RIOT/build/stm32/cmsis/f1/.pkg-state.git-downloaded
[INFO] patch stm32cmsis
Applying: stm32f1xx: remove ErrorStatus
"make" -C /senslab/users/sannikov/RIOT/boards/common/init
"make" -C /senslab/users/sannikov/RIOT/boards/iotlab-a8-m3
"make" -C /senslab/users/sannikov/RIOT/boards/common/iotlab
"make" -C /senslab/users/sannikov/RIOT/core
"make" -C /senslab/users/sannikov/RIOT/core/lib
"make" -C /senslab/users/sannikov/RIOT/cpu/stm32
"make" -C /senslab/users/sannikov/RIOT/cpu/cortexm_common
"make" -C /senslab/users/sannikov/RIOT/cpu/cortexm_common/periph
"make" -C /senslab/users/sannikov/RIOT/cpu/stm32/periph
"make" -C /senslab/users/sannikov/RIOT/cpu/stm32/stmclk
"make" -C /senslab/users/sannikov/RIOT/cpu/stm32/vectors
"make" -C /senslab/users/sannikov/RIOT/drivers
"make" -C /senslab/users/sannikov/RIOT/drivers/at86rf2xx
"make" -C /senslab/users/sannikov/RIOT/drivers/ethos
"make" -C /senslab/users/sannikov/RIOT/drivers/netdev
"make" -C /senslab/users/sannikov/RIOT/drivers/periph_common
"make" -C /senslab/users/sannikov/RIOT/sys
"make" -C /senslab/users/sannikov/RIOT/sys/auto_init
"make" -C /senslab/users/sannikov/RIOT/sys/div
"make" -C /senslab/users/sannikov/RIOT/sys/evtimer
"make" -C /senslab/users/sannikov/RIOT/sys/fmt
"make" -C /senslab/users/sannikov/RIOT/sys/frac
"make" -C /senslab/users/sannikov/RIOT/sys/iolist
"make" -C /senslab/users/sannikov/RIOT/sys/isrpipe
"make" -C /senslab/users/sannikov/RIOT/sys/luid
"make" -C /senslab/users/sannikov/RIOT/sys/malloc_thread_safe
"make" -C /senslab/users/sannikov/RIOT/sys/net/application_layer/uhcp
"make" -C /senslab/users/sannikov/RIOT/sys/net/crosslayer/inet_csum
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netapi
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netif
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netif/ethernet
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netif/hdr
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netif/ieee802154
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netif/init_devs
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/netreg
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/icmpv6
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/icmpv6/echo
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/ipv6
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/ipv6/hdr
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/ipv6/nib
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/ndp
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/ctx
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/frag
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/frag/fb
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/frag/rb
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/iphc
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/network_layer/sixlowpan/nd
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/pkt
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/pktbuf
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/pktbuf_static
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/sock
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/sock/udp
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/transport_layer/udp
"make" -C /senslab/users/sannikov/RIOT/sys/net/gnrc/application_layer/uhcpc
"make" -C /senslab/users/sannikov/RIOT/sys/net/link_layer/eui_provider
"make" -C /senslab/users/sannikov/RIOT/sys/net/link_layer/ieee802154
"make" -C /senslab/users/sannikov/RIOT/sys/net/link_layer/l2util
"make" -C /senslab/users/sannikov/RIOT/sys/net/netif
"make" -C /senslab/users/sannikov/RIOT/sys/net/netutils
"make" -C /senslab/users/sannikov/RIOT/sys/net/network_layer/icmpv6
"make" -C /senslab/users/sannikov/RIOT/sys/net/network_layer/ipv6/addr
"make" -C /senslab/users/sannikov/RIOT/sys/net/network_layer/ipv6/hdr
"make" -C /senslab/users/sannikov/RIOT/sys/net/network_layer/sixlowpan
"make" -C /senslab/users/sannikov/RIOT/sys/net/transport_layer/udp
"make" -C /senslab/users/sannikov/RIOT/sys/newlib_syscalls_default
"make" -C /senslab/users/sannikov/RIOT/sys/pm_layered
"make" -C /senslab/users/sannikov/RIOT/sys/posix/inet
"make" -C /senslab/users/sannikov/RIOT/sys/ps
"make" -C /senslab/users/sannikov/RIOT/sys/random
"make" -C /senslab/users/sannikov/RIOT/sys/shell
"make" -C /senslab/users/sannikov/RIOT/sys/shell/commands
"make" -C /senslab/users/sannikov/RIOT/sys/tsrb
"make" -C /senslab/users/sannikov/RIOT/sys/ztimer
"make" -C /senslab/users/sannikov/RIOT/sys/ztimer64
   text	   data	    bss	    dec	    hex	filename
  80808	    188	  22132	 103128	  192d8	/senslab/users/sannikov/RIOT/examples/gnrc_border_router/bin/iotlab-a8-m3/gnrc_border_router.elf
make: Leaving directory '/senslab/users/sannikov/RIOT/examples/gnrc_border_router'

 ``` 
Компилируем  и прошиваем скомпилированную прошивку на первый экспериментальный узел

```
sannikov@saclay:~/RIOT$ scp examples/gnrc_border_router/bin/iotlab-a8-m3/gnrc_border_router.elf root@node-a8-100:
gnrc_border_router.elf                        100% 3104KB   3.0MB/s   00:01    
sannikov@saclay:~/RIOT$ ssh root@node-a8-100
root@node-a8-100:~# flash_a8_m3 gnrc_border_router.elf
Open On-Chip Debugger 0.10.0+dev-01293-g7c88e76a7-dirty (2021-10-22-12:48)
Licensed under GNU GPL v2
For bug reports, read
	http://openocd.org/doc/doxygen/bugs.html
debug_level: 0

trst_and_srst separate srst_nogate trst_push_pull srst_open_drain connect_assert_srst

    TargetName         Type       Endian TapName            State       
--  ------------------ ---------- ------ ------------------ ------------
 0* stm32f1x.cpu       cortex_m   little stm32f1x.cpu       reset

target halted due to debug-request, current mode: Thread 
xPSR: 0x01000000 pc: 0x080017f4 msp: 0x20010000
target halted due to debug-request, current mode: Thread 
xPSR: 0x01000000 pc: 0x080017f4 msp: 0x20010000
auto erase enabled
wrote 81920 bytes from file /home/root/gnrc_border_router.elf in 3.561459s (22.463 KiB/s)

verified 80996 bytes in 1.249479s (63.305 KiB/s)

shutdown command invoked
flash OK
```
Прошивка сделана! 

**Настройка сетевых параметров пограничного маршрутизатора**

Используем первый узел для компиляции инструмента с именем uhcpd(Micro Host Configuration Protocol)
```
root@node-a8-100:~# cd ~/A8/riot/RIOT/dist/tools/uhcpd
root@node-a8-100:~/A8/riot/RIOT/dist/tools/uhcpd# make clean all
rm -f bin/uhcpd
cc -g -O3 -Wall -DUHCP_SERVER -I../../../sys/include -I. ../../../sys/net/application_layer/uhcp/uhcp.c uhcpd.c -o bin/uhcpd

```

Также скомпилируем инструменты с именем ethos(Ethernet Over Serial) и настроим общедоступную сеть IPv6 узла:

```
root@node-a8-100:~/A8/riot/RIOT/dist/tools/uhcpd# cd ../ethos
root@node-a8-100:~/A8/riot/RIOT/dist/tools/ethos# make clean all
rm -f ethos
cc -O3 -Wall ethos.c -o ethos
root@node-a8-100:~/A8/riot/RIOT/dist/tools/ethos# ./start_network.sh /dev/ttyA8_M3 tap0 2001:660:3207:401::/64 500000
```
Следующий вывод говорит о корректной работе

```
net.ipv6.conf.tap0.forwarding = 1
net.ipv6.conf.tap0.accept_ra = 0
----> ethos: sending hello.
----> ethos: activating serial pass through.
----> ethos: hello reply received
----> ethos: hello reply received
```

**Настройка и запуск брокера MQTT**

В другом терминале подключитесь ко второму узлу ( a8-101) 
Подключаемся к внешнему интерфейсу Saclay SSH, затем ко второму узлу

```
oldest@oldest-Lenovo-ideapad-320-15IKB:~/Desktop$ ssh sannikov@saclay.iot-lab.info

sannikov@saclay:~$ ssh root@node-a8-101
```
Создаем и редактируем  файл конфигурации config.conf.

```
root@node-a8-101:~# vim config.conf
```

Добавляем код конфигурации, который сделает брокера MQTT доступным с любого узла за пограничным маршрутизатором, используя MQTT-SN на порту 1885, и из внешнего интерфейса SSH, используя MQTT на порту 1886

```
# Enable debugging output
 trace_output protocol

 # Listen for MQTT-SN traffic on UDP port 1885
 listener 1885 INADDR_ANY mqtts
 ipv6 true

 # Listen to MQTT connections on TCP port 1886
 listener 1886 INADDR_ANY
 ipv6 true
```

Получаем глобальный IPv6-адрес этого узла, так как он понадобится в дальнейшем для подключения к нему на шаге 6:

```
root@node-a8-101:~# ip -6 -o addr show eth0
2: eth0    inet6 2001:660:3207:400::65/64 scope global \       valid_lft forever preferred_lft forever
2: eth0    inet6 fe80::fadc:7aff:fe01:9910/64 scope link \       valid_lft forever preferred_lft forever
root@node-a8-101:~# broker_mqtts config.conf
```
Запускаем брокера на втором узле

```
root@node-a8-2:~# broker_mqtts config.conf
```
**Полученный вывод в терминале брокера MQTT**
```
20220323 212730.730 CWNAN9999I Really Small Message Broker
20220323 212730.735 CWNAN9998I Part of Project Mosquitto in Eclipse
(http://projects.eclipse.org/projects/technology.mosquitto)
20220323 212730.737 CWNAN0049I Configuration file name is config.conf
20220323 212730.749 CWNAN0053I Version 1.3.0.2, Dec 20 2020 23:34:33
20220323 212730.751 CWNAN0054I Features included: bridge MQTTS 
20220323 212730.752 CWNAN9993I Authors: Ian Craggs (icraggs@uk.ibm.com), Nicholas O'Leary
20220323 212730.755 CWNAN0300I MQTT-S protocol starting, listening on port 1885
20220323 212730.757 CWNAN0014I MQTT protocol starting, listening on port 1886

```
**Настройка клиента MQTT-SN**

Для настройки клиента используем третий терминал a8-3

Войдемв интерфейс Saclay SSH, собираем и прошьем прошивку RIOT MQTT-SN:

```
oldest@oldest-Lenovo-ideapad-320-15IKB:~/Desktop$ ssh sannikov@saclay.iot-lab.info
```
Прошивка 

```
sannikov@saclay:~$ source /opt/riot.source
sannikov@saclay:~$ cd RIOT
sannikov@saclay:~/RIOT$ make BOARD=iotlab-a8-m3 -C examples/emcute_mqttsn
sannikov@saclay:~/RIOT/$ scp examples/emcute_mqttsn/bin/iotlab-a8-m3/emcute_mqttsn.elf root@node-a8-102:
sannikov@saclay:~/RIOT/$ ssh root@node-a8-102
root@node-a8-102:~# flash_a8_m3 emcute_mqttsn.elf
```

Далее, используем программу miniterm.pyдля доступа к интерфейсу оболочки третьего узла и вводим "help"

Как можно заметить данные после вывода двоятся, но считывание кода идет корректно

```
root@node-a8-102:~# miniterm.py /dev/ttyA8_M3 500000 -e
--- Miniterm on /dev/ttyA8_M3  500000,8,N,1 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
hheellpp
```


```
> hheellpp

Command              Description
---------------------------------------
con                  connect to MQTT broker
discon               disconnect from the current broker
pub                  publish something
sub                  subscribe topic
unsub                unsubscribe from topic
will                 register a last will
reboot               Reboot the node
version              Prints current RIOT_VERSION
pm                   interact with layered PM subsystem
ps                   Prints information about running threads.
ping6                Ping via ICMPv6
ping                 Alias for ping6
nib                  Configure neighbor information base
ifconfig             Configure network interfaces
6ctx                 6LoWPAN context configuration tool


```
Следующие команды не удалось реабилитировать и возникали ошибки, возможно это связано со скоростью интернета или скорости термина
```
 > con 2001:660:3207:400::66 1885
 > sub test/riot
```
ВЫВОД
```
> 
> con 2con 2001:660:3001:660:3207:400:207:400::66 188:66 18855

error: unable to connect to [2001:660:3207:400::66]:1885
> 
> ␛[␛[AA

shell: command not found: ␛[A
> 
> con 2001con 2001:660:320:660:3207:400::67:400::66 18856 1885

error: unable to connect to [2001:660:3207:400::66]:1885
> 
> con 20con 2001:660:301:660:3207:400:207:400::65 1885:65 1885

error: unable to connect to [2001:660:3207:400::65]:1885
> 
> con 2con 2001:660:001:660:3207:400:3207:400::65 1885:65 1885

error: unable to connect to [2001:660:3207:400::65]:1885
> 
> con con 2001:6602001:660:3207:40:3207:400::64 180::64 188585

error: unable to connect to [2001:660:3207:400::64]:1885
> 
> subsub test/r test/riotiot

error: unable to subscribe to test/riot
> 
> 
```
**Проверка механизма публикации**

Четвертый терминал планировался использоваться для проверки механизма публикации/подписки между MQTT-брокером и клиентскими узлами. Но из-за возникновения ошибки в предыдущем терминале, данный этап также не осуществим 
**Пример работы терминалов**
<img width="1026" alt="image" src="https://user-images.githubusercontent.com/101215070/160013310-4a6d9512-b108-406f-9ab6-5407d3e91d97.png">

Ссылка на видео: 

https://user-images.githubusercontent.com/101215070/160014007-036b26a0-3f22-4e51-a2b2-791ac9b0a431.mp4

