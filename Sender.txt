CoordMode, Mouse, Screen

;0-9 алфавит тонкий черный
Black.="|<0>*140$5.R6AMlWu"
Black.="|<1>*140$5.4MEV24S"
Black.="|<2>*140$5.S48F4Ey"
Black.="|<3>*140$5.S49UV2u"
Black.="|<4>*140$5.6QeTV26"
Black.="|<5>*140$5.SV3kV2u"
Black.="|<6>*140$5.CW7clmu"
Black.="|<7>*140$5.S48l68m"
Black.="|<8>*140$5.R6/dlWu"
Black.="|<9>*140$5.R6ALV6u"

;Тонкий черный шрифт, буквы типа почты
Black.="|<E>*140$5.z27sEVy"
Black.="|<ж>*140$7.ZeXWeJ9U"
Black.="|<м>*140$5.XihMlU"

;Тонкий красный шрифт, буквы ошибок
Red.="|<Д>*200$8.DW8W8W9WEY/zUsC"
Red.="|<О>*200$7.TMsQ631Vsro"
Red.="|<Е>*200$6.zUUUyUUUzU"

;Толстый шрифт числа мешков
Fat.="|<0>*114$5.RiAMlqu"
Fat.="|<1>*114$5.BslX6By"
Fat.="|<2>*114$5.xAMnity"
Fat.="|<3>*114$5.zAPVV7u"
Fat.="|<4>*114$5.CRytz6C"
Fat.="|<5>*114$5.zX7lV7u"
Fat.="|<6>*114$5.TX7stqu"
Fat.="|<7>*114$5.yANXANW"
Fat.="|<8>*114$5.TjDjlny"
Fat.="|<9>*114$5.RiCTX7u"

CarTime:=""
FormatTime, CarTime,, d.MM HH_mm
WinGetPos, Xpos, Ypos, , , Документ

#F2::reload

#F1::
OLD:=FindTextOCR(Xpos+317, Ypos+232, 9, 4, 0.2, 0.1, Fat)
s:=10
x:=0
Ch:=0
NEW:=""
NEWIND:=""
NEWGROUP:=""
OLDGROUP:=""
BAGNUM:=""
Gui Maing:+AlwaysOnTop
Gui Maing:Font, s300
Gui Maing:Add, Text,vNEWGROUP x0 y0 w680,0
Gui Maing:Add, Text,vNEW x0 y450 w680,0
Gui Maing:Show, w700 h830 x900 y0
WinGetPos, Xpos, Ypos, , , Документ
if ((Xpos == 448) && (Ypos == 176))
	MouseClickDrag, Left, 550, 190, 200, 190
WinGetPos, Xpos, Ypos, , , Документ
MouseClick, left, Xpos+280, Ypos+230
if (OLD < 0)
	OLD := 0
if (OLD > 0)
	{
	ycoord := GetYcoord()
	TYPEPOST:=FindTextOCR(Xpos+117, ycoord, 4, 4, 0.01, 0.01, Black)
	NEWIND:=FindTextOCR(Xpos+402, ycoord, 18, 4, 0.01, 0.01, Black)
	NEWGROUP:=Group(NEWIND, TYPEPOST)
	GuiControl, Maing:, NEWGROUP,%NEWGROUP%
	GuiControl, Maing:,NEW,%OLD%
	}
