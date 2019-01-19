
HTML:
``` html
<input type="text" style="ime-mode:disabled;"  onkeypress="keyPress()" />
``` 
Javascript:
``` javascript
function keyPress() {    
	var keyCode = event.keyCode;    
	if ((keyCode >= 48 && keyCode <= 57)){    
		event.returnValue = true;    
	} else {    
		event.returnValue = false;    
	}
}    
```

ime-mode:disabled 表示仅能输入英文字符集
onpaste="return false;" 表示不能粘贴
