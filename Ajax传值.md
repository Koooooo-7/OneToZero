### layui

//监听提交 <br>
 ```
        form.on('submit(*)', function(data){ 
            $.ajax({
                type:"post",
                url:"{:url('user/register/doRegister')}",
                datatype:"json",
                contentType:"application/json",
                data:JSON.stringify(data.field),
                success: function (data) {
                    layer.alert(JSON.stringify(data.msg));
                    if(data.code){
                        setTimeout(function(){window.location.href = data.url;},2000);
                    }
                }
            });
            return false;
        });
        ```
        <br>
//click事件监听 <br>


```
        $("#my_id").on("click",function(){
            $email = document.getElementById('my_email').value;
            // layer.msg("点击了");
            // layer.msg($email);
            $.ajax({
                type:"get",
                url:"{:url('user/register/doRegister')}",
                data:{"email":$email},
                success: function (data) {
                    layer.alert(JSON.stringify(data.msg));
                }
            });
```
