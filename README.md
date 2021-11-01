1. Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
2. какой переменной можно задать длину журнала history, и на какой строчке manual это описывается
history-size (unset) -   Set the maximum number of history entries saved in the history list.  
 СТРОКА 1928

3. что делает директива ignoreboth в bash?
  Заменяет две команды: A value  of ignoreboth is shorthand for ignorespace and ignoredups.
  
4. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?
СТРОКА 794
Any  element of an array may be referenced using ${name[subscript]}.  The braces are required to avoid conflicts with pathname expansion.  


5. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
 НЕ РАЗОБРАЛСЯ. ВВОДИЛ КОМАНДУ touch {1...10000}, touch {[10000]}, touch {1...10000]} - не создает столько файлов.
 
6. Что делает конструкция [[ -d /tmp ]]
Проверяет существование директории:
[[ expression ]]
              Return  a status of 0 or 1 depending on the evaluation of the conditional expression expression.  

7. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:
bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash
НЕ РАЗОБРАЛСЯ: сделал команды: 
vagrant@vagrant:~$ mkdir /tmp/new_bash
vagrant@vagrant:~$ cp /bin/bash /tmp/new_bash/
vagrant@vagrant:~$ PATH=$PATH:/tmp/new_bash/
ПОЛУЧИЛОСЬ  ТАК: vagrant@vagrant:~$ type -a bash
bash is /usr/bin/bash
bash is /bin/bash
bash is /tmp/new_bash/bash

8. Чем отличается планирование команд с помощью batch и at?
at and batch read commands from standard input or a specified file which are to be executed at a later time, using /bin/sh.
at      executes commands at a specified time. Запускает команды в определенное время.
batch   executes commands when system load levels permit; in other words, when the load average drops below 1.5, or the value specified in the invocation of atd. Запускает команды когда позволяет уровень загруженности системы.

