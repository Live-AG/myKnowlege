## Useful links
https://infostart.ru/1c/articles/810752/


### Windows
Файл register-ras.bat:

	@echo off
	rem %1 - полный номер версии 1С:Предприятия
	set SrvUserName=<имя пользователя>
	set SrvUserPwd=<пароль пользователя>
	set CtrlPort=1540
	set AgentName=localhost
	set RASPort=1545
	set SrvcName="1C:Enterprise 8.3 Remote Server"
	set BinPath="\"C:\Program Files\1cv8\%1\bin\ras.exe\" cluster --service --port=%RASPort% %AgentName%:%CtrlPort%"
	set Desctiption="Сервер администрирования 1С:Предприятия 8.3"
	sc stop %SrvcName%
	sc delete %SrvcName%
	sc create %SrvcName% binPath= %BinPath% start= auto obj= %SrvUserName% password= %SrvUserPwd% displayname= %Desctiption%



