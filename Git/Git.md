## Установка GIT

1. Скачать и установить [GIT](https://git-scm.com/downloads)


## Заметки

```
Git status
Git add -a
Git commit -a -m <message>
```

```
Использования кириллических наименований внешних обработок, длинных имён файлов и корректной работы с окончаниями строк:

git config --local core.quotepath false 
git config --local core.longpaths true 

git config user.name "Andrey Gusarov" 
git config user.email "a.gusarov@gnpholding.ru"
```


```
git remote add gitlab http://AGusarov:gFKrZ6G9LWJXAxQ2ubcu@localhost:9999/root/myObsidian

git remote set-url --add --push origin http://AGusarov:gFKrZ6G9LWJXAxQ2ubcu@localhost:9999/root/myObsidian

```

```
git remote add gitlab http://AGusarov:q92_2ztv4xgoucPu5m3s@10.78.77.67:8283/sius/ek2017_refactoring

git remote set-url --add --push origin http://AGusarov:q92_2ztv4xgoucPu5m3s@10.78.77.67:8283/sius/ek2017_refactoring

```

Token
	sius q92_2ztv4xgoucPu5m3s
	
Хранение параметров авторизации:

	git config --global credential.helper store
	
	
	git remote add gitlab http://AGusarov:q92_2ztv4xgoucPu5m3s@10.78.77.67:8283/sius/ek2017_ext_postref

git remote set-url --add --push gitlab http://AGusarov:q92_2ztv4xgoucPu5m3s@10.78.77.67:8283/sius/ek2017_ext_postref