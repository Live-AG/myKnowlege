


####	[Git + 1С. Часть 1. Как подключиться к команде разработки и начать использовать Git](https://infostart.ru/1c/articles/864097/)
https://infostart.ru/1c/articles/1157400/

### Настройка Gitsync
1.	Установить и настроить [[GIT]]
2.	Устанавливаем [[OneScript]]
3.	Установить GitSync: `opm install gitsync`
4.	`gitsync plugins init` - инициализировать плагины
5.	`gitsync  plugins enable -a` - включить все плагины или нужные
6.	`gitsync plugins d edtExport` - Отключить плагин который добавляет возможность выгрузки в формате EDT. Важно: Для работы плагина необходимы установленные EDT и Ring.

#### [GitSync 3.0. Шпаргалка по использованию](https://infostart.ru/1c/articles/1157400/)

## Заметки
```
***************************************
***************** GIT *****************
***************************************

git clone url_your_project_1c /path/to/project1c/src

***************************************
*************** GITSYNС ***************
***************************************
#local
gitsync sync C:\Git\Storage\ek2017_upp_work C:\Git\Repository\ek2017_upp_work
gitsync sync C:\Git\Storage\ek2017_upp_work C:\Git\Repository\ek2017_upp_work_new

#server
gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync --limit 5 C:\Git\Storage\ek2017_upp_work C:\Git\Repository\ek2017_upp_work_new
```

---

```
gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync C:\Git\Storage\ConfStorage_REF_EK2017 C:\Git\Repository\ek2017_Refactoring

  

gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync --limit 5 C:\Git\Storage\ek2017_upp_work C:\Git\Repository\ek2017_upp_work_new

  

http://AGusarov:q92_2ztv4xgoucPu5m3s@10.78.77.67:8283/sius/ek2017_refactoring

  
  

gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync \\10.78.78.10\ref\ConfStorage_REF_EK2017 C:\Git\Repository\ek2017_Refactoring

  

gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync tcp://LNG-DMZ-RDS/ConfStorage_REF_EK2017 C:\Git\Repository\ek2017_Refactoring

tcp://LNG-DMZ-RDS/ConfStorage_REF_EK2017

  
  

gitsync --ibconnection /SLNG-SRV-1C-TEST:2541\upp_gitsync_test sync --storage-user Администратор --storage-pwd 1qaz@WSX tcp://10.78.78.10/ConfStorage_REF_EK2017 C:\Git\Repository\ek2017_Refactoring

```