Loop
	{
	WinGetPos, Xpos, Ypos, , , Документ
	if WinActive("Предупреждение")
		{
		sleep 500
		if WinActive("Предупреждение")
			{
			SoundPlay *16
			Sleep 150
			SoundPlay *16
			Gui Err:Color, Red
			Gui Err:Font, S64 CDefault Bold, Verdana
			Gui Err:Add, Text, x2 y0 w790 h120 , Пункт выгрузки
			Gui Err:Add, Text, x55 y110 w680 h110 , 1 - приписать
			Gui Err:Add, Edit, vCh x332 y250 w130 h120 , 2
			Gui Err:Add, Button, default gSubmit x582 y280 w170 h80 , ОК
			Gui Err:Show, x100 y200 h392 w798, Пункт выгрузки
			sleep 100
			continue
			}
		}
	if WinActive("Объект заблокирован")
		{
		Send {Left}
		Sleep 100
		Send {Enter}
		Sleep 500
		WinActivate, Документ
		continue
		}
	NEW:=FindTextOCR(Xpos+317, Ypos+232, 9, 4, 0.1, 0.1, Fat)
	OLDERROR := ERROR
	ERROR:=FindTextOCR(Xpos+372, Ypos+220, 16, 16, 0.1, 0.1, Red)
	if (ERROR != OLDERROR)
		{
		if (ERROR == "О")
			Gui Maing:Color, Yellow
		if (ERROR == "Д")
			Gui Maing:Color, Green
		if (ERROR == "Е")
			Gui Maing:Color, Red
		if (ERROR == "")
			Gui Maing:Color, White
		}
	if (NEW > 0)
		{
		if (OLD != NEW)
			{
			CNT := NEW - OLD
			ycoord := GetYcoord()
			if (ycoord == 0)
				continue
			ycoord := ycoord - ((CNT - 1) * 14)
			if (OLD > NEW)
				{
				if (CNT == -1)
					{
					SoundPlay *16
					ycoord := GetYcoord()
					TYPEPOST:=FindTextOCR(Xpos+117, ycoord, 4, 4, 0.01, 0.01, Black)
					NEWIND:=FindTextOCR(Xpos+402, ycoord, 18, 4, 0.01, 0.01, Black)
					NEWGROUP:=Group(NEWIND, TYPEPOST)
					GuiControl, Maing:, NEWGROUP,%NEWGROUP%
					}
				else if (x < 5)
					{
					x+=1
					sleep 250
					continue
					}
				else
					{
					SoundPlay *16
					Sleep 100
					SoundPlay *16
					x:=0
					}
				}
			if (CNT > 9)
				{
				if (x < 5)
					{
					x+=1
					sleep 250
					continue
					}
				else
					{
					Soundplay *48
					x:=0
					}
				}
			GuiControl, Maing:,NEW,%NEW%
			x:=0
			If ((NEW > OLD) || (CNT <= 9))
					Loop, %CNT%
						{
						BAGNUM:=FindTextOCR(Xpos+241, ycoord, 48, 4, 0.01, 0.01, Black)
						TYPEPOST:=FindTextOCR(Xpos+117, ycoord, 4, 4, 0.01, 0.01, Black)
						NEWIND:=FindTextOCR(Xpos+402, ycoord, 18, 4, 0.01, 0.01, Black)
						FileAppend, %BAGNUM%`n, %A_MyDocuments%\%CarTime%.txt
						OLDGROUP:=NEWGROUP
						NEWGROUP:=Group(NEWIND, TYPEPOST)
						if (NEWGROUP == "A")
							{
							MASS:=FindTextOCR(Xpos+605, ycoord, 14, 4, 0.01, 0.01, Black)
							if (MASS > 2000)
								Loop, 2
									{
									SoundPlay *48
									Sleep 200
									}
							}
						ycoord += 14
						if (OLDGROUP!=NEWGROUP)
							{
							SoundPlay *48
							GuiControl, Maing:, NEWGROUP,%NEWGROUP%
							}
						else
							{
							SoundPlay, %A_MyDocuments%\Ring.wav
							}
						}
			OLD:=NEW
			}
		}
	if ((NEW == 0) && (OLD == 1))
		{
		OLD:=0
		SoundPlay *48
		GuiControl, Maing:,NEW,%NEW%
		NEWGROUP := 0
		GuiControl, Maing:,NEWGROUP,%NEWGROUP%
		}
	Sleep 100
	}
Return

GetYcoord() {
global Xpos, Ypos, Black
ycoord := Ypos+460
NEWIND:=""
arrcount := 1
while ((NEWIND == "") && (arrcount <= 10))
	{
	ycoord -= 14
	NEWIND:=FindTextOCR(Xpos+402, ycoord, 5, 4, 0.01, 0.01, Black)
	arrcount += 1
	}
if (arrcount == 11)
	return 0
return ycoord
}

Group(ind, TYPEPOST) {
if ((ind >= 100000) && (ind <= 101999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 1
	}
else if ((ind >= 102000) && (ind <= 102999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 103000) && (ind <= 149999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 1
	}
else if ((ind >= 150000) && (ind <= 152999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C5"
	}
else if ((ind >= 153000) && (ind <= 155999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C5"
	}
else if ((ind >= 156000) && (ind <= 157999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C5"
	}
else if ((ind >= 160000) && (ind <= 162999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 15
	}
else if ((ind >= 163000) && (ind <= 165999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 13
	}
else if ((ind >= 166000) && (ind <= 166999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 167000) && (ind <= 169999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 6
	}
else if ((ind >= 170000) && (ind <= 172999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C2"
	}
else if ((ind >= 173000) && (ind <= 175999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 3
	}
else if ((ind >= 180000) && (ind <= 182999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 3
	}
else if ((ind >= 183000) && (ind <= 184999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 13
	}
else if ((ind >= 185000) && (ind <= 186999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 3
	}
else if ((ind >= 187000) && (ind <= 200999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 3
	}
else if ((ind >= 214000) && (ind <= 216999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if (ind == 228228)
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 236000) && (ind <= 238999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 13
	}
else if ((ind >= 241000) && (ind <= 243999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 248000) && (ind <= 249999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C2"
	}
else if ((ind >= 295000) && (ind <= 298999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 10
	}
else if ((ind >= 299000) && (ind <= 299999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 10
	}
else if ((ind >= 300000) && (ind <= 301999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C2"
	}
else if ((ind >= 302000) && (ind <= 303999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 305000) && (ind <= 307999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 308000) && (ind <= 309999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 344000) && (ind <= 347999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 350000) && (ind <= 354999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 355000) && (ind <= 357999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 10
	}
else if ((ind >= 358000) && (ind <= 359999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 9
	}
else if ((ind >= 360000) && (ind <= 361999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 362000) && (ind <= 363999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 364000) && (ind <= 366999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 367000) && (ind <= 368999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 369000) && (ind <= 369999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 385000) && (ind <= 385999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 10
	}
else if ((ind >= 386000) && (ind <= 386999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C1"
	}
else if ((ind >= 390000) && (ind <= 391999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 392000) && (ind <= 393999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 2
	}
else if ((ind >= 394000) && (ind <= 397999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 398000) && (ind <= 399999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 400000) && (ind <= 404999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 9
	}
else if ((ind >= 410000) && (ind <= 413999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 9
	}
else if ((ind >= 414000) && (ind <= 416999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 9
	}
else if ((ind >= 420000) && (ind <= 423999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 424000) && (ind <= 425999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 426000) && (ind <= 427999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 428000) && (ind <= 429999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 430000) && (ind <= 431999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 432000) && (ind <= 433999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 440000) && (ind <= 442999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 9
	}
else if ((ind >= 443000) && (ind <= 446999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 450000) && (ind <= 453999))
	return "C8"
else if ((ind >= 454000) && (ind <= 457999))
	return "C8"
else if ((ind >= 460000) && (ind <= 462999))
	return 12
else if ((ind >= 468000) && (ind <= 468999))
	return 12
else if ((ind >= 600000) && (ind <= 602999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C2"
	}
else if ((ind >= 603000) && (ind <= 607999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return "C3"
	}
else if ((ind >= 610000) && (ind <= 613999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 6
	}
else if ((ind >= 614000) && (ind <= 619999))
	return "C7"
else if ((ind >= 620000) && (ind <= 620965))
	return "C6"
else if (ind == 620966)
	return "V"
else if ((ind >= 620967) && (ind <= 620969))
	return "C6"
else if (ind == 620970)
	return "A"
else if ((ind >= 620971) && (ind <= 620980))
	return "C6"
else if (ind == 620981)
	return "M"
else if ((ind >= 620982) && (ind <= 624999))
	return "C6"
else if ((ind >= 625000) && (ind <= 629999))
	return "C4"
else if ((ind >= 630000) && (ind <= 633999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 5
	}
else if ((ind >= 634000) && (ind <= 636999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 5
	}
else if ((ind >= 640000) && (ind <= 641999))
	return "C9"
else if ((ind >= 644000) && (ind <= 646999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 14
	}
else if ((ind >= 647000) && (ind <= 648999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 17
	}
else if ((ind >= 649000) && (ind <= 649999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 5
	}
else if ((ind >= 650000) && (ind <= 654999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 5
	}
else if ((ind >= 655000) && (ind <= 655999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 4
	}
else if ((ind >= 656000) && (ind <= 659999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 5
	}
else if ((ind >= 660000) && (ind <= 663299))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 4
	}
else if ((ind >= 663300) && (ind <= 663399))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 17
	}
else if ((ind >= 663400) && (ind <= 663999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 4
	}
else if ((ind >= 664000) && (ind <= 666999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 4
	}
else if ((ind >= 667000) && (ind <= 668999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 4
	}
else if ((ind >= 670000) && (ind <= 671999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 8
	}
else if ((ind >= 672000) && (ind <= 674999))
	{
	if (TYPEPOST == "E")
		return "A"
	else
		return 8
	}
else if ((ind >= 675000) && (ind <= 676999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else if ((ind >= 677000) && (ind <= 678999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 17
	}
else if ((ind >= 679000) && (ind <= 679999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else if ((ind >= 680000) && (ind <= 680999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else if ((ind >= 681000) && (ind <= 681045))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 7
	}
else if ((ind >= 681046) && (ind <= 682999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else if ((ind >= 683000) && (ind <= 684999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 7
	}
else if ((ind >= 685000) && (ind <= 686999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 17
	}
else if ((ind >= 689000) && (ind <= 689999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else if ((ind >= 690000) && (ind <= 692999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 7
	}
else if ((ind >= 693000) && (ind <= 694999))
	{
	if ((TYPEPOST == "E") || (TYPEPOST == "м"))
		return "A"
	else
		return 8
	}
else
	return 0
}

#F3::
Gui +AlwaysOnTop
Gui, Font, s20
Gui, Add, ListBox, r2 vTYPE, Простая|Заказ
Gui, Add, Edit, vINDEX
Gui, Add, Button, Default, OK
Gui, Show
return

#F5::
Gui Maing:destroy
Gui +AlwaysOnTop
Gui, Font, s20
Gui, Add, ListBox, r2 vCOPY, 1 копия|2 копии
Gui, Add, ListBox, r2 vSEND, Отправить|Не отправлять
Gui, Add, Button, Default, Print
Gui, Show
return

ButtonPrint:
Gui, Submit
Gui, destroy
Mouseclick, left, Xpos+555, Ypos+520
Print()
WinWait, Предупреждение
WinActivate, Предупреждение
sleep 1000
if (SEND == "Отправить")
	{
	send {Left}
	sleep 100
	send {Enter}
	WinWait, Отправка
	send {Enter 3}
	}
else
	send {Enter}
WinWaitClose, Отправка
sleep 1000
if (COPY == "2 копии")
	{
	send {Enter}
	Print()
	}
reload
return

Print() {
global Black
Loop
	{
	Sleep 1000
	IfWinExist, Печать
		{
		WinActivate, Печать
		WinGetPos, PrintXpos, PrintYpos, , , Печать
		PRINT23a:=FindTextOCR(PrintXpos+168, PrintYpos+43, 10, 6, 0.01, 0.01, Black)
		PRINT23:=FindTextOCR(PrintXpos+139, PrintYpos+43, 10, 6, 0.01, 0.01, Black)
		if (PRINT23a == 23)
			{
			send {Enter 2}
			break
			}
		if ((PRINT23 == 23) || (PRINT23a == 16))
			send {Enter 2}
		if ((PRINT23 == "") && (PRINT23a == ""))
			{
			Mouseclick, left, A_ScreenWidth - 1, A_ScreenWidth - 1
			sleep 500
			}
		PRINT23a := ""
		PRINT23 := ""
		}
	IfWinExist, Предупреждение
		break
	}
}

ButtonOK:
Gui, Submit
Gui, destroy
if (TYPE == "Простая")
	SELECTTYPE := "ж"
if (TYPE == "Заказ")
	SELECTTYPE := "м"
OLD:=FindTextOCR(765, 408, 9, 4, 0.2, 0.1, Fat)
if (OLD < 0)
	OLD := 0
s:=10
x:=0
Ch:=0
NEW:=""
NEWIND:=""
NEWGROUP:=""
OLDGROUP:=""
BAGNUM:=""
Gui +AlwaysOnTop
Gui, Font, s300
Gui, Add, Text,vNEWGROUP x0 y0 w680,0
Gui, Add, Text,vNEW x0 y450 w680,0
Gui, Show, w700 h830 x900 y0
Loop
	{
	if WinActive("Предупреждение")
		{
		sleep 500
		if WinActive("Предупреждение")
			{
			SoundPlay *16
			Sleep 150
			SoundPlay *16
			Gui Err:Color, Red
			Gui Err:Font, S64 CDefault Bold, Verdana
			Gui Err:Add, Text, x2 y0 w790 h120 , Пункт выгрузки
			Gui Err:Add, Text, x55 y110 w680 h110 , 1 - приписать
			Gui Err:Add, Edit, vCh x332 y250 w130 h120 , 2
			Gui Err:Add, Button, default gSubmit x582 y280 w170 h80 , ОК
			Gui Err:Show, x100 y200 h392 w798, Пункт выгрузки
			sleep 100
			continue
			}
		}
	if WinActive("Объект заблокирован")
		{
		Send {Left}
		Sleep 100
		Send {Enter}
		Sleep 500
		WinActivate, Документ
		continue
		}
	NEW:=FindTextOCR(765, 408, 9, 4, 0.1, 0.1, Fat)
	if (NEW > 0)
		{
		if (OLD != NEW)
			{
			if (OLD > NEW)
				{
				if ((OLD-NEW) == 1)
					{
					SoundPlay *16
					}
				else if (x < 5)
					{
					x+=1
					sleep 250
					continue
					}
				else
					{
					SoundPlay *16
					Sleep 100
					SoundPlay *16
					x:=0
					}
				}
			CNT := NEW - OLD
			if (CNT > 5)
				{
				if (x < 5)
					{
					x+=1
					sleep 250
					continue
					}
				else
					{
					Soundplay *48
					x:=0
					}
				}
			if (CNT <= 5)
				{
				Loop, %CNT%
					{
					SoundPlay, %A_MyDocuments%\Ring.wav
					Sleep 200
					}
				}
			GuiControl,,NEW,%NEW%
			x:=0
			Sleep 100
			NEWIND:=""
			arrcount := 1
			while (NEWIND == "")
				{
				if (arrcount <= 9)
					{
					Arraycoord := [622, 608, 594, 580, 566, 552, 538, 524, 510]
					coordy := Arraycoord[arrcount]
					arrcount += 1
					NEWIND:=FindTextOCR(850, coordy, 18, 4, 0.01, 0.01, Black)
					}
				}
			BAGNUM:=FindTextOCR(689, coordy, 48, 4, 0.01, 0.01, Black)
			TYPEPOST:=FindTextOCR(565, coordy, 4, 4, 0.01, 0.01, Black)
			If (NEW > OLD)
				FileAppend, %BAGNUM%`n, %A_MyDocuments%\%CarTime%.txt
			OLD:=NEW
			if ((TYPEPOST != SELECTTYPE)||(NEWIND != INDEX))
				{
				NEWGROUP:="ER"
				sleep 100
				SoundPlay *48
				}
			else
				NEWGROUP:="OK"
			GuiControl,,NEWGROUP,%NEWGROUP%
			}
		}
	}
	if ((NEW == 0) && (OLD == 1))
		{
		OLD:=0
		SoundPlay *48
		}
	Sleep 100
Return

#F8::
MouseMove, 0, 0
ycoord := Ypos+334
BagsToRem := ""
Loop, 9
	{
	BAG := FindTextOCR(Xpos+241, ycoord, 48, 4, 0.01, 0.01, Black)
	BagsToRem .= BAG "|"
	ycoord += 14
	}
Gui Rem:Font, s20
Gui Rem:Add, ListBox, r9 vRemBag, %BagsToRem%
Gui Rem:Add, Button, Default gRemove, Remove
Gui Rem:Show
return

Remove:
Gui Rem:submit
Gui Rem:destroy
FileAppend, %RemBag%`n, %A_MyDocuments%\%CarTime%_del.txt
Loop, read, %A_MyDocuments%\%CarTime%.txt
	{
	If ((A_LoopReadLine > 0) && !(InStr(A_LoopReadLine, RemBag)))
		FileAppend, %A_LoopReadLine%`n, %A_MyDocuments%\tmp.txt
	}
