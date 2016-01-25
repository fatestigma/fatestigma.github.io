+++
comments = true
date = "2015-05-13T19:49:02+08:00"
draft = false
image = ""
share = true
tags = ["AppleScript", "Mac"]
title = "AppleScript 按键内容"

+++



## 按键
### Keystroke

**keystroke** text [ using modifiers down ]

```applescript
tell application "System Events" to keystroke "i"
```

如果需要修饰键则添加可选语句：`using command down`；
如果需要多个修饰键，使用 `using {command down, shift down}`。

### Key code
**key code** integer or list of integer [using modifiers down]

```applescript
key code 37 using command down
```

每个键都有其对应的 Key code，可以使用 AppStore 中的 Key Codes 应用来查看各个按键对应的 Key Code。

### Modifier Down

## 按键检测
存在的问题：

1. AppleScript 中随着 AppleScript Studio 的消失也不具有很多监听器，其中就有按键监听器。
2. AppleScript 本身没有获取当前按键的方法。

解决方案：

1. 对于监听器这里只能采用定时检测的方式来完成对按键的检测。
2. 按键获取方面则好在可以使用 ObjC-Bridge。

```applescript
using framework "Foundation"
tell application "System Events"
    repeat
        delay 0.05
        set flag to my BWAND((current application's NSEvent's modifierFlags()),(current application's NSCommandKeyMask)) > 0
        if flag is true then exit repeat
    end repeat
end tell
```

1. 第一行调用 ObjC 的 Foundation 框架。
2. 使用 `current application's NSEvent's modifierFlags()` 获取当前按键（修饰键）的值，并且其二进制值的各位代表不同修饰键。
3. 使用 `current application's NSCommandKeyMask()` 方法获取 Command 键对应位为 1 的二进制值。
4. 使用自定义函数 `my BWAND()` 来比较当前按键和 Command 键的按位与运算，如果其结果非零，则代表 Command 键被按下。
5. 退出循环。

附 `my BWAND` 代码：

```applescript
on BWAND(__int1, __int2)
	set theResult to 0
	repeat with bitOffset from 30 to 0 by -1
		if __int1 div (2 ^ bitOffset) = 1 and __int2 div (2 ^ bitOffset) = 1 then
			set theResult to theResult + 2 ^ bitOffset
		end if
		set __int1 to __int1 mod (2 ^ bitOffset)
		set __int2 to __int2 mod (2 ^ bitOffset)
	end repeat
	return theResult as integer
end BWAND
```


