js30
1.打鼓囉
首先先使用

window.addEventListener('keydown',function(e){
});

addEventListener（https://www.w3schools.com/jsref/met_element_addeventlistener.asp）（替每個按鈕元素綁上一個固定的"keydown"事件）
接下來使用

  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);

querySeleteor (https://www.w3schools.com/jsref/met_document_queryselector.asp)
(取得文件中第一個 audio tag元素）
並且取得每個keycode的值，當輸入的按鈕沒有預設的data-key值就停止這個function

由於音效檔案是會播放一段時間的，我們想從0秒開始於是使用
audio.currentTime = 0;

來設置從0秒開始播放

單純按鍵有點無聊，來替每個按下的class加個放大變色的特效吧！
首先一樣使用

  const audio = document.querySelector(`.key[data-key="${e.keyCode}"]`);
來取得每個按下按鍵的div

當取得每個按下去的div使用
key.classList.add('playing');
來新增一個class="playing"給這個div，我們只想讓他閃一下，所以放大完之後要移除
有幾種方式，能使用setTimeout來設定時間多久就消失(但是你不知道這個音效會播多久），或是使用css transition來控制變換的時間

首先使用querySelectorAll來抓到每個class="key"並且使用addEventListener替每個按鍵綁上transitionend的事件和一個removeTransition的function(來移除變化完之後用來變化的class)
keys.forEach(key=> key.addEventListener('transitionend',removeTransition));

function removeTransition(e){
  if(e.propertyName !=='transform') return;
  this.classList.remove('playing');
  (這邊的this指的是key.addEventListener，不清楚的話可以使用
  console.log(this) 來印出來看看目前的this指到哪裡
}

最後再把播放音效的function另外寫出來就好啦


2.滴答滴答滴答

首先畫出指針，再利用css屬性 transform:rotate 來旋轉div達到時鐘的效果
但是旋轉的位置不是我們想要的，使用transform-origin 來改變旋轉的基準點
使用 transition-timing-function 來讓指針有跳針的效果

開始讓秒針轉動使用setInterval() 來讓他每秒跳一次
再來使用getSeconds根據現在當地時間來傳回秒數，拿到秒數就得讓指針跳動
依照每秒來計算他該跳幾度，用querySelector來找到秒針的class


3.視力檢查（js改變css)

由於想要使用js改變css，首先我們將一些css屬性宣告成變數
  :root{
      --base:  #ff0000;
      --spacing: 10px;
      --blur: 10px;
    }

 就可以以變數的方式使用它了

   padding: var(--spacing);
    background: var(--base);
    filter: blur(var(--blur));

其中filter有許多特效（https://www.w3schools.com/cssref/css3_pr_filter.asp），這邊我們使用blur(模糊）

接著使用querySeletorAll來選取controls下的每個input，由querySelectorAll 回傳一個類似陣列的物件稱為 Node List 這些資料結構簡稱為「類陣列」，因為他們和陣列很相似，但是不能使用陣列的方法像是 map 等，所以通常會把它轉成array 的形式，不過這個例子中只會用到forEach就先不用轉啦

接著使用forEach監聽input值的變動，當有變動使用addEventListener觸發change更新函式，這樣的方式沒辦法隨著滑鼠移動時同時變動，在這多加一個滑鼠移動的事件來觸發

變動的值沒有單位，宣告一個變數來取得寫在html中的data-sizing值"px"
   const suffix=this.dataset.sizing||'';

使用
   document.documentElement.style.setProperty(`--${this.name}`,this.value+suffix);
將變更的值傳到選到name的這個css中

vue的做法
https://vuejs.org/v2/examples/svg.html


4.array用法
filter篩選
map使用原資料做改變，改變完原資料不變
sort排序

'template string'   `${}
http://www.infoq.com/cn/articles/es6-in-depth-template-string

queryseletor是取得nodelist 所以如果要對他做map 的方法就得先轉array
  const links = category.querySelectorAll('a');
  const links = Array.from(category.querySelectorAll('a'));

includes  確認字串中是否有包含這個字串
split

使用console.table()輸出array的時候會是表格的形式，但是物件的屬性不同時會出現很多undefine

5.flexbox
Cubic Bezier 用來控制動畫的速度
http://www.oxxostudio.tw/articles/201406/css-cubic-bezier.html
http://cubic-bezier.com/#1,-0.47,.01,.57

先將div設為display:flex;
flex-direction可控制排列的方向（從左到右或是從上到下）
justify-content決定子元素和父元素的水平對齊位置
align-item決定子元素和父元素的垂直對齊位置
寫好要變化的css
 transform: translateY(0); 可以控制css在y軸變化的位置

 先使用queryselectorall選取所有.panel的div
 在使用addEventListener監聽事件當click觸發toggleopen函式
  function toggleopen(){
    this.classList.toggle('open');
        // console.log(this);
  }
在使用addEventListener監聽事件當transitionend(變化完成）觸發toggleactive函式

  function toggleactive(e){
    if(e.propertyName.includes('flex')){
      this.classList.toggle('open-active');
    }

  }


6.ajax

fetch
它會被用來執行送出Request(要求)的工作，如果成功得到回應的話，它會回傳一個帶有Response(回應)物件的已實現Promise物件。

then
https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/contents/then_adv.html

push es6

// ES5
var arr1 = [0, 1, 2];var arr2 = [3, 4, 5];Array.prototype.push.apply(arr1, arr2);
// ES6
var arr1 = [0, 1, 2];var arr2 = [3, 4, 5];arr1.push(...arr2);


7.array 用法  (some every find findindex)

 some
 https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/some
 https://www.denpe.com/jsbook/javascript-array-some.html
 some裡面的callback 支援(element, index, array)
 some()方法的返回值为Boolean，将原数组中的每个元素都按顺序执行一次 callback 函数，只要有任何一项元素执行结果返回 true，则some()方法返回 true。如果所有元素都返回 false，some()方法返回 false。
 some()不会改变原数组。
 some()遍历的元素的范围在第一次调用 callback 就已經確定。在调用some()后被添加到数组中的值不会被  callback 访问到。如果数组中存在的元素被更改，则他们传入 callback 的值是some()访问到他们那一刻的值。那 些被删除的元素或从来未被赋值的元素将不会被访问到。

 只要裡面有一個符合的話就傳回true
 由於想要選出有沒有>19歲的，有的話返回true 沒有false
    const older = people.some(person => 2017 - person.year >19 )
  但是不是每一年都是2017 所以得使用 getFullYear()來取得今年是幾年 (會搭配new date())
   const currentYear = (new Date ()).getFullYear();
   將2017以currentYear代入即可

 every
  裡面全都符合的話就傳回true
     const allolder = people.every(person => currentYear - person.year>19 )

 find
  跟filter很像，但是他只會傳回條件指定的值()
     const findnum = comments.find ( comment => comment.id = 823423)

 findindex
  傳回條件指定值得索引
   const findnum = comments.findindex ( comment => comment.id = 823423)

  可用splice or slice方式來刪除數組中的資料
  splice是直接刪除原數組中選定的值
    comments.splice(index ,1)

  slice會提取所選定範圍的值

8.
