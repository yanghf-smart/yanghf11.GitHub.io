# 脚本说明

```javascript
 <script language="JavaScript">

var xo=0;
var yo=0;
var Ox = -1;
var Oy = -1;

var rad = Math.PI/180;
var maxY = 400;
var color = "red";

function print(str) {
document.write(str)
}

function orgY(y) {
return maxY-y
}
function outPlot(x,y,w,h) {
print('<span style="position:absolute;left:'+x+';top:'+y+';height:'+h+';width:'+w+';font-size:1px;background-color:'+color+'"></span>')
}

function Plot(x,y) {
outPlot(x,y,1,1)
if(Ox>=0 || Oy>=0) {
ShowLine(Ox,Oy,x-Ox,y-Oy)
}
Ox = x
Oy = y
}

function ShowLine(x,y,w,h) {
if(w<0) {
x += w
w = Math.abs(w)
}
if(h<0) {
y += h
h = Math.abs(h)
}
if(w<1) w=1
if(h<1) h=1
outPlot(x,y,Math.round(w),Math.round(h))
}

function LineTo(x,y) {
Line(xo,yo,x,y)
}

function sign(n) {
if(n>0)
return 1
if(n<0)
return -1
return n
}

function Line(x1,y1,x2,y2) {
x2 = Math.round(x2)
y2 = Math.round(y2)
xo = x2
yo = y2
y1 = orgY(y1)
y2 = orgY(y2)
var str = ""
var i=0

var x = x1
var y = y1
dx = Math.abs(x2-x1)
dy = Math.abs(y2-y1)
s1 = sign(x2-x1)
s2 = sign(y2-y1)

if(dx==0 || dy==0) {
ShowLine(x1,y1,x2-x1,y2-y1)
return
}

if(dx>dy) {
temp = dx
dx = dy
dy = temp
key = 1
}else
key = 0
e = 2*dy-dx

for(i=0;i<dx;i++) {
px = 0
py = 0
Plot(x,y)
while(e>=0) {
if(key==1) {
x += s1
px += s1
}else {
y += s2
py += s2
}
e = e-2*dx
}
if(key==1)
y += s2
else
x += s1
e = e+2*dy
}
} 

function MoveTo(x,y) {
Ox = Oy = -1
xo = Math.round(x)
yo = Math.round(y)
}

// 圆
function Cir(x,y,r) {
MoveTo(x+r,y)
for(i=0;i<=360;i+=5) {
LineTo(r*Math.cos(i*rad)+x,r*Math.sin(i*rad)+y)
}
}
// 弧形
function Arc(x,y,r,a1,a2) {
MoveTo(r*Math.cos(a1*rad)+x,r*Math.sin(a1*rad)+y)
for(i=a1;i<=a2;i++) {
LineTo(r*Math.cos(i*rad)+x,r*Math.sin(i*rad)+y)
}
}
// 扇形
function Pei(x,y,r,a1,a2) {
MoveTo(x,y)
for(var i=a1;i<=a2;i++) {
LineTo(r*Math.cos(i*rad)+x,r*Math.sin(i*rad)+y)
}
LineTo(x,y)
}
// 弹出扇形
function PopPei(x,y,r,a1,a2) {
dx = r*Math.cos((a1+(a2-a1)/2)*rad)/10
dy = r*Math.sin((a1+(a2-a1)/2)*rad)/10
x += dx
y += dy
MoveTo(x,y)
for(var i=a1;i<=a2;i++) {
LineTo(r*Math.cos(i*rad)+x,r*Math.sin(i*rad)+y)
}
LineTo(x,y)
}

// 矩形
function Rect(x,y,w,h) {
MoveTo(x,y)
LineTo(x+w,y)
LineTo(x+w,y+h)
LineTo(x,y+h)
LineTo(x,y)
}

// 准星
function zhunxing(x,y) {
var ox = xo
var oy = yo
var oColor = color
color = "#000000"
Line(x-5,y,x+6,y)
Line(x,y-6,x,y+5)
print('<span style="position:absolute;font-size:10pt;left:'+(x+5)+';top:'+orgY(y+5)+';">['+x+','+y+']</span>')
color = oColor
xo = ox
yo = oy
}
// 标注
function biaozhuStr(x,y,s) {
return '<span style="position:absolute;font-size:10pt;left:'+x+';top:'+orgY(y)+';">'+s+'</span>'
}
function biaozhu(x,y,s,t) {
var ox = xo
var oy = yo
var oColor = color
point = "p01.gif"
if(t==1) {
print(biaozhuStr(x-5,y+6,"·"+s))
}
if(t==2) {
print(biaozhuStr(xo+x*Math.cos(y*rad)-10,yo+x*Math.sin(y*rad),s))
}
color = oColor
xo = ox
yo = oy
}
</script>
</head>

<body>
<table border="0" width="100%">
<tr>
<td width="100%" style="font-family: 方正舒体; font-size: 18pt; color: #FF0000" class="t1">JavaScript绘图</td>
</tr>
</table>

# 基本图形
color = "maroon"
Cir(50,40,20)
Arc(100,40,20,60,120)
Pei(150,60,40,240,300)
Rect(200,20,40,40)

// 折线图
color = "#FF0000"
var jd = new Array(
203,232,277,223,271,234,273,284,276,250,267,280
)
MoveTo(30,jd[0]-40)
biaozhu(xo,yo,jd[0])
for(i=1;i<jd.length;i++) {
LineTo(i*30+30,jd[i]-40)
biaozhu(xo,yo,jd[i],1)
}
color = "#C0C0C0"
Line(30,140,i*30+30,140)
Line(30,140,30,260)

// 饼图
color = "#00FF00"
var gc = new Array(
150,120,200,180,180
)
var s = 0
var m = 0
var n = 0
for(i=0;i<gc.length;i++) {
s +=gc[i]
if(gc[i] > m) {
m = gc[i]
n = i
}
}
var k = s/360
var mm = 0
var a =0
for(i=0;i<gc.length;i++) {
b = Math.round((gc[i]+mm)/k)
if(i==n)
PopPei(600,150,100,a,b)
else
Pei(600,150,100,a,b)
biaozhu(60,a+(b-a)/2,Math.round(gc[i]/s*100)+"%",2)
mm = mm+gc[i]
a = b
}

// 十字标注
MoveTo(280,20)
zhunxing(xo,yo)

```
