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
