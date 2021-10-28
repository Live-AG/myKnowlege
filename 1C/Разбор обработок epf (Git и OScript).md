
#git #OScript #SonarQube 

 [[GitSync]]

### Настрока под Windows

1.	Установить [[Git]]
2.	Установить [[OneScript]]
3.	Установить [precommit4onec](https://github.com/bia-technologies/precommit4onec): `opm install precommit4onec`
1.	Создать каталог репозитория `/ExternalAnalisys/Repository/`
2.	Создать подкаталог: `/ExternalAnalisys/Repository/EpfSource`
3.	В каталоге `/ExternalAnalisys/Repository/` выполнить:

		git init

		git config --local core.quotepath false
		git config --local core.longpaths true

		git config user.name "Andrey Gusarov"
		git config user.email "a.gusarov@gnph.ru"

		precommit4onec install ./


6. Создать gitignore `/ExternalAnalisys/Repository/.gitignore`

		/src
		/.scannerwork
		sonar-project.properties
		check.bat

7. C помощью обработки ![[ВыгрузкаВнешнихОбработок.epf]] выполнить выгрузку в каталог `/ExternalAnalisys/Repository/EpfSource`
8. В каталоге выполнить:

		git add -A
		git commit -a -m "First commit"
