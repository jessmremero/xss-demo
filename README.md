# xss-demo
xss：cross site scripting。跨站脚本，实现代码注入，利用的是字符串拼接的原理，然后放到页面里。
例如：
 $.get('./comments.json').then(function(response){
    let comments = response
    var $ol = $('#ureol')
    comments.forEach(function(item){
    var $li = $(`
       <li>评论者：${item.user}
       <img src="${item.avatar}" alt=""><!--图片利用双引号拼接src，实现代码注入-->    
       <p>评论内容：${item.content}</p>
       </li>
        `) 
 json数据如下所示：
 {"user":"jack","content":"how are you?","avatar":"\" width=100 height=100 onclick=\"alert(1)\""},
 {"user":"frank","content":"<script>alert('stupid')</script>","avatar":"http://img2.imgtn.bdimg.com/it/u=1343212205,678582139&fm=214&gp=0.jpg"} 
 问题一、假设有<script>标签，就会运行里面的代码，如item.content
 问题二、假设有图片，利用字符串拼接原理，把图片src，变为“” 实现代码注入
 解决方法：
 1、所有的页面输入通过.text()来实现，因为该API会把所有能转义的都转义
 2、有一个库叫escape-HTML，专门用来转义的。通过正则转义
