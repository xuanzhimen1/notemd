## 1. 表单校验

- 提升用户体验, 减轻服务器压力

  ```js
  //校验用户名
  //单词字符，长度8到20位
  function checkUsername() {
      //1.获取用户名值
     var username = $("#username").val();
     //2.定义正则
     var reg_username = /^\w{8,20}$/;
     
     //3.判断，给出提示信息
      var flag = reg_username.test(username);
      if(flag){
          //用户名合法
          $("#username").css("border","");
     }else{
          //用户名非法,加一个红色边框
  		$("#username").css("border","1px solid red");
     }
      return flag;
  }
  
  //校验密码
  function checkPassword() {
      //1.获取密码值
      var password = $("#password").val();
      //2.定义正则
      var reg_password = /^\w{8,20}$/;
  
      //3.判断，给出提示信息
      var flag = reg_password.test(password);
      if(flag){
          //密码合法
          $("#password").css("border","");
      }else{
          //密码非法,加一个红色边框
          $("#password").css("border","1px solid red");
      }
  
      return flag;
  }
  
  //校验邮箱
  function checkEmail(){
      //1.获取邮箱
     var email = $("#email").val();
     //2.定义正则      itcast@163.com
     var reg_email = /^\w+@\w+\.\w+$/;
     //3.判断
     var flag = reg_email.test(email);
     if(flag){
                   $("#email").css("border","");
     }else{
                   $("#email").css("border","1px solid red");
     }
  
     return flag;
  }
  
  $(function () {
      //当表单提交时，调用所有的校验方法
     $("#registerForm").submit(function(){
         return checkUsername() && checkPassword() && checkEmail();
         //如果这个方法没有返回值，或者返回为true，则表单提交，如果返回为false，则表单不提交
     });
      //当某一个组件失去焦点是，调用对应的校验方法
      $("#username").blur(checkUsername);
      $("#password").blur(checkPassword);
      $("#email").blur(checkEmail);
  });
  ```

## 2. 异步ajax提交表单

- post请求

  ```js
  $("#registerForm").submit(function () {
      if (checkUsername() && checkPassword() && checkEmail()) {
          $.post("registUserServlet", $(this).serialize(), function (data) {
              // 处理加回显数据
          });
      }
  });
  ```

  