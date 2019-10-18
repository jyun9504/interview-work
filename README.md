# interview-test

* CSS
---------------------------------------
1. 簡述一下對 Html 語意化的理解：

答：  
使用恰當的 HTML 標籤，讓頁面結構更清晰，讓機器或是人都可以更好的理解，每一個標籤在做什麼。  
對人來說，在維護上可以從標籤名字就了解這個區塊是什麼，例如：header、footer、nav ．．．  
對機器來說，語義化標籤也能幫助SEO，爬蟲會根據標籤名字對內容權重有所不同，例如：strong、em 有強調的語義。  
  
---------------------------------------
2. 請排序以下css權重優先順序
```
A. p { background-color: pink; }  
B. <p style="background-color:green"></p>  
C. body div p { background-color: yellow; }  
D. #blue { background-color: blue; }  
E. .red { background-color: red; }  
F. body p { background-color: white; }  
G. !important  
```

答：  
Ｇ > B > D > E > C > F > A
  
---------------------------------------
2. 說明 box-model 盒子模型

答：  
假設現在有一個 div 標籤，在 CSS 中，會再把它分成好幾個部分，從外到內：  
margin > border > padding > content 這四個區塊，不同瀏覽器可能會有不同的預設 box-model，  
在舊的 IE 把 border 和 padding 包含在 content 裡，所以長度寬度也會算進 border 和 padding 裡。  
使用 box-sizing 屬性 可以改變模式。  
  
---------------------------------------
3. 說明 relative、fixed、absolute 和 static 元件差異?

答：  
position 屬性用來定義現在這個元素的排版定位：  
static：預設  
fixed：根據瀏覽器視窗定位  
relative：相對定位，被設定的元素跟預設的表現一樣，可以用來讓子元素做定位  
absolute：找到上層最近的 relative 做定位，如果沒有就跟瀏覽器視窗定位  
  
---------------------------------------
4. 如何讓元素 水平 + 垂直 置中?  

答：  
1.在子元素上加  
position： absolute;  
top: 50%;  
left: 50%;  
transform: translate(-50%, -50%);  

2.在父元素上加  
display: flex;  
justify-content: center;  
align-items: center;  
  
---------------------------------------
5. 會怎麼佈局切版?

答：  
最外層 container 固定寬度和設定相對定位，裡面分上下兩個區塊，  
上面區塊又分上下 藍色背景 和 頭像名字 兩個區塊，  
藍色背景區塊 設定絕對定位讓 頭像名字 不用以藍色背景為基準，直接跟 container 做 margin-top  
頭像名字區塊 margin-top 一半的藍色區塊高度。  
    
下面區塊只包含四個小方塊，  
小方塊設定 display: inline-block，裡面內容水平垂直置中。  
  
---------------------------------------
* JavaScript
---------------------------------------
1. 說明這段程式碼運作流程,並改造程式碼,使之輸出0-9,寫出你能想到的所有解法

```
for (var i = 0; i< 10; i++){  
    setTimeout(() => {  
        console.log(i);  
    }, 1000)  
}  
```

答：  
因為JavaScript是單執行緒，  
for 迴圈會先執行完，才把所有 setTimeout 放到非同步隊列中，時間到了才一起執行，  
這時候會發現所有console.log(i)都會顯示 10。  

用let取代var，產生塊級作用域  
```
for(let i =0; i< 10; i++){
    setTimeout(() => {
        console.log(i)
    }, 1000)
}
```

用立即執行函式，把i儲存到函式裡  
```
for(var i =0; i< 10; i++){
    (function(i){
        setTimeout(() => {
            console.log(i)
        }, 1000)
    })(i)
}
```
  
---------------------------------------
2. console印出的值為何?

```
function test(){  
    console.log(a);  
    console.log(foo());  
    var a =1;  
    var b =2;  
    function foo(){  
        return2;  
    }  
}  
test();  
console.log(b);  
// 1. a ?  
// 2. foo ?  
// 3. b ?  
```

答：  
console.log(a);  undefined  
console.log(foo()); return 沒有空格 顯示 return2 is not defined  
console.log(b); 因為上面錯誤不執行此行  
  
---------------------------------------
3. 請說明如何解決 js 非同步的問題?

答：  
可以使用 ES6 Promise 這個解決方案，使用 Promise物件 可以用同步操作的流程寫法來表達非同步操作，  
避免了層層巢狀的非同步回撥，程式碼也更加清晰易懂。  
  
---------------------------------------
4. 要複製一個物件(Object),淺拷貝(ShallowCopy) VS 深拷貝(DeepCopy)有何差異?

