<template>
<v-btn v-if="!started" @click="start">start</v-btn>
<svg v-if="started" height=400 width=400>
<rect x=0 y=0 width=400 height=400 fill="white">
</rect>
<line v-for="c,i in meiro"
:x1="i%10*40"
:y1="(i-i%10)/10*40"
:x2="i%10*40+40"
:y2="(i-i%10)/10*40"
stroke="black"
:opacity="c.south? 0:1"
>
</line>
<line v-for="c,i in meiro"
:x1="i%10*40"
:y1="(i-i%10)/10*40+40"
:x2="i%10*40+40"
:y2="(i-i%10)/10*40+40"
stroke="black"
:opacity="c.north? 0:1"
>
</line>
<line v-for="c,i in meiro"
:x1="i%10*40"
:y1="(i-i%10)/10*40"
:x2="i%10*40"
:y2="(i-i%10)/10*40+40"
stroke="black"
:opacity="c.west? 0:1"
>
</line>
<line v-for="c,i in meiro"
:x1="i%10*40+40"
:y1="(i-i%10)/10*40+40"
:x2="i%10*40+40"
:y2="(i-i%10)/10*40"
stroke="black"
:opacity="c.east? 0:1"
>
</line>

<circle :cx="40*position.x+20" :cy="40*position.y+20" r=20 fill="red">         </circle>
<circle :cx="40*esa.x+20" :cy="40*esa.y+20" r=20 fill="yellow">         </circle>
<circle :cx="40*esa2.x+20" :cy="40*esa2.y+20" r=20 fill="pink">         </circle>
<circle :cx="40*esa3.x+20" :cy="40*esa3.y+20" r=20 fill="skyblue">         </circle>
<text v-if="gameclear" :x="150" :y="150" font-size=40 >It was eaten!</text>
<text v-if="win" :x="200" :y="200" font-size=80 >win</text>
<text v="start" :x="1" :y="15" font-size=20  ></text>
<text v="goal" :x="360" :y="390" font-size=20 >goal</text>
</svg>
</template>

<script>

export default {
data() {
return{
position:{x:0,y:0},
esa:{x:8,y:8},
esa2:{x:4,y:4},
esa3:{x:5,y:5},
started:false,     
meiro:null,
gameclear:false,
timer:null,
win:false,     
};       
},
mounted(){
window.addEventListener("keydown",e=>this.kbd(e)); 
},
methods:{
esamove(){
let random =Math.floor(Math.random()*4);
console.log(random);    
let i=this.esa.y*10+this.esa.x;
let c=this.meiro[i];
let random2 =Math.floor(Math.random()*4);
let i2=this.esa2.y*10+this.esa2.x;
let c2=this.meiro[i2];
let random3 =Math.floor(Math.random()*4);
let i3=this.esa3.y*10+this.esa3.x;
let c3=this.meiro[i3];
if(this.gameclear)
remove();

if(random==0 && c.south) {
this.esa={x:this.esa.x,y:this.esa.y-1};
}
else if(random==1 && c.north) {
this.esa={x:this.esa.x,y:this.esa.y+1};
}
else if(random==2 && c.west) {
this.esa={x:this.esa.x-1,y:this.esa.y};
}
else if(random==3 && c.east) {
this.esa={x:this.esa.x+1,y:this.esa.y};
}
if(this.position.x==this.esa.x && this.position.y==this.esa.y){
this.gameclear=true;
clearInterval(this.timer);

}
if(random2==0 && c2.south) {
this.esa2={x:this.esa2.x,y:this.esa2.y-1};
}
else if(random2==1 && c2.north) {
this.esa2={x:this.esa2.x,y:this.esa2.y+1};
}
else if(random2==2 && c2.west) {
this.esa2={x:this.esa2.x-1,y:this.esa2.y};
}
else if(random2==3 && c2.east) {
this.esa2={x:this.esa2.x+1,y:this.esa2.y};
}
if(this.position.x==this.esa2.x && this.position.y==this.esa2.y){
this.gameclear=true;
clearInterval(this.timer);

}
if(random3==0 && c3.south) {
this.esa3={x:this.esa3.x,y:this.esa3.y-1};
}
else if(random3==1 && c3.north) {
this.esa3={x:this.esa3.x,y:this.esa3.y+1};
}
else if(random3==2 && c3.west) {
this.esa3={x:this.esa3.x-1,y:this.esa3.y};
}
else if(random3==3 && c3.east) {
this.esa3={x:this.esa3.x+1,y:this.esa3.y};
}
if(this.position.x==this.esa3.x && this.position.y==this.esa3.y){
this.gameclear=true;
clearInterval(this.timer);
}
},

start(){
this.timer=setInterval(this.esamove, 100);
let t=maze(10);
this.meiro=makeGraph(t,10);
console.log(this.meiro);     
this.started=true;
},
kbd(e){
if(this.gameclear)
return;
let i=this.position.y*10+this.position.x;
let c=this.meiro[i];
if(e.keyCode==39&&this.position.x<9 && c.east){
this.position.x+=1;  
}
else if(e.keyCode==37&&this.position.x>0 && c.west){
this.position.x-=1;
}
else if(e.keyCode==38&&this.position.y>0 && c.south){
this.position.y-=1;
}
else if(e.keyCode==40&&this.position.y<9 && c.north){
this.position.y+=1;
}
if(this.position.x==9&&this.position.y==9){
this.win=true;
clearInterval(this.timer);
}
}
}
}





//右39
//左37
//上38
//下40

function maze(n){
let next = [];
let inTree = [];
for (let i = 0; i < n*n; i++){
next.push(-1);
inTree.push(false);
}
inTree[0] = true;
for (let i = 1; i < n*n; i++){
let u = i;
while (!inTree[u]){
console.log(i2coord(u,n));
next[u] = randomSuccessor(u,n);
u = next[u];
}
u = i;
while (!inTree[u]){
inTree[u] = true;
u = next[u];
}
}
return next;
}

function i2coord(u,n){
const x = u%n;
const y = (u-x)/n;
return {x: x, y: y};
}

function c2index(x,y,n){
return(y*n + x);
}

function inside(x,y,n){
if (x >= 0 && x < n && y >= 0 && y < n){
return true;
}
else{
return false;
}
}

function randomSuccessor(u,n){
const neighbors = [];
const c = i2coord(u,n);
const dx = [0,1,0,-1];
const dy = [1,0,-1,0];
for (let i = 0; i < 4; i++){
if (inside(c.x + dx[i], c.y + dy[i],n)){
neighbors.push(c2index(c.x + dx[i], c.y+dy[i],n));
}
}
const l = neighbors.length;
return neighbors[Math.floor(Math.random()*l)];
}

function makeGraph(next,n){
let neighbors = [];
for (let i = 0; i < next.length; i++){
neighbors.push({east:false, north:false, west:false,
south:false});
}
for (let i = 1; i < next.length; i++){
if (next[i]-i == 1){
neighbors[i].east=true;
neighbors[next[i]].west=true;
}
else if (next[i]-i == -1){
neighbors[i].west=true;
neighbors[next[i]].east=true;
}
else if (next[i]-i == n){
neighbors[i].north=true;
neighbors[next[i]].south=true;
}
else{
neighbors[i].south=true;
neighbors[next[i]].north=true;
}
}
return neighbors;
}





</script>
