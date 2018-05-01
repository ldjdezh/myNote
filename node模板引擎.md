# 在express中使用art-template模板引擎

步骤

1. 要先安装 `art-template` 和 `express-art-template` 

2. `app.engine('html', require('express-art-template'))`，要写上这句话

3. ```javascript
   res.render('index.html', {
   	item: r
   })
   ```

   ​