ycoord := Ypos+334
MouseMove, 0, 0
Loop, 9
	{
	BAG := FindTextOCR(Xpos+241, ycoord, 48, 4, 0.01, 0.01, Black)
	if (BAG == RemBag)
		{
		MouseClick, Left, Xpos+241, ycoord
		Send {F8}
		Send +{Tab}
		break
		}
	ycoord += 14
	}
MouseMove, 0, 0
FileDelete, %A_MyDocuments%\%CarTime%.txt
FileMove, %A_MyDocuments%\tmp.txt, %A_MyDocuments%\%CarTime%.txt
return

#F9::
MouseMove, 0, 0
ycoord := GetYcoord()
BAG := FindTextOCR(Xpos+241, ycoord, 48, 4, 0.01, 0.01, Black)
FileAppend, %RemBag%`n, %A_MyDocuments%\%CarTime%_del.txt
Loop, read, %A_MyDocuments%\%CarTime%.txt
	{
	If ((A_LoopReadLine > 0) && !(InStr(A_LoopReadLine, RemBag)))
		FileAppend, %A_LoopReadLine%`n, %A_MyDocuments%\tmp.txt
	}
FileDelete, %A_MyDocuments%\%CarTime%.txt
FileMove, %A_MyDocuments%\tmp.txt, %A_MyDocuments%\%CarTime%.txt
Send {Enter}
sleep 200
Send {F8}
sleep 200
Send {Esc}
return