答：  
淺拷貝只是複製 reference，會指向同一個記憶體位置，會造成一個改變兩個都一起變，  
要確保物件整個複製，就要使用深拷貝的方式，  
比如JSON.parse(JSON.stringify(object_array))  
或是使用第三方套件：  
jQuery 的 $.extend(true, a, b);  
loadash 的 cloneDeep(a);  
  
---------------------------------------
5. 使用 Ajax 請求3條不同 API 網址,如何取得速度最快 API 網址,請說明你的作法

答：  
使用 Promise 中的 race()，取得最早被解決的 API
  
---------------------------------------
6. console 印出值的順序為何?
```
async function async1 () {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2 () {
    console.log('async2');
}
console.log('script start');
setTimeout(function () {
    console.log('setTimeout');
}, 0);
async1();
new Promise(function (resolve) {
    console.log('promise1');
    resolve();
}).then(function () {
    console.log('promise2');
});
console.log('script end');
```

答：  
1.console.log('script start');  
2.console.log('async1 start');  
3.console.log('async2');  
4.console.log('promise1');  
5.console.log('script end');  
6.console.log('async1 end');  
7.console.log('promise2');  
8.console.log('setTimeout');  
  
---------------------------------------
* Vue
---------------------------------------
11.methods vs computed vs watch

答： 
methods：  
普通的 function 域，觸發重新渲染時，不管資料是否相依都會更新。  

computed：  
當定義 computed 之後，相依的 data 或 props 改變，computed 也會隨之更新，  
需要每次都做更新，就用 method，需要效能，就用 computed。  

watch：  
監聽一個值的改變後執行特定的函數。  
  
---------------------------------------
12.v-if vs v-show

答：  
v-if 是有條件的渲染，若條件判斷為 true 則渲染，v-if 可以有 else，  
v-show 則是渲染後，視條件以 inline style css 隱藏。  
  
---------------------------------------
13.vue 父子組件傳值

答：  
vue 是單向數據流，父子之間形成單向向下綁定，父組件的 prop 更新時，  
子組件 prop 連帶進行更新，反過來則不行。  
  
---------------------------------------
14.請試說 vue 生命週期

答：  
創建 Vue Instance > 初始化 > Template 編譯模板 > DOM 元素掛載 > 資料的渲染與更新 > 卸載  
在這整個週期的期間，vue也讓我們有客製化的空間，稱為 hooks。  
  
---------------------------------------
15.DOM 渲染在哪個週期中就已經完成?

答：  
在 mounted 的時候組渲染完成，可以獲取dom。  
  
---------------------------------------
16.第一次頁面加載會觸發哪些 hook?

答：  
beforeCreate  
create  
beforeMounted  
mounted  
  
---------------------------------------
17.為什麼 vue 實例的 data 是物件 組件的 data 卻是函數

答：  
vue 的 data 是 vue 原型上的属性，  
為了讓每個組件的 data 獨立，必須使用函數，而不是物件，  
因為 data 函數中的 this 會指向當下的組件本身。  
  
---------------------------------------
18.如何解決資料有更新,畫面卻沒有更新

答：  
物件：  
有可能是因為物件一開始沒有被註冊，所以不會觸發到響應式更新，  
可以在一開始就把資料加到 data 裡，  
或是動態添加新的 data，this.$set(object, key, value)  
一次要新增多個屬性可以用 Object.assign  

陣列：  
如果以陣列索引值的方式修改資料內容，Vue 會無法監測到，  
雖然資料已經修改了，但是 Vue 不會觸發響應式更新，  
使用 this.$set(this.items, 0, this.item)  
也可以使用 Vue 可觀察到的陣列方法  
例如： this.items.splice(0, 1, this.item)  
  
---------------------------------------
19.請說明 $nextTick 使用情境

答：  
將 callback 延遲到下一次 DOM 更新後執行  
1.在Vue生命週期的 created hook 做 DOM 操作一定要放在 $nextTick，因為這個時候 DOM 還沒渲染  
2.在資料發生變化後需要執行操作，又需要使用到隨資料變化改變的 DOM 的時候就可以使用 $nextTick  
  
---------------------------------------
20.[Vuex]mutation vs actions

答：  
為了追蹤狀態的變化，mutation 只能是同步的，
action 則可以做任何事只要最後提交 mutation 就好，

mutation：  
每個 mutation 都有名稱和處理程序，是唯一可以改變state的方法，  
他不在乎業務邏輯，只專注在 state。  

actions：  
用來調度 mutation，專注在業務邏輯，可以在一個 actions 裡處理多個 mutation。  

