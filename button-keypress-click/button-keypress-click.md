Button keypress click
============================
2016-10-03






Test 1
---------

The test file is button-keypress-click.html.


Let's see how different browsers behave when we add the following listeners to a button that has focus: 
click, keypress, keydown, keyup.



Firefox 49, Chrome 53, Opera 39, ie11, edge13:
Press enter:
- keydown
- keypress
- click
- keyup

Press space:
- keydown
- keypress
- keyup
- click


Notice that enter and space have different behaviours if the key is held down for a moment:

If enter is held down:

- keydown
- keypress
- click
- keydown
- keypress
- click
- ...(repeats the three events as long as enter is held down)
- keyup


If space is held down:

- keydown
- keypress
- keydown
- keypress
- ...(repeats the two events as long as space is held down)
- keyup
- click


In other words, enter clicks multiple times (as long as the key is held down) whereas space clicks only 
once when the space key is released.




Test 2
---------

The test file is button-keypress-prevent-click.html.

Now let's see what happens if we try to prevent the click event from happening.
In this test we use preventDefault on the keypress, keydown and keyup event, alternately.

Here are the results.




### Button with keypress prevents default


Firefox 49, Chrome 53, Safari 9, Opera 39, ie11, edge13:


Press enter:

- keydown
- keypress
- keyup

Press space:

- keydown
- keypress
- keyup
- click

Holding down enter:
- keydown
- keypress
- keydown
- keypress
- ...(repeats the two events as long as enter is held down)
- keyup


Holding down space:
- keydown
- keypress
- keydown
- keypress
- ...(repeats the two events as long as enter is held down)
- keyup
- click


### Button with keydown prevents default


Firefox 49, Chrome 53, Safari 9, Opera 39, ie11, edge13:


Press enter:

- keydown
- keyup

Press space:

- keydown
- keyup
- click (firefox only)

Holding down enter:
- keydown
- keydown
- ...(repeats the events as long as enter is held down)
- keyup


Holding down space:
- keydown
- keydown
- ...(repeats the events as long as enter is held down)
- keyup
- click (firefox only)



### Button with keyup prevents default


Firefox 49, Chrome 53, Safari 9, Opera 39, ie11, edge13:


Press enter:

- keydown
- keypress
- click
- keyup

Press space:

- keydown
- keypress
- keyup
- click (ie11 and edge13 only)


Holding down enter:
- keydown
- keypress
- click
- keydown
- keypress
- click
- ...(repeats the three events as long as enter is held down)
- keyup


Holding down space:
- keydown
- keypress
- keydown
- keypress
- ...(repeats the two events as long as enter is held down)
- keyup
- click (ie11 and edge13 only)