#F12::
FileSelectFile, SelectedFile, 3, %A_MyDocuments%, Выбор машины, Text Documents (*.txt)
Gui Reg:Add, Text, , Начать с:
Gui Reg:Add, Edit, vNumb, 1
Gui Reg:Add, Button, default gReg, ОК
Gui Reg:Show, w150 , Регистрация
return

Reg:
gui Reg:submit, NoHide
sleep 100
gui Reg:destroy
Loop, read, %SelectedFile%
	{
	if (Numb > 1)
		{
		Numb -= 1
		continue
		}
	WinGetPos, Xpos, Ypos, , , Документ
	OLD:=FindTextOCR(Xpos+317, Ypos+232, 9, 4, 0.2, 0.1, Fat)
	Loop
		{
		Clipboard := A_LoopReadLine
		sleep 100
		send ^{vk56}
		sleep 100
		send {Enter}
		sleep 200
		if WinActive("Объект заблокирован")
			{
			sleep 100
			Send {Left}
			Sleep 100
			Send {Enter}
			Sleep 500
			WinActivate, Документ
			}
		if WinActive("Предупреждение")
			{
			clipboard := "3023443vsh"
			sleep 250
			send {Left}
			Sleep 250
			send {Enter}
			sleep 250
			send ^{vk56}
			sleep 250
			send {Enter}
			}
		sleep 200
		Loop, 20
			if (NEW == OLD)
				{
				NEW:=FindTextOCR(Xpos+317, Ypos+232, 9, 4, 0.2, 0.1, Fat)
				sleep 500
				}
		if ((OLD == NEW-1) && (NEW > 0))
			{
			sleep 100
			OLD := NEW
			break
			}
		else 
			MouseClick, left, Xpos+280, Ypos+230
		}
	}
