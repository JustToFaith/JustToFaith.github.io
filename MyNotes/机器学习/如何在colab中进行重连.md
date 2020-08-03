如何在colab中进行重连

`ctrl + shift + i`进去console中运行如下代码：

```
function ClickConnect(){
console.log("Working"); 
document.querySelector("colab-toolbar-button#connect").click() 
}
setInterval(ClickConnect,60000)
```



下面的代码好像也行：

```
// Interval time to check if runtime is disconnected
interval = 1000 * 60;

// Busy/Reconnect button top-right
reloadButton = document.querySelector('#connect > paper-button > span')

setInterval(() => {
    if (reloadButton.innerText == 'Reconnect') {
        reloadButton.click();
        console.log('Restarting');
    } else console.log('Keeps working');
}, interval);
```

