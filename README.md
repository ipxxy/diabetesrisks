<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LOOK BUILDER AI PRO</title>

<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">

<style>
:root{
--primary:#7a5cff;
--secondary:#00e0ff;
--dark:#0a0a12;
--dark2:#12121f;
--card:#1c1c2b;
}

*{margin:0;padding:0;box-sizing:border-box;-webkit-user-select:none;user-select:none}

body{
font-family:'Inter',sans-serif;
background:
radial-gradient(circle at 15% 20%, rgba(122,92,255,0.25), transparent 40%),
radial-gradient(circle at 85% 80%, rgba(0,224,255,0.2), transparent 40%),
linear-gradient(135deg,var(--dark),var(--dark2));
color:white;
overflow-x:hidden;
}

h1,h2{font-family:'Orbitron',sans-serif}
.hidden{display:none}

#entryScreen{
position:fixed;
inset:0;
display:flex;
justify-content:center;
align-items:center;
gap:120px;
z-index:9999;
background:inherit;
transition:opacity .6s ease;
}

.gender-card{
width:260px;
height:260px;
border-radius:30px;
border:2px solid rgba(122,92,255,0.5);
display:flex;
justify-content:center;
align-items:center;
font-family:'Orbitron';
font-size:20px;
cursor:pointer;
transition:.4s;
background:rgba(255,255,255,0.02);
backdrop-filter:blur(10px);
}

.gender-card:hover{
transform:scale(1.05);
box-shadow:0 0 40px var(--secondary);
}

.menu-btn{
position:fixed;
top:25px;
left:25px;
font-size:32px;
cursor:pointer;
z-index:5000;
}

.overlay{
position:fixed;
inset:0;
background:rgba(0,0,0,0.6);
backdrop-filter:blur(8px);
opacity:0;
pointer-events:none;
transition:.3s;
z-index:4000;
}

.overlay.active{
opacity:1;
pointer-events:all;
}

