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


//邮箱验证keyup  
layui.use(['form','element'], function(){
        var $ = layui.jquery;
        var form = layui.form
            ,layer = layui.layer;

        $("#mt-email").keyup(function(){
            $email = document.getElementById('mt-email').value;
            if(/^([a-zA-Z0-9._-])+@([a-zA-Z0-9_-]){2,5}(\.[a-zA-Z0-9_-])+/.test($email)){
                setTimeout(function(){
                    $.ajax({
                        type:"get",
                        url:"{:url('user/register/doRegister')}",
                        data:{"email":$email},
                        success: function (data) {
                            if(data.msg){
                                layer.msg(JSON.stringify(data.msg),{time:2000, shift: 6},function(){});
                            }
                            // layer.alert(JSON.stringify(data.msg));
                        }
                    });
                    return false;
                },1000);
            }
        });
        return false;
     });
     
