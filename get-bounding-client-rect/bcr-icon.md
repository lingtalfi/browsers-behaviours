Browsers getBoundingClientRect's left/top - width/height measurements
===========================================================================
2016-10-04


In this document, I have 3 buttons, one of which containing an icon from google material icon.

I take screenshots of the browser on my desktop and I compare the values that I see
in the screenshots against the values that the browsers's getBoundingClientRect method return.


The browsers I tested were:
- ffox49
- chrome53
- chrome55
- opera39
- safari9
- ie11 (via virtual box)
- edge13 (via virtual box)


Assets
----------

- The html file used for the test is the following

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8"/>
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Testing getBoundingClientRect with a button containing an icon</title>
	<link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">	

	<style>
		
		body{
			margin:0;	
			padding:0;	
			background: blue;
		}
		
		button {
			position: relative; 
			overflow: hidden;
			border: none;
			outline: none;
			cursor: pointer;
			background: #89669b;
			color: white;
			padding: 18px 60px;
			border-radius: 2px;
			font-size: 24px;
		}
		
		
		*:focus{
			outline: 5px solid red;
		}
	</style>
</head>

<body>


<button id="btn1" class="ripple">
	<i class="material-icons">favorite</i>
</button>

<button id="btn2" class="ripple" data-ripple-color="red">
	Red ripple
</button>

<hr>


<button id="btn3" class="coderipple">
	Orange ripple from code
</button>


<script>
	var btn1 = document.getElementById('btn1');
	var btn2 = document.getElementById('btn2');
	var btn3 = document.getElementById('btn3');
	console.log(btn1.getBoundingClientRect());
	console.log(btn2.getBoundingClientRect());
	console.log(btn3.getBoundingClientRect());
	
	
</script>


</body>
</html>
```

- The screenshots are in the screenshots folder





Results
-----------

The format for the result is: left/top - width/height.
There are 3 buttons (1,2,3).



photoshop - ie11:
- 1: 0/0 - 144/66 
- 2: 148/2  - 229/64 
- 3: 0/84 - 379/63

ie11:
- 1: 0/0 - 194.6199951171875/63.599998474121094
- 2: 198.6199951171875/0 - 229.39999389648437/63.599998474121094
- 3: 0/81.5999984741411 - 378.8199768066406/63.599998474121094

----------

photoshop - edge13:
- 1: 0/0 - 144/65
- 2: 148/2  - 229/63
- 3: 0/83 - 379/64


edge13:
- 1: 0/0 - 144/65.47999572753906
- 2: 148/1.8799999952316284 - 229.39999389648437/63.59999465942383
- 3: 0/83.47999572753906 - 378.8199768066406/63.600006103515625


----------

photoshop - opera39:
- 1: 0/0 - 144/65
- 2: 148/1 - 224/64 
- 3: 0/83 - 371/64


opera39:
- 1: 0/0 - 194.625/64
- 2: 198.625/0 - 224.328125/64
- 3: 0/82 - 371.171875/64




----------
photoshop - safari9:
- 1: 2/2 - 144/65
- 2: 152/3 - 228/64 
- 3: 0/89 - 379/64



safari9:
- 1: 2/2 - 208.359375/64
- 2: 218.359375/2 - 227.859375/64
- 3: 2/88 - 379.265625/64



----------
photoshop - ffox49:
- 1: 0/0 - 150/68
- 2: 154/0 - 230/68  
- 3: 0/86 - 377/68


ffox49:
- 1: 0/0 - 150/68
- 2: 154/0 - 230.3333282470703/68
- 3: 0/86 - 377.16668701171875/68


----------

photoshop - chrome53/chrome55:
- 1: 0/0 - 144/65  
- 2: 148/1  - 224/64  
- 3: 0/83 - 371/64


chrome53/chrome55:
- 1: 0/0 - 194.625/64
- 2: 198.625/0 - 224.328125/64
- 3: 0/82 - 371.171875/64





My conclusion
-----------------

Firefox and edge13's getBoundingClientRect method's return values represent "exactly" what you have in the photoshop visual.

Ie11 and chromium based browsers (chrome, safari, opera) seems to add 50+px to the width of the button which contain the icon.

- getBoundingClientRect in the case of an icon inside a button is not safe (not cross browser)
- some browsers have inconsistencies between the visual appearance and the values returned by the getBoundingClientRect method



  
Working around
--------------------

I filed the chrome bug to chromium:

- https://bugs.chromium.org/p/chromium/issues/detail?id=652626
- https://bugs.chromium.org/p/chromium/issues/detail?id=654260


The second time I noticed that wrapping the code into a setTimeout with enough delay "resolves" the problem.

A delay of 10ms is not enough for chrome, but 50 was in local.

Then putting this bug online: http://codepen.io/lingtalfi/pen/dpmOPp,
I noticed that even Firefox had the problem.

So, this probably means that browsers take some time to make that computation.

Note: this isn't related to the load event (I just tried and it failed in safari).
So the code below fails in safari, that's because it's dependent on time, not on the load event.

```js
	window.addEventListener("load", function(event) {
		// now values might be consistent? => Actually not in local safari (and maybe other browsers)...
	});
```











