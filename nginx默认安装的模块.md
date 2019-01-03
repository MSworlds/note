​      nginx的所有模块必须在编译的时候添加，不能再运行的时候动态加载，默认的编译选项下包含的模块，如果你不是显示的用参数关闭它。

nginx默认安装的模块如下

| 模块名称           | 描述                                       | 版本     | 如何禁用                                     |
| -------------- | ---------------------------------------- | ------ | ---------------------------------------- |
| Core           | Control ports, locations, error pages, aliases, and other essentials. |        | --without-http                           |
| Access         | Allow/deny based on IP address.          |        | --without-http_access_module             |
| Auth Basic     | Basic HTTP authentication.               |        | --without-http_auth_basic_module         |
| Auto Index     | Generates automatic directory listings.  |        | --without-http_autoindex_module          |
| Browser        | Interpret "User-Agent" string.           | 0.4.3  | --without-http_browser_module            |
| Charset        | Recode web pages.                        |        | --without-http_charset_module            |
| Empty GIF      | Serve a 1x1 image from memory.           | 0.3.10 | --without-http_empty_gif_module          |
| FastCGI        | FastCGI Support.                         |        | --without-http_fastcgi_module            |
| Geo            | Set config variables using key/value pairs of IP addresses. | 0.1.17 | --without-http_geo_module                |
| Gzip           | Gzip responses.                          |        | --without-http_gzip_module               |
| Headers        | Set arbitrary HTTP response headers.     |        |                                          |
| Index          | Controls which files are to be used as index. |        |                                          |
| Limit Requests | Limit frequency of connections from a client. | 0.7.20 | --without-http_limit_req_module          |
| Limit Zone     | Limit simultaneous connections from a client. Deprecated in 1.1.8, use Limit Conn Instead. | 0.5.6  | --without-http_limit_zone_module         |
| Limit Conn     | Limit concurrent connections based on a variable. |        | --without-http_limit_conn_module         |
| Log            | Customize access logs.                   |        |                                          |
| Map            | Set config variables using arbitrary key/value pairs. | 0.3.16 | --without-http_map_module                |
| Memcached      | Memcached support.                       |        | --without-http_memcached_module          |
| Proxy          | Proxy to upstream servers.               |        | --without-http_proxy_module              |
| Referer        | Filter requests based on `Referer` header. |        | --without-http_referer_module            |
| Rewrite        | Request rewriting using regular expressions. |        | --without-http_rewrite_module            |
| SCGI           | SCGI protocol support.                   | 0.8.42 | --without-http_scgi_module               |
| Split Clients  | Splits clients based on some conditions  | 0.8.37 | --without-http_split_clients_module      |
| SSI            | Server-side includes.                    |        | --without-http_ssi_module                |
| Upstream       | For load-balancing.                      |        | --without-http_upstream_ip_hash_module (ip_hash directive only) |
| User ID        | Issue identifying cookies.               |        | --without-http_userid_module             |
| uWSGI          | uWSGI protocol support.                  | 0.8.40 | --without-http_uwsgi_module              |
| X-Accel        | X-Sendfile-like module.                  |        |                                          |

```json
 location / {
        proxy_pass  http://apachephp;

        #Proxy Settings
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_max_temp_file_size 0;
        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
   }

```

）server
语法： server name[parameters];
配置块： upstream
server配置项指定了一台上游服务器的名字，这个名字可以是域名、IP地址端口、UNIX
句柄等，在其后还可以跟下列参数。
·weight=number：设置向这台上游服务器转发的权重，默认为1。
·max_fails=number：该选项与fail_timeout配合使用，指在fail_timeout时间段内，如果向 当前的上游服务器转发失败次数超过number，则认为在当前的fail_timeout时间段内这台上游 服务器不可用。max_fails默认为1，如果设置为0，则表示不检查失败次数。
·fail_timeout=time：fail_timeout表示该时间段内转发失败多少次后就认为上游服务器暂 时不可用，用于优化反向代理功能。它与向上游服务器建立连接的超时时间、读取上游服务 器的响应超时时间等完全无关。fail_timeout默认为10秒。
·down：表示所在的上游服务器永久下线，只在使用ip_hash配置项时才有用。
·backup：在使用ip_hash配置项时它是无效的。它表示所在的上游服务器只是备份服务 器，只有在所有的非备份上游服务器都失效后，才会向所在的上游服务器转发请求。













（8）proxy_next_upstream
语法： proxy_next_upstream[error|timeout|invalid_header|http_500|http_502|http_503|http_504|http_404|off]; 默认： proxy_next_upstream error timeout;
配置块： http、server、location
此配置项表示当向一台上游服务器转发请求出现错误时，继续换一台上游服务器处理这
个请求。前面已经说过，上游服务器一旦开始发送应答，Nginx反向代理服务器会立刻把应 答包转发给客户端。因此，一旦Nginx开始向客户端发送响应包，之后的过程中若出现错误
也是不允许换下一台上游服务器继续处理的。这很好理解，这样才可以更好地保证客户端只 收到来自一个上游服务器的应答。proxy_next_upstream的参数用来说明在哪些情况下会继续
选择下一台上游服务器转发请求。
·error：当向上游服务器发起连接、发送请求、读取响应时出错。
·timeout：发送请求或读取响应时发生超时。
·invalid_header：上游服务器发送的响应是不合法的。
·http_500：上游服务器返回的HTTP响应码是500。
·http_502：上游服务器返回的HTTP响应码是502。
·http_503：上游服务器返回的HTTP响应码是503。
·http_504：上游服务器返回的HTTP响应码是504。
·http_404：上游服务器返回的HTTP响应码是404。
·off：关闭proxy_next_upstream功能—出错就选择另一台上游服务器再次转发