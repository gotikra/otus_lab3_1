### Задание 1

#### Создаём пул pool0 на sdb
>zpool create pool0 sdb  
#### Создаём на пуле pool0 4 файловые системы:
>zfs create pool0/data1  
>zfs create pool0/data2  
>zfs create pool0/data3  
>zfs create pool0/data4  

#### Устанавливаем тип сжатия для созданных файловых систем:
>zfs set compression=lzjb pool0/data1  
>zfs set compression=gzip-9 pool0/data2  
>zfs set compression=zle pool0/data3  
>zfs set compression=lz4 pool0/data4  
#### Получение тестового файла
>wget -O War_and_Peace.txt http://www.gutenberg.org/ebooks/2600.txt.utf-8
#### Размещение полученного файла по файловым системам pool0

>cp War_and_Peace.txt /pool0/data1  
>cp War_and_Peace.txt /pool0/data2  
>cp War_and_Peace.txt /pool0/data3  
>cp War_and_Peace.txt /pool0/data4  

#### Размер файла
[vagrant@lvm ~]$ du -k /pool0/data{1..4}/War_and_Peace.txt
>1191	/pool0/data1/War_and_Peace.txt
>1184	/pool0/data2/War_and_Peace.txt
>1185	/pool0/data3/War_and_Peace.txt
>1185	/pool0/data4/War_and_Peace.txt



### Задание 2
#### Восстановление пула
wget --no-check-certificate -O file.tar.gz 'https://drive.google.com/u/0/uc?id=1KRBNW33QWqbvbVHa3hLJivOAt60yukkg&export=download'  
>tar -xvf file.tar.gz  
>cd zpoolexport  
>mkdir /otus 
>sudo zpool import -d ./ otus  


#### Контрольная сумма
>[vagrant@lvm otus]$ zfs get checksum  
>NAME            PROPERTY  VALUE      SOURCE  
>otus            checksum  sha256     local  
>otus/hometask2  checksum  sha256     inherited from otus  


#### Размер recordsize
>[vagrant@lvm otus]$ zfs get recordsize /otus  
>NAME  PROPERTY    VALUE    SOURCE  
>otus  recordsize  128K     local  


#### Тип и степень сжатия
>[vagrant@lvm otus]$ zfs get compression,compressratio  
NAME            PROPERTY       VALUE     SOURCE  
>otus            compression    zle       local  
>otus            compressratio  1.00x     -  
>otus/hometask2  compression    zle       inherited from otus 
>otus/hometask2  compressratio  1.00x     -  


#### Информация о пуле
>[vagrant@lvm otus]$ zpool status -v otus  
>  pool: otus  
> state: ONLINE  
>  scan: none requested  
>config:  
>  
>	NAME                                 STATE     READ WRITE CKSUM  
>	otus                                 ONLINE       0     0     0  
>	  mirror-0                           ONLINE       0     0     0  
	    /home/vagrant/zpoolexport/filea  ONLINE       0     0     0  
>	    /home/vagrant/zpoolexport/fileb  ONLINE       0     0     0  
## Доступное место 
>vagrant@lvm otus]$ zpool list otus  
>NAME   SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT  
>otus   480M  2.10M   478M        -         -     0%     0%  1.00x    ONLINE  -  




### Задание 3

>wget --no-check-certificate -O otus_task2.file 'https://drive.google.com/u/0/uc?id=1gH8gCL9y7Nd5Ti3IRmplZPF1XjzxeRAG&export=download'  
>zfs receive otus/task2 < otus_task2.file  
>В файле secret_message ссылка https://github.com/sindresorhus/awesome  

















