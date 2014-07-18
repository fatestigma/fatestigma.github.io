---
layout: post
title: "在终端中快速进入当前目录"
date: 2014-07-07 11:34:57 +0800
comments: true
categories: Workflows
tags: Tools AppleScript
---

>作为一名程序猿，终端可以说是平时最常用的应用了。
但是每次到一个工程路径下都要change directory半天，非常费时，有时候还一不小心就把窗口关掉了，
又要敲长长的路径，虽然有Tab键的自动补全，还是相当费时费事。

因此在.profile中添加了一个直接到当前目录的指令`cdf`:

{% codeblock lang:bash %}
function cdf() # cd to finder's front's window's path
{
	path="`osascript -e 'tell application "Finder" to set myname to POSIX path of (target of window 1 as alias)' 2>/dev/null`"
	if [	 -n "$path" ]; then
		echo "cd to $path"
		cd "$path"
	else
		echo "no finder window finded"
	fi
}
{% endcodeblock %}

但是这个还是要Finder切换到iTerm，懒到极致的我于是又写了下面这个AppleScript：

<!--more-->

{% codeblock Quick Access lang:applescript %}
on searchReplace(origStr, searchStr, replaceStr)
	set old_delim to AppleScript's text item delimiters
	set AppleScript's text item delimiters to searchStr
	set origStr to text items of origStr
	set AppleScript's text item delimiters to replaceStr
	set origStr to origStr as string
	set AppleScript's text item delimiters to old_delim
	return origStr
end searchReplace

on alfred_script()
	set selected to false # if file selected
	
	tell application "Finder"
		set myPath to POSIX path of (target of window 1 as alias)
		if (selection ≠ {}) then
			set selected to true
			set myFile to (selection as alias)
			set myFiletype to name extension of myFile
			set myFilename to name of myFile
		end if
	end tell
	# get raw name without extension
	if selected then set myFileRawName to searchReplace(myFilename, "." & myFiletype, "")
	
	tell application "iTerm"
		set _term to (make new terminal)
		tell _term
			launch session "Default"
			set _session to current session
		end tell
		
		tell _session
			write text "cd " & myPath
			write text "clear"
			if selected then
				# Add your process here...
				# Makefile
				if myFilename = "Makefile" or ¬
					myFilename = "makefile" then write text "make"
				# Python
				if myFiletype = "py" then write text "python " & myFilename
				# C and C++
				if myFiletype = "c" or ¬
					myFilename = "cpp" then
					write text "clang " & myFilename & " -o " & myFileRawName
					write text "./" & myFileRawName
				end if
				# Java
				if myFiletype = "java" then
					write text "javac " & myFilename
					write text "java " & myFileRawName
				end if
				# LaTeX
				if myFiletype = "tex" or myFiletype = "ltx" then ¬
					write text "xelatex -shell-escape -8bit" & myFileRawName
			end if
		end tell
	end tell
end alfred_script
{% endcodeblock %}

并给它添加新的功能，在有选中的源文件时就顺便编译运行了。

当然这个是针对我个人习惯使用的是iTerm，而对于使用原生的Terminal只需要做点更改就可以了。

{% codeblock Quick Access with Terminal lang:applescript %}
on searchReplace(origStr, searchStr, replaceStr)
	set old_delim to AppleScript's text item delimiters
	set AppleScript's text item delimiters to searchStr
	set origStr to text items of origStr
	set AppleScript's text item delimiters to replaceStr
	set origStr to origStr as string
	set AppleScript's text item delimiters to old_delim
	return origStr
end searchReplace

on alfred_script()
	set selected to false # if file selected
	
	tell application "Finder"
		set myPath to POSIX path of (target of window 1 as alias)
		if (selection ≠ {}) then
			set selected to true
			set myFile to (selection as alias)
			set myFiletype to name extension of myFile
			set myFilename to name of myFile
		end if
	end tell
	# get raw name without extension
	if selected then set myFileRawName to searchReplace(myFilename, "." & myFiletype, "")
	
	set cmd to "cd " & myPath & "; clear; "
	if selected then
		# Add your process here...
		# Makefile
		if myFilename = "Makefile" or ¬
			myFilename = "makefile" then set cmd to cmd & "make;"
		# Python
		if myFiletype = "py" then set cmd to cmd & "python " & myFilename & ";"
		# C and C++
		if myFiletype = "c" or ¬
			myFilename = "cpp" then
			set cmd to cmd & "clang " & myFilename & " -o " & myFileRawName & ";"
			set cmd to cmd & "./" & myFileRawName & ";"
		end if
		# Java
		if myFiletype = "java" then
			set cmd to cmd & "javac " & myFilename & ";"
			set cmd to cmd & "java " & myFileRawName & ";"
		end if
		# LaTeX
		if myFiletype = "tex" or myFiletype = "ltx" then ¬
			set cmd to cmd & "xelatex -shell-escape -8bit" & myFileRawName & ";"
	end if
	
	tell application "Terminal"
		do script cmd
	end tell
end alfred_script
{% endcodeblock %}

以上的都是主要针对Alfred的Workflows写的，当然可以拷到Automator上，
然后创建Service添加快捷键就可以了。

