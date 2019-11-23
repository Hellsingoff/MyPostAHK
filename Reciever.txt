SetTitleMatchMode, RegEx
CoordMode, Mouse, Screen

n := 2000
w := 200
e := 100
m := 50
s := 30
q := 3
z := 0

hotkey, Enter, Toggle

return

#F2::reload
#F3::exitapp
#F4::hotkey, Enter, Toggle

#F1::
z := 0
loop, %n%
	{
	Winactivate, - Excel
	sleep %e%
	Winactivate, - Excel
	sleep %e%
	Send  ^{vk43}
	sleep %e%
	send {Down}
	Loop
		{
		if WinExist("Предупреждение")
			{
			WinActivate, Предупреждение
			sleep 200
			Send {Enter}
			}
		if WinExist("Емкость")
			continue
		else
			break
		}
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	Mouseclick, Left, 895, 475
	sleep 200
	Send  ^{vk56}
	sleep 200
	Send  ^{vk56}
	sleep %e%
	send {Enter}
	winwait, Емкость|Предупреждение,, 30
	sleep %e%
	WinActivate, Предупреждение
	sleep %e%
	if WinActive("Предупреждение")
		{
		Send {Enter}
		sleep %e%
		Winactivate, - Excel
		sleep %e%
		Winactivate, - Excel
		sleep %e%
		Send {Up}
		sleep %e%
		Send  ^{vk42}
		sleep %e%
		Send {Down}
		continue
		}
	WinActivate, Емкость
	sleep %e%
	if WinActive("Емкость")
		{
		sleep %w%
		Mouseclick, Left, 519, 437, 2
		sleep %w%
		Send  ^{vk43}
		sleep %w%
		send {Enter}
		sleep %w%
		Send  ^{vk56}
		sleep %e%
		send {Enter}
		Mouseclick, Left, 600, 490
		sleep %e%
		Mouseclick, Left, 1132, 465
		sleep %w%
		send {Down}
		sleep %e%
		send {Enter}
		sleep %e%
		Mouseclick, Left, 467, 535
		if (z == 0)
			{
			Mouseclick, Left, 1125, 597
			Mouseclick, Left, 1125, 661
			z := 1
			}
		Mouseclick, Left, 1000, 675
		sleep 500
		if WinActive("Ошибка")
			{
			Send {Enter}
			sleep %w%
			Mouseclick, Left, 519, 437, 2
			sleep %w%
			Send  ^{vk43}
			sleep %w%
			send {Enter}
			sleep %w%
			Send  ^{vk56}
			sleep %w%
			Mouseclick, Left, 1132, 465
			sleep %w%
			Mouseclick, Left, 1132, 500
			sleep %w%
			Mouseclick, Left, 1000, 675
			sleep %w%
			}
		if WinActive("Ошибка")
			{
			Send {Enter}
			sleep %w%
			Mouseclick, Left, 519, 437, 2
			sleep %w%
			Send  ^{vk43}
			sleep %w%
			send {Enter}
			sleep %w%
			Send  ^{vk56}
			sleep %w%
			Mouseclick, Left, 1132, 465
			sleep %w%
			Mouseclick, Left, 1132, 500
			sleep %w%
			Mouseclick, Left, 1000, 675
			sleep %w%
			}
		}
	}
return

#F5::
FileSelectFile, SelectedFile, 3, %A_MyDocuments%, Выбор машины, Text Documents (*.txt)
Gui Reg:Add, Text, , Начать с:
Gui Reg:Add, Edit, vNumb, 1
Gui Reg:Add, Button, default gReg, ОК
Gui Reg:Show, w150 , Регистрация
return

