# Nginx 安全配置

1. 非get、post和HEAD请求使用405 响应，排除XST攻击

   ```shel
   if ($request_method !~ ^(GET|HEAD|POST)$ )
   {
   return 405;
   }
   ```

2. d 

3. d

4. d

