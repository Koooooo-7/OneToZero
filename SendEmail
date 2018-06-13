  /**
     * 前台用户注册提交
     */
    public function doRegister()
    {

        if ($this->request->isGet()) {
//          获取注册邮箱
            $email = $this->request->param('email');
//          邮箱格式是否合法
            if (Validate::is($email, 'email')) {
//          邮箱是否已存在
                $register = new UserModel();
                $is_exist = $register->where('user_email', $email)->find();
                if ($is_exist) {
                    return $this->error('邮箱已存在！');
                } else {
//                    生成11位随机验证码发送邮件
                    Session::set('email',$email);
                    $randomCode = cmf_random_string(11);
                    Session::set('randomCode', $randomCode);
                    send_mail($email, $randomCode);
                    return $this->success('验证码已发送请查收！');
                }
            } else {
                return $this->error('请填写正确的邮箱格式！');
            }
        };
//          提交表单
        if ($this->request->isPost()) {

            //验证输入表单内容基本规范
            $rules = [
                'vercode' => 'require',
                'email' => 'require',
                'password' => 'require|min:6|max:16',
            ];

            $validate = new Validate($rules);
            $validate->message([
                'vercode.require' => '验证码不能为空！',
                'password.require' => '密码不能为空！',
                'password.max' => '密码不能超过16个字符！',
                'password.min' => '密码不能小于6个字符！',

            ]);

            $data = $this->request->post();   //post信息
            if (!$validate->check($data)) {
                $this->error($validate->getError());
            }
            if ($data['password'] !== $data['repass']) {
                $this->error('两次密码输入不一致！');
            }
            if ($data['vercode'] !== Session::get('randomCode')) {
                $this->error('验证码错误，请重新获取！');
            }

//          复检邮箱合法性  注册账号未改变，注册账号格式合法
            $ori_email =Session::get('email');
            if ($data['email'] !== $ori_email) {
               return $this->error('邮箱变更，请重新获取验证码！');
            }
            if (!(Validate::is($data['email'], 'email'))) {
                return $this->error('邮箱格式错误！');
            }
//          保存登录状态
            Session::set('status',1);
//          注册写入数据库
            $register = new UserModel();
            $code = $register->registerEmail($data);
            switch ($code) {
                case 0:
                    return $this->error('注册失败！');
                    break;
                case 1:
                    return $this->success('注册成功！', 'person/recomd');
                    break;
                case 2:
                    return $this->error('邮箱已存在！');
                    break;
                default:
                    return $this->error('系统好像开小差了！请刷新重试！');
            }
        }
        return $this->error('非法请求！');
    }
    
    
    
    -----------------------------------------   
    /**
     * 发送邮件配置采用Phpmail,参考网址
     * http://www.dawnfly.cn/article-1-350.html
     * @param string  $tomail 收件人邮箱地址
     * @param int $randcode 验证码
     * @param string $name 收件人姓名 默认空则为邮箱地址
     * 选用CMF框架时会自带发送邮件方法
     **/  
function send_mail($tomail, $randcode, $name ='' , $subject = '', $attachment = null) {
//    $randcode = cmf_random_string($randcode_len); //随机验证自串长度，默认为6
    $body = '你好，欢迎你注册！你的验证码是: '.$randcode;
    $mail = new \PHPMailer();           //实例化PHPMailer对象
    $mail->CharSet = 'UTF-8';           //设定邮件编码，默认ISO-8859-1，如果发中文此项必须设置，否则乱码
    $mail->IsSMTP();                    // 设定使用SMTP服务
    $mail->SMTPDebug = 0;               // SMTP调试功能 0=关闭 1 = 错误和消息 2 = 消息
    $mail->SMTPAuth = true;             // 启用 SMTP 验证功能
    $mail->SMTPSecure = 'ssl';          // 使用安全协议
    $mail->Host = "smtp.qq.com"; // SMTP 服务器
    $mail->Port = 465;                  // SMTP服务器的端口号
    $mail->Username = "XXXXX@XX.com";    // SMTP服务器用户名
    $mail->Password = "XXXXXXXXX";     // SMTP服务器密码（如果设置独立密码请填空独立密码），请先确保邮箱开通SMTP/POP3功能
    $mail->SetFrom('XXXXX@qq.XX', 'Mt');  //设置发件人
    $replyEmail = '';                   //留空则为发件人EMAIL
    $replyName = '';                    //回复名称（留空则为发件人名称）
    $mail->AddReplyTo($replyEmail, $replyName);
    $mail->Subject = $subject;
    $mail->MsgHTML($body);  //邮件内容
    $mail->AddAddress($tomail, $name);
    if (is_array($attachment)) { // 添加附件
        foreach ($attachment as $file) {
            is_file($file) && $mail->AddAttachment($file);
        }
    }
    return $mail->Send() ? true : $mail->ErrorInfo;
}





发送带样式（背景图等）的邮件
通过HTML内嵌的方式完成，但是好像不支持CSS。
图片引自网络（可连接的对外服务器地址），然后要放在<table>中！
 <!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
</head>
<body  >
   <table >
   <tbody >
    <tr>
   <td  background="https://sz-btfsv2.ftn.qq.com/ftn_handler/4e0482269942fbba52491b9a1f3412341f4d86b784aa6a538e156f9c7f42b29ce7579193b3f78e09b213df49e5f5e21069f211b79016ffcdd68f994ab7ac67c8/?fname=*.png&pictype=scaled&size=1024*768" >
    <div style="position:relative;color: #40485B;width:620px;min-height:508px;font-size: 14px;margin:0 auto;border: 1px solid #fff;">
    	<div style="margin-top:56px;font-size:14px;text-align:left;margin-left:72px;margin-right:42px;">
    		<div style="margin-bottom: 10px;">
				<div style="font-size: 18px;font-weight:bold;line-height: 10px;padding: 10px 20px;">
        注册验证
				</div>
				<div style="padding: 20px;">
					<p><p>
    您好
				<a href="" target="_blank" style="text-decoration:underline;color: #064977; "></a>
    !
			</p>XXXXXXXXXXXXXXXXXXXXXX。</p>
					<p>验证码：<span style="color: #064977;font-weight: bold">'.$randcode.'</span></p>
					<p style="color:#9B9B9B;font-size: 12px;margin-top: 20px;">
        @yhstream.com
			</p>
				</div>
			</div>	
			
    	</div>
	</div>
	</td>
	</tr>
	  </tbody>
	</table>
</body>
</html>';