Reg:
Gui, submit
CarTime:=""
FormatTime, CarTime,, d.MM HH_mm
z := 0
Loop, read, %SelectedFile%
	{
	if (Numb > 1)
		{
		Numb -= 1
		continue
		}
	Clipboard := A_LoopReadLine
	Loop
		{
		if WinExist("Предупреждение")
			{
			WinActivate, Предупреждение
			sleep 200
			Send {Enter}
			}
		if WinExist("Емкость")
			continue
		else
			break
		}
	while !(WinActive("Регистрация мешков по штрих-коду"))
		{
		sleep %e%
		winactivate, Регистрация мешков по штрих-коду
		}
	Mouseclick, Left, 895, 475
	sleep 200
	Send  ^{vk56}
	sleep 200
	Send  ^{vk56}
	sleep %e%
	send {Enter}
	winwait, Емкость|Предупреждение,, 30
	sleep %e%
	WinActivate, Предупреждение
	sleep %e%
	if WinActive("Предупреждение")
		{
		send {Enter}
		FileAppend, %A_LoopReadLine%`n, %A_MyDocuments%\%CarTime%.txt
		sleep 100
		continue
		}
	WinActivate, Емкость
	sleep %e%
	if WinActive("Емкость")
		{
		mass := SubStr(A_LoopReadLine, -2)/10
		clipboard := StrReplace(mass, ".", ",")
		sleep 100
		Send  ^{vk56}
		sleep %e%
		send {Enter}
		Mouseclick, Left, 600, 490
		sleep %e%
		Mouseclick, Left, 1132, 465
		sleep %w%
		send {Down}
		sleep %e%
		send {Enter}
		sleep %e%
		Mouseclick, Left, 467, 535
		if (z == 0)
			{
			Mouseclick, Left, 1125, 597
			Mouseclick, Left, 1125, 661
			z := 1
			}
		Mouseclick, Left, 1000, 675
		sleep 500
		if WinActive("Ошибка")
			{
			Send {Enter}
			sleep %w%
			Mouseclick, Left, 519, 437, 2
			sleep %w%
			Send  ^{vk43}
			sleep %w%
			send {Enter}
			sleep %w%
			Send  ^{vk56}
			sleep %w%
			Mouseclick, Left, 1132, 465
			sleep %w%
			Mouseclick, Left, 1132, 500
			sleep %w%
			Mouseclick, Left, 1000, 675
			sleep %w%
			}
		if WinActive("Ошибка")
			{
			Send {Enter}
			sleep %w%
			Mouseclick, Left, 519, 437, 2
			sleep %w%
			Send  ^{vk43}
			sleep %w%
			send {Enter}
			sleep %w%
			Send  ^{vk56}
			sleep %w%
			Mouseclick, Left, 1132, 465
			sleep %w%
			Mouseclick, Left, 1132, 500
			sleep %w%
			Mouseclick, Left, 1000, 675
			sleep %w%
			}
		}
	}
return

Enter::
Send {Enter}
winwait, Емкость
Mouseclick, Left, 519, 437, 2
sleep %e%
Send  ^{vk43}
sleep %e%
send {Enter}
sleep %e%
Send  ^{vk56}
sleep %e%
send {Enter}
Mouseclick, Left, 600, 490
Mouseclick, Left, 1132, 465
Mouseclick, Left, 1132, 500
Mouseclick, Left, 467, 535
if (z == 0)
	{
	Mouseclick, Left, 1125, 597
	Mouseclick, Left, 1125, 661
	z := 1
	}
Mouseclick, Left, 1000, 675
sleep %w%
if WinActive("Ошибка")
	{
	Send {Enter}
	sleep %e%
	Mouseclick, Left, 519, 437, 2
	sleep %w%
	Send  ^{vk43}
	sleep %w%
	send {Enter}
	sleep %w%
	Send  ^{vk56}
	sleep %e%
	Mouseclick, Left, 600, 490
	sleep %e%
	Mouseclick, Left, 1132, 465
	sleep %e%
	Mouseclick, Left, 1132, 500
	sleep %e%
	Mouseclick, Left, 1000, 675
	sleep %w%
	}
return

#F7::
loop, %n%
	{
	Winactivate, - Excel
	sleep %e%
	Winactivate, - Excel
	sleep %e%
	Send  ^{vk43}
	sleep %e%
	send {Down}
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	Mouseclick, Left, 895, 475
	sleep 200
	Send  ^{vk56}
	sleep %e%
	send {Enter}
	winwait, Предупреждение
	send {Enter}
	sleep 200
	}
return

#F6::
z := 0
loop, %n%
	{
	Winactivate, - Excel
	sleep %e%
	Winactivate, - Excel
	sleep %e%
	Send  ^{vk43}
	sleep %e%
	send {Down}
	sleep %e%
	Winwaitclose, Емкость
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	winactivate, Регистрация мешков по штрих-коду
	sleep %e%
	Mouseclick, Left, 895, 475
	sleep 200
	Send  ^{vk56}
	sleep 200
	Send  ^{vk56}
	sleep %e%
	send {Enter}
	winwait, Емкость|Предупреждение,, 30
	sleep %e%
	WinActivate, Предупреждение
	sleep %e%
	if WinActive("Предупреждение")
		{
		Send {Enter}
		sleep %e%
		Winactivate, - Excel
		sleep %e%
		Winactivate, - Excel
		sleep %e%
		Send {Up}
		sleep %e%
		Send  ^{vk42}
		sleep %e%
		Send {Down}
		continue
		}
	WinActivate, Емкость
	sleep %e%
	if WinActive("Емкость")
		{
		sleep %w%
		Mouseclick, Left, 519, 437, 2
		sleep %w%
		Send  ^{vk43}
		sleep %w%
		send {Enter}
		sleep %w%
		Send  ^{vk56}
		sleep %e%
		send {Enter}
		sleep %e%
		Mouseclick, Left, 467, 535
		sleep %e%
		Mouseclick, Left, 1000, 675
		sleep 500
		if WinActive("Ошибка")
			{
			Send {Enter}
			sleep %w%
			Mouseclick, Left, 519, 437, 2
			sleep %w%
			Send  ^{vk43}
			sleep %w%
			send {Enter}
			sleep %w%
			Send  ^{vk56}
			sleep %e%
			Mouseclick, Left, 1000, 675
			sleep %w%
			}
		}
	}
return