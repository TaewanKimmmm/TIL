# classList.toggle vs classList.add

**간단 요약: toggle은 있으면 remove, 없으면 add 하는 것.**


```javascript
if (hide) {
    el.classList.add('hidden');
} else {
    el.classList.remove('hidden');
}
```
위 코드는 아래와 같다.
```javascript
el.classList.toggle('hidden', hide);
```


```javascript
// returns undefined
classList.add('myClass'); 
classList.remove('myClass'); 

// returns true if class was added, false if it wasn't for some reason
var was_added = classList.toggle('myClass', true) === true;

// returns false if class was removed, true if it wasn't removed for
var was_removed = classList.toggle('myClass', false) === false;
```

