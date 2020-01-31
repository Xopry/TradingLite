# TradingLite - LitScript Documentation

>The idea of [LitScript](https://tradinglite.com/guides/litscript) is to be as close to [PineScript](https://www.tradingview.com/pine-script-docs/en/v4/index.html) as possible, we intend to make the experience of switching from one to another as effortless as possible.
>
> This documentation will serve you to some extent.
> 
> ### Table Of Contents
>
> - [PineScript Quickstart Script That Works With LitScript](#pinescript-quickstart-script-that-works-with-litscript)
> - [Function Declaration Examples](#function-declaration-examples)
> - - [single line functions](#single-line-functions)
> - - [multi-line functions](#multi-line-functions)
> - [Arrays Support, Loops, Overlay - Example](#arrays-support-loops-overlay---example)
> - - [overlay](#overlay)
> - - [non-series](#non-series)
> - - [crazy visualizations](#crazy-visualizations)
> - [Supported Functionality](#supported-functionality)
> - - [`study()`](#study)
> - - [`input()`](#input)
> - - [series](#series)
> - - [multiple definitions per single line](#multiple-definitions-per-single-line)
> - - [`var`](#var)
> - [Plot Built-In Functions](#plot-built-in-functions)
> - - [`plot()`](#plot)
> - - [`plotbar()`](#plotbar)
> - - [`histogram()`](#histogram)
> - - [`plotshape()`](#plotshape)
> - - [`fill()`](#fill)
> - - [`bgcolor()`](#bgcolor)
> - - [stepped linechart](#stepped-linechart)
> - - [linewidth setting](#linewidth-setting)
> - - [offset](#offset)
> - - [transp](#transp)
> - [Arrays & Conditional Statements](#arrays--conditional-statements)
> - - [arrays](#arrays)
> - - [`if` `else` statements](#if-else-statements)
> - - [shorthand conditional expressions](#shorthand-conditional-expressions)
> - [Rest Of The Syntax](#rest-of-the-syntax)
> - - [`for()`](#for)
> - - [`while()`](#while)
> - - [`switch()`](#switch)
> - [Operators](#operators)
> - - [or](#or)
> - - [and](#and)
> - - [equal to](#equal-to)
> - - [not equal to](#not-equal-to)
> - - [others](#others)
> - [Series Math Functions](#series-math-functions)
> - [Built-In Math Functions](#built-in-math-functions)
> - [Colors](#colors)
> - - [built-in color functions](#built-in-color-functions)
> - - [custom colors](#custom-colors)
> - - [list of static colors](#list-of-static-colors)
> - - [color blending](#color-blending)
> - [Currently Available Data Series](#currently-available-data-series)
> - - [time](#time)
> - - [price](#price)
> - - [volume](#volume)
> - - [volume delta](#volume-delta)
> - - [cumulative volume delta](#cvd)
> - - [open interest](#open-interest)
> - - [bids asks](#bids--asks)
> - - - [bid/ask sum](#bidask-sum)
> - - - [bid/ask range sum bands](#bidask-range-sum-bands)
> - [Built-In Variables](#built-in-variables)
> - - [`na` support](#na-support)
> - [Additional Built-In Functions](#additional-built-in-functions)
> - [List of shapes for `plotshape()`](#list-of-shapes-for-plotshape)
> - [LitScript compilation and execution errors](#litscript-compilation-and-execution-errors)
> - [Auto-Complete And Syntax Highlighting](#auto-complete-and-syntax-highlighting)
> - [Script Vault](#script-vault)
> - - [private scripts](#private-scripts)
> - - [unlisted scripts](#unlisted-scripts)
> - - [public scripts](#public-scripts)
> - [Script Manager](#script-manager)
> - [Litscript Editor](#litscript-editor)
> - [Official Indicators](#official-indicators)
> - - [free indicators](#free-indicators)
> - - - [volume](#volume)
> - - - [vpvr](#vpvr)
> - - - [heatmap](#heatmap)
> - - - [heatmap lens](#heatmap-lens)
> - - [subscription indicators](#subscription-indicators)
> - - - [open interest built-in](#open-interest-built-in)
> - [Layer Settings](#layer-settings)
> - - [layer system](#layer-system)
> - - [auto-scale](#auto-scale)
> - - [cross-hair](#cross-hair)]
> - [Other Features](#other-features)
---

## [PineScript](https://www.tradingview.com/pine-script-docs/en/v4/index.html) Quickstart Script That Works With [LitScript](https://tradinglite.com/guides/litscript)

```js
study("My Script")
fast = 12, slow = 26
fastMA = ema(close, fast)
slowMA = ema(close, slow)
macd = fastMA - slowMA
signal = sma(macd, 9)
plot(macd, color=#00f)
plot(signal, color=#ff0)
```


## Function Declaration Examples

> Explicit variable type definition for functions
```js
func MyFunc(mySeriesVar:series, myConstantVar:var) => mySeriesVar * myConstantVar`
```

### single line functions

```js
study("My Script")
func zigzag(val:series) => val[2] == 0 ? 50 : 0
test = zigzag(test)
plot(test, color=#0ff)
```

### multi-line functions

```js
study("My Script")
func mipmap(val:series){
    temp = val[1]
    if (temp == -10)
        temp = -20
    else
        temp = -10
    return temp
}
hiya = mipmap(hiya)
plot(hiya, color=#ff0)
```


## Arrays Support, Loops, Overlay - Example

### overlay

```js
study("array overlaid example", overlay=true)
levelsA = [32] // array allows us to generate 32 charts easily
levelsB = [10]

// simple for loop
for (i=0;i<32;i++) {
  append(levelsA, sma(open,i*2)+i*20, lighten(#509,i*5))  // append is how we fill the data for arrays plots
}

for (i=0;i<100;i+=10) { // step by 10
  append(levelsB, sma(open,i*2)-i*20, darken(#B09,i*2))
}

plot(levelsA)
plot(levelsB)
```

### non-series

> Arrays ( non-series ), `darken`,` lighten` 

[![](https://media.discordapp.net/attachments/607177769402630167/607211628316065792/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/607211628316065792/unknown.png)

### crazy visualizations 

> Array plots can also have various colors for some crazy visualizations like these.

[![](https://media.discordapp.net/attachments/607177769402630167/607215807520571422/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/607215807520571422/unknown.png)


## Supported Functionality

### `study()`

> The function sets a number of study properties.

```js
study("my script name", overlay = true) // name of the script + overlay it on the last existing pane ( true ) or new pane ( false / default behavior )
```

### `input()`

> `input("Field Name", 0)` . This allows [public scripts](#public-scripts) to be used without touching the code. _Only limited to numbers, booleans for now_
> `input("Color", green)` _( can't be modified for now )_
> `input("Source", open)` _( can't be modified for now )_

```js
myValue = input("My Label", 123)
```

### series

```js
series = open + 10 // any defined value can become a series variable, you can access it by doing series[1]
```

### multiple definitions per single line

```js
seriesA = open, seriesB=close // multiple definitions can be stacked on one line using a comma
```

### `var`

> Special **var** keyword used for assigning and one-time initializing of the variable (not the same as defining values directly).

> example  define variables that stay in memory ( just like in [PineScript](https://www.tradingview.com/pine-script-docs/en/v4/index.html))

```js
var myValue = 5 // (taken from pinescript) this allows the value of the variable to be automatically saved between bars
```

## Plot Built-In Functions

### `plot()`

> Plots a series of data on the chart in the form of a line.
> Plots with no color set will default to white ( e.g. `plot(open)` )

```js
plot(series, color=#FFF) // plot a linechart
```

### `plotbar()`

> Plots ohlc bars on the chart.

```js
plotbar(open,high,low,close) // plot candles
```

### `histogram()`

> Plots a series of data on the chart in the form of a histogram.

```js
histogram(price, color=#af5, trasp=50)
```

### `plotshape()`

> Plots visual shapes on the chart. List of all available shapes [here](#list-of-shapes-for-plotshape).

```js
plotshape(price, "icon", size, color=#af5)
```

`plotshape(value, "icon", size, color)` _(experimental)_

[![](https://media.discordapp.net/attachments/607177769402630167/649701004513771555/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/649701004513771555/unknown.png)


### `fill()`

```js
fill(price1, price2, color=#af5, trasp=50)
```

### `bgcolor()`

```js
bgcolor(#af5, trasp=50)
```

### stepped linechart



### linewidth setting

`linewidth` support for plots!

```js
study("MyScript")
mycolor = blend(#f00,#0f0,(open-open[1])/50)
plot(open, lineType=1, linewidth=10, color=mycolor)
```

[![](https://media.discordapp.net/attachments/607177769402630167/607210627764584465/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/607210627764584465/unknown.png)

### offset

`offset` parameter to `plot`, `fill`

### transp

`transp` ( transparency ) parameter for `histogram`, `fill`, `bgcolor`

[![](https://media.discordapp.net/attachments/607177769402630167/651774244132225035/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/651774244132225035/unknown.png)


## Arrays & Conditional Statements


### arrays

> array/matrix plots
> max limit - 64 plots

```js
array = [size] // size is a positive value, represents the amount of charts to create
append(array, value, #FF00FF) // fill the first chart's array data
```

### `if` `else` statements

```js
if (condition1) {
} else if (condition2) {
} else { }
// optional scopes for single line statements
if (test==2) test=1
else test=2
```

### shorthand conditional expressions

```js
test = (condition) ? true : false // (ternary conditional operator)
test = iff(condition,true,false) // if ... then ... else ... // less efficient than ?:
```


## Rest Of The Syntax
It is pretty close to JavaScript, with some minor differences and restrictions

> Loops are limited to 1000 iterations for your own safety.

### `for()`
```js
for(i=0;i<10;i++) { /* code */ }  // no need to specify type of iterator ( i )
for(j=1;j>0;j-=0.05) { /* code */ }  // reverse loop with fractions
```

### `while()`

```js
while(condition) { /* code */ } // this is self explanatory
```
### `switch()`

```js
switch(somevariable){ // switch statements !
case 0:
     /* code */
    break
case 1:
case 2:
     /* code */
    break
default:
     /* code */
    break
}
```

---

## Operators

### or
```js
or, ||
```
### and

```js
and, &&
```
### equal to

```js
==
```

### not equal to

```js
!=
```

### others

```js
*, /, %, |, ~, ^
++,--
+=,-=
// ...pretty much every other operator javascript supports
```


## Series Math Functions

```js
// --- Series math functions ---
lowest(series, range) // lowest value ( for a given number of candles back )
highest(series, range) // highest value ( for a given number of candles back )
sum(series, range) // sum all values ( for a given number of candles back )
sma(series, range) // simple moving average ( for a given number of candles back )
ema(series, range) // exponential moving average ( for a given number of candles back )
stdev(series, range) // standard deviation ( for a given number of candles back )
mom(series, range) // momentum ( for a given number of candles back )
rma(series, alpha) // rolling moving average series function
```


## Built-In Math Functions

```js
min(value1, value2, value3, ...) // returns smallest value of all arguments passed
max(value1, value2, value3, ...) // returns highest value of all arguments passed

radians(degrees) // transforms degrees into radians
degrees(radians) // transforms radians into degrees
isfinite(value) // returns false if value is Infinity or NaN
isnan(value) // returns true if value is NaN
sqrt(value) // returns squareroot of value
abs(value) // returns absolute value
ceil(value) // returns round number up
floor(value) // returns round number down
round(value) // returns rounds number 
trunc(value) // returns the integer part of a number by removing any fractional
exp(value) //  returns Euler's number power of value
sign(value) // returns 1 if value is positive, -1 if negative, 0 if it's 0
sin(value) // returns sinus
cos(value) // returns cosinus
tan(value) // returns tangent
asin(value) // returns arcsine (in radians)
acos(value) // returns arccosine (in radians)
atan(value) // returns arctangent (in radians)

log(value) // returns the base-e logarithm of value
log2(value) // returns the base-2 logarithm of value
log10(value) // returns  the base-10 logarithm of value
pow(value, pow) // returns value power of pow
```


## Colors



### built-in color functions

```js
brightness(color, amount) // adjusts color's brightness by a value ( 0-255 )
darken(color, amount) // alias to brightness(color, -amount)
lighten(color, amount) // alias to brightness(color, amount)
blend(colorA, colorB, amount) //  blends between two colors by amount ( 0.0-1.0 )
```

### custom colors

```js
test = #F0b // can be defined using 3 character hexadecimal notation
test = #FF00BB // can be defined using 6 character hexadecimal notation
test = magenta // can be defined using a static color name ( see list below )
```

### list of static colors

```js
black, white, red, lime, blue, yellow, cyan, magenta, silver, gray, maroon, olive, green, purple, teal, navy
```

### color blending

> Sometimes some random math such as (close-open) can give good enough results. Everything negative is the first color, everything positive the second color.
> Blend color value requires a normalized value between 0-1 instead of -1 to 1

```js
study("MyScript")
mycolor = blend(#f00,#0f0,(open-open))
plot(open, lineType=1, linewidth=10, color=mycolor)
```


## Currently Available Data Series

### time

```js
time
```

### price

```js
open, high, low, close
ohlc4, hlc3, hl2 // or averages
```

```js
study("Price Candles", overlay=false)
plotbar(open, high, low, close) // candle plots
```

### volume

```js
vbuy, vsell
```

```js
study("Volume", overlay=false)
v = (vbuy + vsell)
histogram(v, color = close<open ? red : green, title="Volume")
```

### volume delta

```js
study("Volume Delta", overlay=false)
vd = (vbuy - vsell)
histogram(vd, color = vd<0 ? red : green, title="Volume Delta")
```

### cvd

> cumulative volume delta 

```js
study("CVD (Cumulative Volume Delta)")
vd = vbuy-vsell, cvd = cum(vd)
plot(cvd,color=#00ff00,title="CVD")
```

### open interest

> Open Interest has [a built-in layer](#open-interest-built-in).
> It is also available in LitScript. Data series: `oi_open, oi_high, oi_low, oi_close`.
> **Open Interest** is available for **all subscribers**.
> It updates automatically.

```js
study("Open Interest in LitScript", overlay=false)
plotbar(oi_open, oi_high, oi_low, oi_close)
```

```js
oi_open, oi_high, oi_low, oi_close
oi_ohlc4, oi_hlc3, oi_hl2 // or averages
```

[![](https://media.discordapp.net/attachments/607177769402630167/643626329064996864/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/643626329064996864/unknown.png)


### bids & asks

> **Bid/Ask Sum accepts range** _( experimental and slow, only for full range )_ ( in %, e.g. `bid_sum(120)` 120% range of its current closing price )

#### bid/ask sum

```js
study("[example] Bid/Ask Sum")
plot(bid_sum(),color=#AF0)
plot(ask_sum(),color=#F30)
```

#### bid/ask range sum bands

```js
study("Bid/Ask Range Sum Bands")

divide = input("Divide by", 7)
asks = [10]
bids = [10]

for (i=1;i<=10;i++) append(asks,ask_sum((i*i)/divide), darken(#FFAA00,i*10))
for (i=1;i<=10;i++) append(bids,-bid_sum((i*i)/divide), darken(#AAFFAA,i*10))

plot(asks)
plot(bids)

plot(ask_sum())
plot(-bid_sum())

plot(0, color=#333) // middle line
```

[![](https://media.discordapp.net/attachments/607177769402630167/668161766043156511/2LjJKqSBTFAAAAAElFTkSuQmCC.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/668161766043156511/2LjJKqSBTFAAAAAElFTkSuQmCC.png)

## Built-In Variables

### `na` support

## Additional Built-In Functions

```js
cum(series) // cumulative value ( like sum but for whole range )

na(value) // same as isnan() checks for NaN value
nz(value) // returns 0 if value is NaN otherwise it returns the value itself

normalize(value, min, max) // transforms value from min/max range to normalized value (0.0-1.0)
denormalize(normalized, min, max) // transforms normalized value (0.0-1.0) to min/max range
lerp(start, end, amount) // same as denormalize(1-amount, start, end)
clamp(value, min, max) // clamps/restricts value to min and max
saturate(value) // same as clamp(value, 0, 1)
maplinear(value, fromStart, fromEnd, toStart, toEnd) // map a value from a range to another range
wrap(value, min, max) // wraps value to min/max range

radians(degrees) // transform degrees to radians
degrees(radians) // transform radians to degrees 

step(a, value)
fmod(a, b)
smoothstep(value, min, max)
```


## List of shapes for `plotshape()`

> `plotshape()` examples [here](#plotshape).

```js
adjust
air
alert
arrow-combo
attach
attention
battery
bell
block
bookmark
bookmarks
bullseye
camera
cancel
cancel-circled
cancel-squared
cd
chart-area
chart-bar
chart-pie
check
circle
circle-empty
circle-thin
clock
cloud
cloud-thunder
cog
database
dot-circled
down
down-bold
down-circled
down-dir
down-open
down-open-big
down-thin
droplet
erase
eye
fast-backward
fast-forward
feather
flag
flash
gauge
heart
heart-empty
help
help-circled
info
info-circled
key
lamp
left
left-bold
left-circled
left-dir
left-open
left-open-big
left-thin
light-down
light-up
link
location
lock
lock-open
magnet
minus
minus-circled
minus-squared
moon
music
note
note-beamed
pause
play
plus
plus-circled
plus-squared
progress-0
progress-1
progress-2
progress-3
quote
record
right
right-bold
right-circled
right-dir
right-open
right-open-big
right-thin
rocket
rss
search
signal
star
star-empty
stop
tag
target
thermometer
thumbs-down
thumbs-up
to-end
to-start
traffic-cone
trophy
up
up-bold
up-circled
up-dir
up-open
up-open-big
up-thin
water
```


## LitScript compilation and execution errors

> Ignore the mystery error numbers. It's unknown if anyone actually managed to trigger those.
> Most important of all: `Open Interest is only available to subscribers`.
> Compiler errors for litscripts are shown on main tab.

Error Codes																																																																				| Meaning
--------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------
<code class="js errcodes">Value unused?																																																	</code>		|	
<code class="js errcodes">00001																																																					</code>		|	
<code class="js errcodes">Historical value of a series variable is read-only!																														</code>		|	
<code class="js errcodes">Constant "${name}" cannot be modified																																					</code>		|	
<code class="js errcodes">Matrix "${name}" cannot be redefined																																					</code>		|	
<code class="js errcodes">Matrix "${name}" size cannot exceed [2-${maxSize}] size																												</code>		|	
<code class="js errcodes">Can't assign "${name}" before initialization																																	</code>		|	
<code class="js errcodes">00005																																																					</code>		|	
<code class="js errcodes">Error 00010																																																		</code>		|	
<code class="js errcodes">"${name}()" can only be used in the root scope																																</code>		|	
<code class="js errcodes">Shape "${shape}" doesn't exist																																								</code>		|	
<code class="js errcodes">append(matrix, value, color) : argument mismatch!																															</code>		|	
<code class="js errcodes">append(matrix, value, color) "${matrix}" : argument needs to be a matrix																			</code>		|	
<code class="js errcodes">Study title can't be empty																																										</code>		|	
<code class="js errcodes">Study title can't be longer than 100 characters!																															</code>		|	
<code class="js errcodes">Function "${name}" doesn't exist!																																							</code>		|	
<code class="js errcodes">00006																																																					</code>		|	
<code class="js errcodes">Error 00009																																																		</code>		|	
<code class="js errcodes">Can't declare a function inside a function !																																	</code>		|	
<code class="js errcodes">Variable/function "${name}" cannot be redefined																																</code>		|	
<code class="js errcodes">00008																																																					</code>		|	
<code class="js errcodes">Constant "${name}" cannot be modified																																					</code>		|	
<code class="js errcodes">00101																																																					</code>		|	
<code class="js errcodes">Constant "${name}" cannot be redeclared																																				</code>		|	
<code class="js errcodes">Variable "${name}" cannot be redeclared																																				</code>		|	
<code class="js errcodes">00008																																																					</code>		|	
<code class="js errcodes">Constant "${name}" cannot be modified																																					</code>		|	
<code class="js errcodes">Unexpected INTERNAL decl: ` + stmnt.typ`																																			</code>		|	
<code class="js errcodes">00004																																																					</code>		|	
<code class="js errcodes">Function "${name}" requires arguments!																																				</code>		|	
<code class="js errcodes">Inline series math not supported yet. Temporary solution: Put the argument contents into a separate variable.	</code>		|	
<code class="js errcodes">First argument needs to be a string																																						</code>		|	
<code class="js errcodes">Second argument missing																																												</code>		|	
<code class="js errcodes">Function "${name}" doesn't exist!																																							</code>		|	
<code class="js errcodes">Variable "${name}" doesn't exist																																							</code>		|	
<code class="js errcodes">00007																																																					</code>		|	
<code class="js errcodes">Cannot assign a value in an expression ( did you mean == ?)																										</code>		|	
<code class="js errcodes">Open Interest is only available to subscribers																																</code>		|	















## Auto-Complete And Syntax Highlighting

[![](https://media.discordapp.net/attachments/607177769402630167/607213919173345291/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/607213919173345291/unknown.png)


## Script Vault

> **Script Vault**: the humble beginning of user-made script library
> Make your scripts private/unlisted/public or simply use scripts made by others

#### private scripts

> the script is private and only you can see its source code and edit it.

#### unlisted scripts

> only users with the link can use the script and view its source code and only you can edit it.

#### public scripts

> anyone can see the source code of the script and they can also make a copy of it.

## Script Manager

> LitScript can be saved. 
> Separated window.

## LitScript Editor

> Editor undo / redo history is cleared when changing scripts
> **Multi-Script support** !
> Separated LitScript window.
> The LitScript labels of the panes can be toggled _( hidden by default )_
> Active LitScripts are Sorted by pane order
> _EA+ users:_ - LitScript slots extended to **5**
> LitScripts can have **descriptions**. Descriptions are visible in the **[Vault](#script-vault)**.

[![](https://media.discordapp.net/attachments/607177769402630167/671812350931632129/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/671812350931632129/unknown.png)

[![](https://media.discordapp.net/attachments/607177769402630167/651891812310450176/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/651891812310450176/unknown.png)

> LitScript Editor size is a **fully flexible draggable window**.

[![](https://media.discordapp.net/attachments/607177769402630167/663792602679607347/tradingresizee.gif?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/663792602679607347/tradingresizee.gif)


## Official Indicators

> Official Indicators tab in `Indicators` window

### free indicators
	
> free built-in indicators that can be used by anyone

#### volume

> free built-in volume indicator that can be used by anyone
> it has the volume delta incorporated

#### vpvr

> volume profile visible range
> **Value Area for VPVR**

#### heatmap

#### heatmap lens

### subscription indicators

#### open interest built-in

## Layer Settings

[![](https://media.discordapp.net/attachments/607177769402630167/660618972500328480/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/660618972500328480/unknown.png)


### layer system

> LitScript can create panes.
> Multi-pane features (add, remove and move layers)
> You can have more than 2 panes!

[![](https://media.discordapp.net/attachments/607177769402630167/632279592727609344/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/632279592727609344/unknown.png)


> Last Price Line option added to all candle layers ( incl. OI ) 
> Panes: Smaller the pane = more price axis ticks will be displayed
> Price Axis: digits separator for larger numbers
> Price Axis: auto-scale padding is now flexible ( smaller panes = smaller padding, more chart less empty space )
> You have to enable `Last Price Line` on main candles manually, it's a layer option

[![](https://media.discordapp.net/attachments/607177769402630167/646076967606681600/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/646076967606681600/unknown.png)


### auto-scale

> Smooth updates when moving multi-pane chart with auto-scale
> ALL plots will be taken in account
> heatmap & vpvr will also auto-scale ( so you'll be able to put it in a separate pane )
>You can auto-scale panes individually, double-click on axis to set auto-scale
> Auto-scale button is only linked to the first pane

[![](https://media.discordapp.net/attachments/607177769402630167/644339532686426122/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/644339532686426122/unknown.png)

### cross-hair

> Cross-hair spans across all panes + candle/volume info is always visible
> Line charts are not cut off at start and end of visible range

[![](https://media.discordapp.net/attachments/607177769402630167/644658253996883978/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/644658253996883978/unknown.png)


## Other Features

- Large values are supported
- NaN or Infinite value plots don't break the price axis
- Block comments color in script editor
- Series functions range ( it acts like pinescript where 1 is current candle instead of 0 )
- **Reactive `Compiling...` message**

[![](https://media.discordapp.net/attachments/607177769402630167/636982629215895552/GLXhQsznjmb1v8B6o3sHb1dgcAAAAASUVORK5CYII.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/636982629215895552/GLXhQsznjmb1v8B6o3sHb1dgcAAAAASUVORK5CYII.png)


- Custom charts layout is persistent
- Script is automatically loaded on start
- Navigation UI drop-down menus are a bit sexier
- LitScript plots are grouped under one layer

[![](https://media.discordapp.net/attachments/607177769402630167/643915649600585758/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/643915649600585758/unknown.png)


[![](https://media.discordapp.net/attachments/607177769402630167/644957015613112341/A1JGQDIemvfAAAAAElFTkSuQmCC.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/644957015613112341/A1JGQDIemvfAAAAAElFTkSuQmCC.png)


- [Layer settings](#layer-settings) are accessible straight from the script editor
- Layer Settings: Add/Remove pane buttons
- Ability to filter scripts in Script Vault
- Max personal scripts limit -  20
- Plot lines have proper size on high-dpi displays
- **Ability to draw on any pane!**
- Add/Remove pane buttons

[![](https://media.discordapp.net/attachments/607177769402630167/652222711363403831/unknown.png?width=680&height=403)](https://cdn.discordapp.com/attachments/607177769402630167/652222711363403831/unknown.png)



- UTF-16 support
- **Maximize Pane** - feature ( + double click shortcut )
- Pane separator lines are visible in screenshots
- **Accurate latency** algorithm, now called RTT
- Network compression enabled