.side-menu{
position:fixed;
top:0;
left:-360px;
width:340px;
height:100vh;
background:linear-gradient(180deg,#111122,#0d0d18);
padding:120px 40px;
transition:.4s ease;
box-shadow:15px 0 60px rgba(0,0,0,0.6);
border-top-right-radius:30px;
border-bottom-right-radius:30px;
z-index:4500;
overflow-y:auto;
}

.side-menu.active{
left:0;
}

.side-menu h3{
font-size:26px;
margin-bottom:40px;
background:linear-gradient(90deg,var(--primary),var(--secondary));
-webkit-background-clip:text;
-webkit-text-fill-color:transparent;
}

.side-menu p{
margin:18px 0;
color:#aaa;
cursor:pointer;
transition:.3s;
font-size:17px;
letter-spacing:1px;
padding:8px 10px;
border-radius:8px;
}

.side-menu p:hover{
color:white;
background:rgba(122,92,255,0.15);
transform:translateX(6px);
}

header{
text-align:center;
padding:120px 20px 60px;
}

header h1{
font-size:54px;
letter-spacing:8px;
background:linear-gradient(90deg,var(--primary),var(--secondary),var(--primary));
background-size:200% auto;
-webkit-background-clip:text;
-webkit-text-fill-color:transparent;
animation:gradientMove 6s linear infinite;
}

@keyframes gradientMove{
0%{background-position:0%}
100%{background-position:200%}
}

.counter{
margin-top:30px;
color:var(--secondary);
font-weight:600;
}

section{
padding:100px 8%;
}

.category-title{
font-size:40px;
margin-bottom:60px;
position:relative;
letter-spacing:4px;
}

.category-title::after{
content:"";
position:absolute;
bottom:-15px;
left:0;
width:140px;
height:5px;
border-radius:10px;
background:linear-gradient(90deg,var(--primary),var(--secondary));
}

.items{
display:grid;
grid-template-columns:repeat(5,1fr);
gap:50px 30px;
}

.item-wrapper{
text-align:center;
}

.item{
background:var(--card);
border-radius:30px;
overflow:hidden;
cursor:pointer;
transition:.4s;
border:2px solid rgba(122,92,255,0.4);
}

.item img{
width:100%;
height:360px;
object-fit:cover;
display:block;
pointer-events:none;
}

.item:hover{
transform:translateY(-12px);
box-shadow:0 25px 60px rgba(122,92,255,0.6);
}

.item.selected{
outline:3px solid var(--secondary);
box-shadow:0 0 35px var(--secondary);
}

.item-title{
font-family:'Orbitron';
font-size:16px;
margin-top:18px;
opacity:0;
transform:translateY(-10px);
transition:.3s;
}

.item-wrapper:hover .item-title,
.item.selected + .item-title{
opacity:1;
transform:translateY(0);
}

.ai-box{
text-align:center;
padding:120px 20px;
}

.ai-btn{
margin-top:30px;
padding:18px 60px;
border:none;
border-radius:60px;
font-size:16px;
font-family:'Orbitron';
background:linear-gradient(90deg,var(--primary),var(--secondary));
color:white;
cursor:pointer;
letter-spacing:2px;
box-shadow:0 0 25px rgba(122,92,255,.5);
}

.loader{
margin-top:40px;
height:6px;
width:300px;
background:#222;
border-radius:20px;
overflow:hidden;
display:none;
margin-left:auto;
margin-right:auto;
}

.loader-bar{
height:100%;
width:0%;
background:linear-gradient(90deg,var(--primary),var(--secondary));
animation:load 2.2s forwards;
}

@keyframes load{
0%{width:0%}
100%{width:100%}
}

.models{
display:flex;
justify-content:center;
margin-top:60px;
opacity:0;
transition:opacity .8s ease;
}

.models.active{
opacity:1;
}

.model-box img{
height:420px;
border-radius:30px;
}

footer{
margin-top:200px;
padding:120px 0;
text-align:center;
background:#0f0f18;
color:#888;
font-size:14px;
}
</style>
</head>

<body>

<div id="entryScreen">
<div class="gender-card" onclick="enterSite()">I AM MAN</div>
<div class="gender-card" onclick="enterSite()">I AM WOMAN</div>
</div>

<div class="menu-btn hidden" onclick="toggleMenu()">⋮</div>
<div class="overlay" id="overlay" onclick="closeMenu()"></div>

<div class="side-menu" id="menu">
<h3>MENU</h3>
<p onclick="goHome()">MAIN PAGE</p>
<p onclick="scrollToSection('tshirts')">T-SHIRTS</p>
<p onclick="scrollToSection('shorts')">SHORTS</p>
<p onclick="scrollToSection('sneakers')">SNEAKERS</p>
<p onclick="openPage('Saved Looks')">Saved Looks</p>
<p onclick="openPage('More')">More</p>
<p onclick="openPage('Telegram')">Telegram</p>
<p onclick="openPage('Instagram')">Instagram</p>
<p onclick="closeMenu()">Close</p>
</div>

<div id="mainContent" class="hidden">

<header>
<h1>LOOK BUILDER AI PRO</h1>
<div class="counter">Selected items: <span id="count">0</span> / 3</div>
</header>

<section id="tshirts">
<h2 class="category-title">T-SHIRTS</h2>
<div class="items">

<div class="item-wrapper">
<div class="item" data-type="tshirt" data-link="https://www.wildberries.ru/catalog/243327753/detail.aspx" onclick="handleClick(this)">
<img src="https://basket-16.wbbasket.ru/vol2433/part243327/243327753/images/big/1.webp">
</div>
<div class="item-title">Nike TOP DAWG</div>
</div>

<div class="item-wrapper"><div class="item" data-type="tshirt" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">T-Shirt 2</div></div>
<div class="item-wrapper"><div class="item" data-type="tshirt" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">T-Shirt 3</div></div>
<div class="item-wrapper"><div class="item" data-type="tshirt" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">T-Shirt 4</div></div>
<div class="item-wrapper"><div class="item" data-type="tshirt" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">T-Shirt 5</div></div>

</div>
</section>

<section id="shorts">
<h2 class="category-title">SHORTS</h2>
<div class="items">

<div class="item-wrapper">
<div class="item" data-type="shorts" data-link="https://www.wildberries.ru/catalog/26869256/detail.aspx?targetUrl=EX" onclick="handleClick(this)">
<img src="https://basket-02.wbcontent.net/vol268/part26869/26869256/images/big/1.webp">
</div>
<div class="item-title">Short 1</div>
</div>

<div class="item-wrapper"><div class="item" data-type="shorts" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Short 2</div></div>
<div class="item-wrapper"><div class="item" data-type="shorts" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Short 3</div></div>
<div class="item-wrapper"><div class="item" data-type="shorts" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Short 4</div></div>
<div class="item-wrapper"><div class="item" data-type="shorts" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Short 5</div></div>

</div>
</section>

<section id="sneakers">
<h2 class="category-title">SNEAKERS</h2>
<div class="items">

<div class="item-wrapper">
<div class="item" data-type="sneaker" data-link="https://www.wildberries.ru/catalog/384368190/detail.aspx?utm_source=ya_direct&utm_medium=cpc&utm_campaign=c:706995844/n:y_int_wb_dsa_search_cpc_wear_ru/a:1901405389784653232/g:5715839897/p:205715839897/st:search/s:none/ap:no/post:premium/pos:1/ret:205715839897/r:20041/at:205715839897/d:desktop/ct:type1/y:15013524302623932415&etext=2202.6t4UIP6VdnCsFfurZAWS7Xs5ZEkdGejcd36CELmWEN6er2Iev37BFz16Ur9V5d8TbWtnYmhyb3RnaXF2Znh2dQ.789acd9b74f6b5da39d38f213273e11c3e210735&yclid=15013524302623932415" onclick="handleClick(this)">
<img src="https://basket-22.wbbasket.ru/vol3843/part384368/384368190/images/big/5.webp">
</div>
<div class="item-title">Кроссовки adidas</div>
</div>

<div class="item-wrapper"><div class="item" data-type="sneaker" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Sneaker 2</div></div>
<div class="item-wrapper"><div class="item" data-type="sneaker" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Sneaker 3</div></div>
<div class="item-wrapper"><div class="item" data-type="sneaker" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Sneaker 4</div></div>
<div class="item-wrapper"><div class="item" data-type="sneaker" onclick="handleClick(this)"><img src="https://via.placeholder.com/400x500"></div><div class="item-title">Sneaker 5</div></div>

</div>
</section>

<section class="ai-box">
<h2 class="category-title">AI LOOK GENERATOR</h2>
<button class="ai-btn" onclick="generateLook()">GENERATE</button>

<div class="loader" id="loader"><div class="loader-bar"></div></div>

<div id="models" class="models">
<div class="model-box">
<img id="resultImage" src="https://ru.pinterest.com/pin/68749633029/">
<button class="ai-btn" onclick="saveLook()">SAVE LOOK</button>
<button class="ai-btn" onclick="downloadImage()">SAVE IMAGE</button>
</div>
</div>

</section>

<footer>
LOOK BUILDER AI PRO © 2026
</footer>

</div>

<script>

let selectedItems={}
let lastClickTime=0
let lastElement=null

function enterSite(){
entryScreen.style.opacity='0'
setTimeout(()=>{
entryScreen.style.display='none'
document.querySelector('.menu-btn').classList.remove('hidden')
mainContent.classList.remove('hidden')
},600)
}

function toggleMenu(){menu.classList.add('active');overlay.classList.add('active')}
function closeMenu(){menu.classList.remove('active');overlay.classList.remove('active')}
function goHome(){
location.reload()
}

function scrollToSection(id){
document.getElementById(id).scrollIntoView({behavior:'smooth'})
closeMenu()
}

function openPage(title){
mainContent.innerHTML=`
<header>
<h1>${title}</h1>
</header>
`
closeMenu()
}

function updateCounter(){
count.innerText=Object.keys(selectedItems).length
}

function handleClick(el){

const type=el.dataset.type
const now=Date.now()

if(el.dataset.link && lastElement===el && (now-lastClickTime)<250){
window.open(el.dataset.link,'_blank')
lastClickTime=0
lastElement=null
return
}

if(el.classList.contains('selected')){
el.classList.remove('selected')
delete selectedItems[type]
updateCounter()
lastClickTime=now
lastElement=el
return
}

if(Object.keys(selectedItems).length>=3){
alert('Select only 3 items')
return
}

if(selectedItems[type]){
document.querySelectorAll(`.item[data-type='${type}']`).forEach(i=>i.classList.remove('selected'))
}

selectedItems[type]=true
el.classList.add('selected')

updateCounter()

lastClickTime=now
lastElement=el
}

function generateLook(){

if(Object.keys(selectedItems).length!==3){
alert('Select 3 items first')
return
}

loader.style.display='block'
models.classList.remove('active')

setTimeout(()=>{

loader.style.display='none'
models.classList.add('active')

// ГЕНЕРАЦИЯ №1 (ФУТБОЛКА 1 + ШОРТЫ 1 + КРОССОВКИ 1)
const tshirt = document.querySelector(".item[data-type='tshirt'].selected")
const shorts = document.querySelector(".item[data-type='shorts'].selected")
const sneakers = document.querySelector(".item[data-type='sneaker'].selected")

if(
tshirt?.dataset.link.includes("243327753") &&
shorts?.dataset.link.includes("26869256") &&
sneakers?.dataset.link.includes("384368190")
){
resultImage.src = "https://i.imgur.com/pXtExDG.jpeg"
}

},2200)

}

function saveLook(){alert('Look saved')}
function downloadImage(){
const link=document.createElement('a')
link.href=resultImage.src
link.download='look.png'
link.click()
}

</script>

</body>
</html>
