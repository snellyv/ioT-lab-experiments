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
 В данном эксперементе использовался узел с номером 99
 ``` 
sannikov@grenoble:~$ nc m3-99 20000
 ``` 
 **Показания датчика во времени**
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
 **Вывод о эксперементе №1**
 В данной работе мы изучили работу в настройенной среде, взаимодейсвие с виртуальными узлами M3 и A8, метод их компеляции  и как запускать эксперемент на веб-сервере через терминал 
