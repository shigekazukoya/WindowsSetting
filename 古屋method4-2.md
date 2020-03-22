# 古屋method4-2
2020-03-21 AutoHotKeyの素晴らしさを感じたため

# AutoHotKey再び
AutoHotKeyを舐めていました｡
これは素晴らしいソフトウェアです｡

ワンショットモディファイア とローマ字の再変換がとてもよかったです 参考にしたリンクをご覧ください

[US配列で悠々自適AutoHotkeyScripts](https://blog.phoshigaki.net/2018/10/usautohotkeyscripts.html)
[誰かの役に立つかもしれない実験メモ AutoHotkeyでローマ字の再変換](https://yuruaki.blog.fc2.com/blog-entry-41.html)
[誰かの役に立つかもしれない実験メモ Win + L を含む入力があってもロックを回避する方法](https://yuruaki.blog.fc2.com/blog-entry-70.html)
[AutoHotkeyで短い連続入力を認識させる方法](https://rcmdnk.com/blog/2017/11/05/computer-windows-autohotkey/)
[(AutoHotkey)(IMEの制御をする 編) - もらかなです。](https://morakana.hatenadiary.org/entry/20080213/1202876561)


## おまけ:
関連付けは変えておいたほうがいいかと
[スクリプトの編集環境を整える - hoge](https://sites.google.com/site/agkh6mze/howto/environment)
[Windows10 右クリックの編集の関連付けを変更する - 備忘録](https://kagasu.hatenablog.com/entry/2017/07/26/214410)
[AutoHotkey - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=slevesque.vscode-autohotkey)

## おまけ2:
ahkを管理者権限で実行しておくと管理者権限で開いているツールでも使えます｡
自分はタスクスケジューラーから起動するようにしてます｡
[管理者権限が必要な常駐アプリはタスクスケジューラでスタートアップさせる](https://www.losttechnology.jp/Win7/taskscheduler.html)


## 以下､ 現在のAutoHotKeyとの戦闘状況

F13 F14 F15が出てくるので key置いときます

変換キー → F13
Windowsキー → F14
カナ/かなキー → F15

<!-- 画像 -->

```
;方向キー
F13 & H::Send,{Blind}{Left}
F13 & J::Send,{Blind}{Down}
F13 & K::Send,{Blind}{Up}
F13 & L::Send,{Blind}{Right}

;Alt+方向キー
LAlt & H::Send,{Blind}{Lalt}{Left}
LAlt & J::Send,{Blind}{Lalt}{Down}
LAlt & K::Send,{Blind}{Lalt}{Up}
LAlt & L::Send,{Blind}{Lalt}{Right}

;Win+l 回避の為
f14::LWin
#l::
	Send,{Win Up}
	run,rundll32.exe user32.dll`,LockWorkStation
return

;BS Delete
F13::Send,{BackSpace}
+F13::Send {Delete}

;Home End PgUp Pgdn
F13 & T::Send,{Home}
F13 & E::Send,{End}
F13 & U::Send,{PgUp}
F13 & D::Send,{Pgdn}

F13 & M::Send,{APPSKEY}
F13 & w::Send,^{F4}
F13 & O::Send,{APPSKEY}{a} ; 管理者権限で実行
F13 & 2::Send,{F2}
F13 & y::Send,{BackSpace}


; 英語に変換
Lalt::
WinGet, vcurrentwindow, ID, A
    vimestate := DllCall("user32.dll\SendMessageA", "UInt", DllCall("imm32.dll\ImmGetDefaultIMEWnd", "Uint", vcurrentwindow), "UInt", 0x0283, "Int", 0x0005, "Int", 0)
if (vimestate == 1) {
	Send,{sc029}
}
return

; 日本語変換
F15::
WinGet, vcurrentwindow, ID, A
    vimestate := DllCall("user32.dll\SendMessageA", "UInt", DllCall("imm32.dll\ImmGetDefaultIMEWnd", "Uint", vcurrentwindow), "UInt", 0x0283, "Int", 0x0005, "Int", 0)
if (vimestate == 1) {
Return
}
Send,{sc029}
return
;ローマ字の再変換
+F15::
WinGet, vcurrentwindow, ID, A
    vimestate := DllCall("user32.dll\SendMessageA", "UInt", DllCall("imm32.dll\ImmGetDefaultIMEWnd", "Uint", vcurrentwindow), "UInt", 0x0283, "Int", 0x0005, "Int", 0)
if (vimestate == 1) {
Return
}
	ClipSaved := ClipboardAll
	c:=""
	e:=0
	while(c!=Clipboard or c==""){
		c:=Clipboard
		Send,+{Left}
		Clipboard =
		Send,^c
		ClipWait,1
		sl:=StrLen(Clipboard)
		if(sl==0){
			e:=1
			break
		} else if(RegExMatch(SubStr(Clipboard,1,1),"[\-\~]")){
			Send,+^{Left}
		} else if(RegExMatch(Clipboard,"[^0-9a-zA-Z\-\~]")){
			if(sl==1 or (sl==2 and RegExMatch(Clipboard,"[\r\n]"))){
				e:=1
				Send,+{Right}
			} else {
				Send,+{Right}
				Clipboard =
				Send,^c
				ClipWait,1
			}
			break
		} else {
			Send,+{Right}+^{Left}
		}
		Clipboard =
		Send,^c
		ClipWait,1
	}
		Send,{sc029}	;ここにIMEをオンにする処理
	if(e==0){
		Send,%Clipboard%
	}
	Clipboard := ClipSaved
	ClipSaved =
return

;Spaceを押したとき
$*Space::
    if (isSpaceRepeat == true)    ;キーリピートしているかどうか
    {
        if (A_PriorKey != "Space") ;Space長押し中の他キー押し下げを検出
        {
            KeyWait, Space
            Send {Blind}{ShiftUp}       ;Shiftをリリース
            isSpaceRepeat := false
            Return
        }
        else Return
    }
    Send {Blind}{ShiftDown}     ;Shiftを押し下げ
    isSpaceRepeat := true
Return
;Spaceを離したとき
$*Space up::
    Send {Blind}{ShiftUp}       ;Shiftをリリース
    isSpaceRepeat := false
    if (A_PriorKey == "Space"){     ;Space単押しを検出
        Send {Blind}{Space}     ;Spaceを入力
    }
Return

; Vim用
#IfWinActive, ahk_exe Code.exe,
~j up::
Input, jout, I T0.1 V L1, {j}
if(ErrorLevel == "EndKey:J"){
	WinGet, vcurrentwindow, ID, A
    vimestate := DllCall("user32.dll\SendMessageA", "UInt", DllCall("imm32.dll\ImmGetDefaultIMEWnd", "Uint", vcurrentwindow), "UInt", 0x0283, "Int", 0x0005, "Int", 0)

    If (vimestate==1)
	{
    SendInput, {BackSpace 2}{Esc 3}
	}
}
Return

$*~LShift::Send {Blind}{Shift}
LShift up::
    if (A_PriorKey == "LShift")
    {
        Send, {Esc}
    }
Return

```