return

Submit:
gui Err:submit, NoHide
sleep 500
gui Err:destroy
if (Ch == 1)
	{
	clipboard := "3023443vsh"
	sleep 250
	send {Left}
	Sleep 250
	send {Enter}
	sleep 250
	send ^{vk56}
	sleep 250
	send {Enter}
	}
if (Ch != 1)
	{
	sleep 250
	Send {Enter}
	sleep 100
	}
ch:=0
return

GuiClose:
Reload

FindText(x,y,w,h,err1,err0,text)
{
  xywh2xywh(x-w,y-h,2*w+1,2*h+1,x,y,w,h)
  if (w<1 or h<1)
    return, 0
  bch:=A_BatchLines
  SetBatchLines, -1
  ;--------------------------------------
  GetBitsFromScreen(x,y,w,h,Scan0,Stride,bits)
  ;--------------------------------------
  sx:=0, sy:=0, sw:=w, sh:=h, arr:=[]
  Loop, Parse, text, |
  {
    v:=A_LoopField
    IfNotInString, v, $, Continue
    comment:="", e1:=err1, e0:=err0
    ; You Can Add Comment Text within The <>
    if RegExMatch(v,"<([^>]*)>",r)
      v:=StrReplace(v,r), comment:=Trim(r1)
    ; You can Add two fault-tolerant in the [], separated by commas
    if RegExMatch(v,"\[([^\]]*)]",r)
    {
      v:=StrReplace(v,r), r1.=","
      StringSplit, r, r1, `,
      e1:=r1, e0:=r2
    }
    StringSplit, r, v, $
    color:=r1, v:=r2
    StringSplit, r, v, .
    w1:=r1, v:=base64tobit(r2), h1:=StrLen(v)//w1
    if (r0<2 or h1<1 or w1>sw or h1>sh or StrLen(v)!=w1*h1)
      Continue
    ;--------------------------------------------
    mode:=InStr(color,"*") ? 1:0
    color:=StrReplace(color,"*") . "@"
    StringSplit, r, color, @
    color:=mode=1 ? r1 : ((r1-1)//w1)*Stride+Mod(r1-1,w1)*4
    n:=Round(r2,2)+(!r2), n:=Floor(255*3*(1-n))
    StrReplace(v,"1","",len1), len0:=StrLen(v)-len1
    VarSetCapacity(allpos, 1024*4, 0), k:=StrLen(v)*4
    VarSetCapacity(s1, k, 0), VarSetCapacity(s0, k, 0)
    ;--------------------------------------------
    if (ok:=PicFind(mode,color,n,Scan0,Stride,sx,sy,sw,sh
      ,v,s1,s0,Round(len1*e1),Round(len0*e0),w1,h1,allpos))
      or (err1=0 and err0=0
      and (ok:=PicFind(mode,color,n,Scan0,Stride,sx,sy,sw,sh
      ,v,s1,s0,Round(len1*0.05),Round(len0*0.05),w1,h1,allpos)))
    {
      Loop, % ok
        pos:=NumGet(allpos, 4*(A_Index-1), "uint")
        , rx:=(pos&0xFFFF)+x, ry:=(pos>>16)+y
        , arr.Push( [rx,ry,w1,h1,comment] )
    }
  }
  SetBatchLines, %bch%
  return, arr.MaxIndex() ? arr:0
}

PicFind(mode, color, n, Scan0, Stride, sx, sy, sw, sh
  , ByRef text, ByRef s1, ByRef s0
  , err1, err0, w1, h1, ByRef allpos)
{
  static MyFunc
  if !MyFunc
  {
    x32:="5589E55383EC50C745E8000000008B45242B454083C0018"
    . "945C88B45282B454483C0018945C48B45200FAF45188B551CC"
    . "1E20201D08945C0C745CC000000008B45CC8945D08B45D0894"
    . "5F8C745EC00000000EB78C745F000000000EB638B45EC0FAF4"
    . "5188B55F0C1E20201D08945BC8B45F88D50018955F889C28B4"
    . "52C01D00FB6003C31751C8B45D08D50018955D08D148500000"
    . "0008B453001C28B45BC8902EB1A8B45CC8D50018955CC8D148"
    . "5000000008B453401C28B45BC89028345F0018B45F03B45407"
    . "C958345EC018B45EC3B45447C808B45D03945CC0F4D45CC894"
    . "5B8837D08000F854D020000C745EC00000000E930020000C74"
    . "5F000000000E9140200008B45EC0FAF45188B55F0C1E20201C"
    . "28B45C001D08945F88B45388945D88B453C8945D48B55F88B4"
    . "50C01D08945BC8B45BC83C00289C28B451401D00FB6000FB6C"
    . "08945B48B45BC83C00189C28B451401D00FB6000FB6C08945B"
    . "08B55BC8B451401D00FB6000FB6C08945ACC745F400000000E"
    . "94C0100008B45F43B45D00F8D9A0000008B45F48D148500000"
    . "0008B453001D08B108B45F801D08945BC8B45BC83C00289C28"
    . "B451401D00FB6000FB6C02B45B48945E48B45BC83C00189C28"
    . "B451401D00FB6000FB6C02B45B08945E08B55BC8B451401D00"
    . "FB6000FB6C02B45AC8945DC837DE4007903F75DE4837DE0007"
    . "903F75DE0837DDC007903F75DDC8B55E48B45E001C28B45DC0"
    . "1D03B45107E0E836DD801837DD8000F88EF0000008B45F43B4"
    . "5CC0F8D960000008B45F48D1485000000008B453401D08B108"
    . "B45F801D08945BC8B45BC83C00289C28B451401D00FB6000FB"
    . "6C02B45B48945E48B45BC83C00189C28B451401D00FB6000FB"
    . "6C02B45B08945E08B55BC8B451401D00FB6000FB6C02B45AC8"
    . "945DC837DE4007903F75DE4837DE0007903F75DE0837DDC007"
    . "903F75DDC8B55E48B45E001C28B45DC01D03B45107F0A836DD"
    . "401837DD40078508345F4018B45F43B45B80F8CA8FEFFFF8B4"
    . "5E88D50018955E88D1485000000008B454801D08B4D208B55E"
    . "C01CA89D3C1E3108B4D1C8B55F001CA09DA8910817DE8FF030"
    . "0000F8FFE010000EB0490EB01908345F0018B45F03B45C80F8"
    . "CE0FDFFFF8345EC018B45EC3B45C40F8CC4FDFFFFE9D701000"
    . "08B450C83C00169C0E803000089450CC745EC00000000E98D0"
    . "00000C745F000000000EB788B45EC0FAF45188B55F0C1E2020"
    . "1C28B45C001D08945F88B45F883C00389C28B451401D08B55F"
    . "883C20289D18B551401CA0FB6120FB6D269CA2B0100008B55F"
    . "883C20189D38B551401DA0FB6120FB6D269D24B0200008D1C1"
    . "18B4DF88B551401CA0FB6120FB6D26BD27201DA3B550C0F9CC"
    . "288108345F0018B45F03B45247C808345EC018B45EC3B45280"
    . "F8C67FFFFFF8345C003C745EC00000000E901010000C745F00"
    . "0000000E9E50000008B45EC0FAF45188B55F0C1E20201C28B4"
    . "5C001D08945F88B45388945D88B453C8945D4C745F40000000"
    . "0EB708B45F43B45D07D2E8B45F48D1485000000008B453001D"
    . "08B108B45F801D089C28B451401D00FB6003C01740A836DD80"
    . "1837DD800787B8B45F43B45CC7D2E8B45F48D1485000000008"
    . "B453401D08B108B45F801D089C28B451401D00FB60084C0740"
    . "A836DD401837DD40078488345F4018B45F43B45B87C888B45E"
    . "88D50018955E88D1485000000008B454801D08B4D208B55EC0"
    . "1CA89D3C1E3108B4D1C8B55F001CA09DA8910817DE8FF03000"
    . "07F2BEB0490EB01908345F0018B45F03B45C80F8C0FFFFFFF8"
    . "345EC018B45EC3B45C40F8CF3FEFFFFEB0490EB01908B45E88"
    . "3C4505B5DC244009090"
    x64:="554889E54883EC50894D10895518448945204C894D28C74"
    . "5EC000000008B45482B858000000083C0018945CC8B45502B8"
    . "58800000083C0018945C88B45400FAF45308B5538C1E20201D"
    . "08945C4C745D0000000008B45D08945D48B45D48945FCC745F"
    . "000000000E988000000C745F400000000EB708B45F00FAF453"
    . "08B55F4C1E20201D08945C08B45FC8D50018955FC4863D0488"
    . "B45584801D00FB6003C3175218B45D48D50018955D44898488"
    . "D148500000000488B45604801C28B45C08902EB1F8B45D08D5"
    . "0018955D04898488D148500000000488B45684801C28B45C08"
    . "9028345F4018B45F43B85800000007C858345F0018B45F03B8"
    . "5880000000F8C69FFFFFF8B45D43945D00F4D45D08945BC837"
    . "D10000F8582020000C745F000000000E965020000C745F4000"
    . "00000E9490200008B45F00FAF45308B55F4C1E20201C28B45C"
    . "401D08945FC8B45708945DC8B45788945D88B55FC8B451801D"
    . "08945C08B45C083C0024863D0488B45284801D00FB6000FB6C"
    . "08945B88B45C083C0014863D0488B45284801D00FB6000FB6C"
    . "08945B48B45C04863D0488B45284801D00FB6000FB6C08945B"
    . "0C745F800000000E96C0100008B45F83B45D40F8DAA0000008"
    . "B45F84898488D148500000000488B45604801D08B108B45FC0"
    . "1D08945C08B45C083C0024863D0488B45284801D00FB6000FB"
    . "6C02B45B88945E88B45C083C0014863D0488B45284801D00FB"
    . "6000FB6C02B45B48945E48B45C04863D0488B45284801D00FB"
    . "6000FB6C02B45B08945E0837DE8007903F75DE8837DE400790"
    . "3F75DE4837DE0007903F75DE08B55E88B45E401C28B45E001D"
    . "03B45207E0E836DDC01837DDC000F88090100008B45F83B45D"
    . "00F8DA60000008B45F84898488D148500000000488B4568480"
    . "1D08B108B45FC01D08945C08B45C083C0024863D0488B45284"
    . "801D00FB6000FB6C02B45B88945E88B45C083C0014863D0488"
    . "B45284801D00FB6000FB6C02B45B48945E48B45C04863D0488"
    . "B45284801D00FB6000FB6C02B45B08945E0837DE8007903F75"
    . "DE8837DE4007903F75DE4837DE0007903F75DE08B55E88B45E"
    . "401C28B45E001D03B45207F0A836DD801837DD800785A8345F"
    . "8018B45F83B45BC0F8C88FEFFFF8B45EC8D50018955EC48984"
    . "88D148500000000488B85900000004801D08B4D408B55F001C"
    . "AC1E2104189D08B4D388B55F401CA4409C28910817DECFF030"
    . "0000F8F3A020000EB0490EB01908345F4018B45F43B45CC0F8"
    . "CABFDFFFF8345F0018B45F03B45C80F8C8FFDFFFFE91302000"
    . "08B451883C00169C0E8030000894518C745F000000000E9A40"
    . "00000C745F400000000E9880000008B45F00FAF45308B55F4C"
    . "1E20201C28B45C401D08945FC8B45FC83C0034863D0488B452"
    . "84801D08B55FC83C2024863CA488B55284801CA0FB6120FB6D"
    . "269CA2B0100008B55FC83C2014C63C2488B55284C01C20FB61"
    . "20FB6D269D24B020000448D04118B55FC4863CA488B5528480"
    . "1CA0FB6120FB6D26BD2724401C23B55180F9CC288108345F40"
    . "18B45F43B45480F8C6CFFFFFF8345F0018B45F03B45500F8C5"
    . "0FFFFFF8345C403C745F000000000E926010000C745F400000"
    . "000E90A0100008B45F00FAF45308B55F4C1E20201C28B45C40"
    . "1D08945FC8B45708945DC8B45788945D8C745F800000000E98"
    . "40000008B45F83B45D47D3A8B45F84898488D1485000000004"
    . "88B45604801D08B108B45FC01D04863D0488B45284801D00FB"
    . "6003C01740E836DDC01837DDC000F88910000008B45F83B45D"
    . "07D368B45F84898488D148500000000488B45684801D08B108"
    . "B45FC01D04863D0488B45284801D00FB60084C0740A836DD80"
    . "1837DD80078568345F8018B45F83B45BC0F8C70FFFFFF8B45E"
    . "C8D50018955EC4898488D148500000000488B8590000000480"
    . "1D08B4D408B55F001CAC1E2104189D08B4D388B55F401CA440"
    . "9C28910817DECFF0300007F2BEB0490EB01908345F4018B45F"
    . "43B45CC0F8CEAFEFFFF8345F0018B45F03B45C80F8CCEFEFFF"
    . "FEB0490EB01908B45EC4883C4505DC39090909090909090"
    MCode(MyFunc, A_PtrSize=8 ? x64:x32)
  }
  return, DllCall(&MyFunc, "int",mode
    , "uint",color, "int",n, "ptr",Scan0, "int",Stride
    , "int",sx, "int",sy, "int",sw, "int",sh
    , "AStr",text, "ptr",&s1, "ptr",&s0
    , "int",err1, "int",err0, "int",w1, "int",h1, "ptr",&allpos)
}

xywh2xywh(x1,y1,w1,h1,ByRef x,ByRef y,ByRef w,ByRef h)
{
  SysGet, zx, 76
  SysGet, zy, 77
  SysGet, zw, 78
  SysGet, zh, 79
  left:=x1, right:=x1+w1-1, up:=y1, down:=y1+h1-1
  left:=left<zx ? zx:left, right:=right>zx+zw-1 ? zx+zw-1:right
  up:=up<zy ? zy:up, down:=down>zy+zh-1 ? zy+zh-1:down
  x:=left, y:=up, w:=right-left+1, h:=down-up+1
}

GetBitsFromScreen(x,y,w,h,ByRef Scan0,ByRef Stride,ByRef bits)
{
  VarSetCapacity(bits,w*h*4,0), bpp:=32
  Scan0:=&bits, Stride:=((w*bpp+31)//32)*4
  Ptr:=A_PtrSize ? "UPtr" : "UInt", PtrP:=Ptr . "*"
  win:=DllCall("GetDesktopWindow", Ptr)
  hDC:=DllCall("GetWindowDC", Ptr,win, Ptr)
  mDC:=DllCall("CreateCompatibleDC", Ptr,hDC, Ptr)
  ;-------------------------
  VarSetCapacity(bi, 40, 0), NumPut(40, bi, 0, "int")
  NumPut(w, bi, 4, "int"), NumPut(-h, bi, 8, "int")
  NumPut(1, bi, 12, "short"), NumPut(bpp, bi, 14, "short")
  ;-------------------------
  if hBM:=DllCall("CreateDIBSection", Ptr,mDC, Ptr,&bi
    , "int",0, PtrP,ppvBits, Ptr,0, "int",0, Ptr)
  {
    oBM:=DllCall("SelectObject", Ptr,mDC, Ptr,hBM, Ptr)
    DllCall("BitBlt", Ptr,mDC, "int",0, "int",0, "int",w, "int",h
      , Ptr,hDC, "int",x, "int",y, "uint",0x00CC0020|0x40000000)
    DllCall("RtlMoveMemory", Ptr,Scan0, Ptr,ppvBits, Ptr,Stride*h)
    DllCall("SelectObject", Ptr,mDC, Ptr,oBM)
    DllCall("DeleteObject", Ptr,hBM)
  }
  DllCall("DeleteDC", Ptr,mDC)
  DllCall("ReleaseDC", Ptr,win, Ptr,hDC)
}

MCode(ByRef code, hex)
{
  ListLines, Off
  bch:=A_BatchLines
  SetBatchLines, -1
  VarSetCapacity(code, StrLen(hex)//2)
  Loop, % StrLen(hex)//2
    NumPut("0x" . SubStr(hex,2*A_Index-1,2),code,A_Index-1,"uchar")
  Ptr:=A_PtrSize ? "UPtr" : "UInt"
  DllCall("VirtualProtect", Ptr,&code, Ptr
    ,VarSetCapacity(code), "uint",0x40, Ptr . "*",0)
  SetBatchLines, %bch%
  ListLines, On
}

base64tobit(s)
{
  ListLines, Off
  Chars:="0123456789+/ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    . "abcdefghijklmnopqrstuvwxyz"
  SetFormat, IntegerFast, d
  StringCaseSense, On
  Loop, Parse, Chars
  {
    i:=A_Index-1, v:=(i>>5&1) . (i>>4&1)
      . (i>>3&1) . (i>>2&1) . (i>>1&1) . (i&1)
    s:=StrReplace(s,A_LoopField,v)
  }
  StringCaseSense, Off
  s:=SubStr(s,1,InStr(s,"1",0,0)-1)
  s:=RegExReplace(s,"[^01]+")
  ListLines, On
  return, s
}

bit2base64(s)
{
  ListLines, Off
  s:=RegExReplace(s,"[^01]+")
  s.=SubStr("100000",1,6-Mod(StrLen(s),6))
  s:=RegExReplace(s,".{6}","|$0")
  Chars:="0123456789+/ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    . "abcdefghijklmnopqrstuvwxyz"
  SetFormat, IntegerFast, d
  Loop, Parse, Chars
  {
    i:=A_Index-1, v:="|" . (i>>5&1) . (i>>4&1)
      . (i>>3&1) . (i>>2&1) . (i>>1&1) . (i&1)
    s:=StrReplace(s,v,A_LoopField)
  }
  ListLines, On
  return, s
}

ASCII(s)
{
  if RegExMatch(s,"\$(\d+)\.([\w+/]+)",r)
  {
    s:=RegExReplace(base64tobit(r2),".{" r1 "}","$0`n")
    s:=StrReplace(StrReplace(s,"0","_"),"1","0")
  }
  else s=
  return, s
}

; You can put the text library at the beginning of the script,
; and Use Pic(Text,1) to add the text library to Pic()'s Lib,
; Use Pic("comment1|comment2|...") to get text images from Lib

Pic(comments, add_to_Lib=0)
{
  static Lib:=[]
  if (add_to_Lib)
  {
    re:="<([^>]*)>[^$]+\$\d+\.[\w+/]{3,}"
    Loop, Parse, comments, |
      if RegExMatch(A_LoopField,re,r)
        Lib[Trim(r1)]:=r
    Lib[""]:=""
  }
  else
  {
    text:=""
    Loop, Parse, comments, |
      text.="|" . Lib[Trim(A_LoopField)]
    return, text
  }
}

PicN(number)
{
  return, Pic(Trim(RegExReplace(number,".","$0|"),"|"))
}

FindTextOCR(nX, nY, nW, nH, err1, err0, Text, Interval=5)
{
  OCR:="", Right_X:=nX+nW
  While (ok:=FindText(nX, nY, nW, nH, err1, err0, Text))
  {
    ; For multi text search, This is the number of text images found
    Loop, % ok.MaxIndex()
    {
      ; X is the X coordinates of the upper left corner
      ; and W is the width of the image have been found
      i:=A_Index, x:=ok[i].1, y:=ok[i].2
        , w:=ok[i].3, h:=ok[i].4, comment:=ok[i].5
      ; We need the leftmost X coordinates
      if (A_Index=1 or x<Left_X)
        Left_X:=x, Left_W:=w, Left_OCR:=comment
    }
    ; If the interval exceeds the set value, add "*" to the result
    OCR.=(A_Index>1 and Left_X-Last_X>Interval ? "*":"") . Left_OCR
    ; Update nX and nW for next search
    x:=Left_X+Left_W, nW:=(Right_X-x)//2, nX:=x+nW, Last_X:=x
  }
  Return, OCR
}


/***** C source code of machine code *****

int __attribute__((__stdcall__)) PicFind(
  int mode, int c, int n, unsigned char * Bmp
  , int Stride, int sx, int sy, int sw, int sh
  , char * text, int * s1, int * s0
  , int err1, int err0, int w1, int h1, int * allpos)
{
  int o, i, j, k, x, y, w, h, ok=0;
  int r, g, b, rr, gg, bb, e1, e0, len1, len0, max;
  w=sw-w1+1; h=sh-h1+1; k=sy*Stride+sx*4;
  // Generate Lookup Table
  o=len1=len0=0;
  for (y=0; y<h1; y++)
  {
    for (x=0; x<w1; x++)
    {
      j=y*Stride+x*4;
      if (text[o++]=='1')
        s1[len1++]=j;
      else
        s0[len0++]=j;
    }
  }
  max=len1>len0 ? len1 : len0;
  // Color Mode
  if (mode==0)
  {
    for (y=0; y<h; y++)
    {
      for (x=0; x<w; x++)
      {
        o=y*Stride+x*4+k; e1=err1; e0=err0;
        j=o+c; rr=Bmp[2+j]; gg=Bmp[1+j]; bb=Bmp[j];
        for (i=0; i<max; i++)
        {
          if (i<len1)
          {
            j=o+s1[i]; r=Bmp[2+j]-rr; g=Bmp[1+j]-gg; b=Bmp[j]-bb;
            if (r<0) r=-r; if (g<0) g=-g; if (b<0) b=-b;
            if (r+g+b>n && (--e1)<0) goto NoMatch1;
          }
          if (i<len0)
          {
            j=o+s0[i]; r=Bmp[2+j]-rr; g=Bmp[1+j]-gg; b=Bmp[j]-bb;
            if (r<0) r=-r; if (g<0) g=-g; if (b<0) b=-b;
            if (r+g+b<=n && (--e0)<0) goto NoMatch1;
          }
        }
        allpos[ok++]=(sy+y)<<16|(sx+x);
        if (ok>=1024) goto Return1;
        NoMatch1:
        continue;
      }
    }
    goto Return1;
  }
  // Gray Threshold Mode
  c=(c+1)*1000;
  for (y=0; y<sh; y++)
  {
    for (x=0; x<sw; x++)
    {
      o=y*Stride+x*4+k;
      Bmp[3+o]=Bmp[2+o]*299+Bmp[1+o]*587+Bmp[o]*114<c ? 1:0;
    }
  }
  k=k+3;
  for (y=0; y<h; y++)
  {
    for (x=0; x<w; x++)
    {
      o=y*Stride+x*4+k; e1=err1; e0=err0;
      for (i=0; i<max; i++)
      {
        if (i<len1 && Bmp[o+s1[i]]!=1 && (--e1)<0) goto NoMatch2;
        if (i<len0 && Bmp[o+s0[i]]!=0 && (--e0)<0) goto NoMatch2;
      }
      allpos[ok++]=(sy+y)<<16|(sx+x);
      if (ok>=1024) goto Return1;
      NoMatch2:
      continue;
    }
  }
  Return1:
  return ok;
}

*/
