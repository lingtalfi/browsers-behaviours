Listeners execution order
===================
2016-10-06



According to this post: http://stackoverflow.com/questions/9512551/the-order-of-multiple-event-listeners,
DOM3 event spec [...] introduces the requirement that they be fired in order of registration.


I wrote a little test that I executed in the browsers I care about.


Results
-----------



- Firefox49: ok (in order of registration)
- Chrome53: ok 
- Safari9: ok 
- Opera40: ok 
- Ie11 via virtualbox on mac host: ok 
- Edge13 via virtualbox on mac host: ok 


Important note: ie11 did not understand the dispatchEvent (see the source code of the test below),
I had to click the link manually.




Test code
-------------
Here is the source code for the test:



```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
	<meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Listeners test</title>
</head>

<body>


<div id="target">click me</div>


<script>
	var target = document.getElementById('target');
	target.addEventListener('click', function(e){
	    console.log("listener 1");
	});
	target.addEventListener('click', function(e){
	    console.log("listener 2");
	});
	
	
	target.dispatchEvent(new Event("click")); // did not work in ie11, had to click manually
</script>


</body>
</html>
```

