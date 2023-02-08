# Домашнее задание к занятию "Работа в терминале. Лекция 1"

1. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас
   Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

```
vagrant@vagrant:~$ free -h
              total        used        free      shared  buff/cache   available
Mem:          976Mi       145Mi       110Mi       0.0Ki       721Mi       681Mi
Swap:         1.9Gi       1.0Mi       1.9Gi
vagrant@vagrant:~$ nproc
2
```

Выделено по умолчанию 978 mb RAM и 2 CPU

2. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: документация. Как добавить оперативной памяти или
   ресурсов процессора виртуальной машине?

Настройка осуществляется в файле Vagrantfile:


```
Vagrant.configure("2") do |config|
        config.vm.box = "bento/ubuntu-20.04"
        config.vm.provider :virtualbox do |v|
                v.memory = "4096"
                v.cpus = 4
        end
end
```

```
vagrant@vagrant:~$ free -h
              total        used        free      shared  buff/cache   available
Mem:          3.8Gi       147Mi       3.4Gi       0.0Ki       347Mi       3.5Gi
Swap:         1.9Gi          0B       1.9Gi
vagrant@vagrant:~$ nproc
4
vagrant@vagrant:~$ 
```
3. Ознакомьтесь с разделами man bash, почитайте о настройках самого bash: какой переменной можно задать длину журнала 
   history, и на какой строчке manual это описывается? что делает директива ignoreboth в bash?

HISTSIZE, начиная с 798 строчки man bash

Одновременно выполняет функционал директив ignorespace (не сохраняет команду, начинающуюся с пробела в истории команд) и ignoredups (не сохраняет строки, совпадающие с последней выполненной)

4. В каких сценариях использования применимы скобки {} и на какой строчке man bash это описано?

Описано, начиная с 230 строки man bash:
```
              list  is  simply executed in the current shell environment.  list must be terminated with a newline or semi-
              colon.  This is known as a group command.  The return status is the exit status of list.  Note  that  unlike
              the  metacharacters ( and ), { and } are reserved words and must occur where a reserved word is permitted to
              be recognized.  Since they do not cause a word break, they must be separated  from  list  by  whitespace  or
              another shell metacharacter.
```

Выделяет блок команд, выполняемых в текущей сессии терминала. Каждая строчка команд должны завершаться символом новой строки или точкой с запятой. Это групповая команда. В отличие от метасимволов ( и ), { и } - зарезервированные слова и должны встречаться там, где допустимо использование зарезервированных слов. Так как они не вызывают разрыв команды, они должны быть отделены от списка пробелом или другим метасимволом shell.

5. С учётом ответа на предыдущий вопрос, как создать однократным вызовом touch 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

Напимер вот так

```
touch file{0..100000}
```
Удалить 300000 файлов не получится. Выдает ошибку:
-bash: /usr/bin/touch: Argument list too long

6. В man bash поищите по /\[\[. Что делает конструкция [[ -d /tmp ]]
Проверит существование  /tmp и директория ли это, если да - то возратит 0

7. Сделайте так, чтобы в выводе команды type -a bash первым стояла запись с нестандартным путем, например bash is ... Используйте знания о просмотре существующих и создании новых переменных окружения, обратите внимание на переменную окружения PATH

bash is /tmp/new_path_directory/bash
bash is /usr/local/bin/bash
bash is /bin/bash

```
mkdir -p /tmp/new_path_directory/bash 
cp /usr/bin/bash /tmp/new_path_directory/bash/
PATH=/tmp/new_path_directory/bash:$PATH
```

8. Чем отличается планирование команд с помощью batch и at?
batch запускает команды, когда средняя системная нагрузка падает 1.5 или значения, указанного в atd.
at дает возможность запланировать запуск команд в определенный момент времени. Команды запускаются независимо от нагрузки системы.

