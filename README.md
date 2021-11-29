1.	На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
поместите его в автозагрузку,
предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.
ОТВЕТ:
Создан юнит-файл.
![image](https://user-images.githubusercontent.com/91490218/143919675-1f182d59-f772-48f9-a420-22cba0dd17b6.png)
node exporter:
![image](https://user-images.githubusercontent.com/91490218/143919990-f9b7bb27-1e7e-47c1-bcf6-b30620c05bad.png)

2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
ОТВЕТ:
![image](https://user-images.githubusercontent.com/91490218/143920027-3fe8d064-09a0-472e-a5e0-88b2e2b0c13a.png)
CPU:
HELP node_cpu_seconds_total Seconds the cpus spent in each mode.
# TYPE node_cpu_seconds_total counter
node_cpu_seconds_total{cpu="0",mode="idle"} 29010.6
node_cpu_seconds_total{cpu="0",mode="iowait"} 114.54
node_cpu_seconds_total{cpu="0",mode="irq"} 0
node_cpu_seconds_total{cpu="0",mode="nice"} 0.57
node_cpu_seconds_total{cpu="0",mode="softirq"} 7.53
node_cpu_seconds_total{cpu="0",mode="steal"} 0
node_cpu_seconds_total{cpu="0",mode="system"} 25.65
node_cpu_seconds_total{cpu="0",mode="user"} 44.54
Disk:
# HELP node_disk_read_bytes_total The total number of bytes read successfully.
# TYPE node_disk_read_bytes_total counter
node_disk_read_bytes_total{device="dm-0"} 6.14360064e+08
node_disk_read_bytes_total{device="dm-1"} 3.469312e+06
Memory:
# HELP node_memory_MemAvailable_bytes Memory information field MemAvailable_bytes.
# HELP node_memory_MemFree_bytes Memory information field MemFree_bytes.
# HELP node_memory_MemTotal_bytes Memory information field MemTotal_bytes.
Network:
# HELP node_network_receive_bytes_total Network device statistic receive_bytes.
# HELP node_network_transmit_bytes_total Network device statistic transmit_bytes

3.Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.
ОТВЕТ:
![image](https://user-images.githubusercontent.com/91490218/143920641-b0348baf-4149-4011-8730-736b5ca8cfde.png)

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
ОТВЕТ: похоже ОС понимает, что крутится на системе виртуализации.
![image](https://user-images.githubusercontent.com/91490218/143921237-81bd1967-0cc0-4fa9-a0e9-96e79dd4638b.png)

5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?
ОТВЕТ:
Это максимальное число открытых дескрипторов для ядра (системы).
vagrant@vagrant:~$ /sbin/sysctl -n fs.nr_open
1048576
ulimit -n        the maximum number of open file descriptors

6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
ОТВЕТ:
Набираю sudo –i, затем screen, unshare -f --pid --mount-proc /bin/bash. После этого sleep 1h – и все…засыпает.
Дальнейшая последовательность команд - не разобрался.

7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?
ОТВЕТ: Логическая бомба. Этот Bash код создаёт функцию, которая запускает ещё два своих экземпляра, которые, в свою очередь снова запускают эту функцию и так до тех пор, пока этот процесс не займёт всю физическую память компьютера, и он просто не зависнет.
![image](https://user-images.githubusercontent.com/91490218/143922771-dc0c42f6-949a-402e-83a5-1294d4af40d2.png)
По умолчанию механизм имеет ограничение на создаваемые ресурсы  - 1000.
Установить ограничение числа процессов: ulimit -u .










 







































