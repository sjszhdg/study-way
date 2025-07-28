# 环境搭建 #
首先要把环境下好我是之前直接下的

<img width="601" height="150" alt="image" src="https://github.com/user-attachments/assets/2756a5f2-d0bd-4411-8c34-32070d84a8e4" />

下载完后再浏览器上打开，ip加8080端口号。

<img width="649" height="388" alt="image" src="https://github.com/user-attachments/assets/72a4ad05-6672-4762-b639-35366db5fcd2" />

打开网页后就输入用户名和密码然后下一步安装就行了

到这个界面后

<img width="657" height="275" alt="image" src="https://github.com/user-attachments/assets/e421af87-92ba-492c-a9aa-c4dc0394c3c2" />

创建一个新图形，选device-uptime然后创建就行了，创建完之后就可以退出了，我们是要以访客的状态去测试的。

<img width="654" height="247" alt="image" src="https://github.com/user-attachments/assets/b7d3e8c1-3293-45de-8789-5889c77c7c28" />

我们先看一下数据库怎么存储值
`
docker ps -a
docker exec -it 11e /bin/bash
mysql -uroot -p
然后输入密码
`

然后能看到我们这里的数据库

<img width="650" height="371" alt="image" src="https://github.com/user-attachments/assets/92654c27-d887-4d81-8ef5-26f94b6cccda" />

然后我们进到cacti的数据库里

<img width="651" height="271" alt="image" src="https://github.com/user-attachments/assets/45554c85-3b45-4de0-ada3-21caecc1d3d3" />

这里数据库的表实在太多了

<img width="652" height="449" alt="image" src="https://github.com/user-attachments/assets/f8d528d7-a3df-43e4-800e-dda6fe5bad51" />

通过这个payload我们看到
<img width="648" height="146" alt="image" src="https://github.com/user-attachments/assets/bf7bb1d4-15cf-4161-bf1a-4676ac9121e7" />

这里定义了一个鉴权函数

<img width="658" height="95" alt="image" src="https://github.com/user-attachments/assets/f9e36fa1-f724-423e-a1cb-51926a63be10" />


这段代码的核心逻辑是**权限校验**，具体解析如下： - `remote_client_authorized()` 是一个函数，推测其作用是检查当前访问服务的远程客户端（如用户、设备等）是否已获得授权。 

​		`!remote_client_authorized()` 表示对上述检查结果取反，即“如果远程客户端未被授权”。 - 若满足“未授权”条件，则执行：

​		`print 'FATAL: You are not authorized to use this service'`：输出错误信息，提示“致命错误：你未被授权使用此服务”。 

​		`exit`：终止程序运行，阻止未授权客户端继续访问服务。 整体来看，这段代码是服务端常见的安全校验逻辑，用于拦截未授权访问，保护服务的安全性。


鉴权之后有一下操作我们来看一下

<img width="653" height="580" alt="image" src="https://github.com/user-attachments/assets/a0dd3223-fbd3-4f93-9230-79aaeb084510" />

```
// 1. 函数定义与返回值声明
// 定义一个名为 remote_client_authorized 的函数，返回值类型为 bool（布尔值，true 或 false）
function remote_client_authorized(): bool {  
    // 2. 引入全局变量
    // 引入全局变量 $poller_db_cnn_id ，后续可能用它操作数据库连接
    global $poller_db_cnn_id;  

    // 3. 命令行运行限制注释
    // 注释：不允许从命令行运行（提醒代码设计者/维护者该逻辑的运行场景约束）
    /* don't allow to run from the command line */  

    // 4. 获取客户端地址
    // 调用 get_client_addr 函数获取客户端的 IP 地址或主机地址，结果存在 $client_addr 
    $client_addr = get_client_addr();  
    // 5. 校验客户端地址获取结果
    // 如果获取客户端地址失败（返回 false ），函数直接返回 false ，授权不通过
    if ($client_addr === false) {  
        return false;
    }

    // 6. 校验 IP 格式合法性
    // 使用 filter_var 函数，配合 FILTER_VALIDATE_IP 过滤器，检查 $client_addr 是否是合法 IP 。
    // 如果不是合法 IP ，记录错误日志（cacti_log 是 Cacti 框架里记录日志的函数 ），然后返回 false ，授权不通过
    if (!filter_var(value: $client_addr, filter: FILTER_VALIDATE_IP)) {  
        cacti_log(string: 'ERROR: Invalid remote agent client IP Address.  Exiting');
        return false;
    }

    // 7. 通过 IP 反向解析主机名
    // 调用 gethostbyaddr 函数，尝试通过客户端 IP（$client_addr ）反向解析出主机名，结果存在 $client_name 
    $client_name = gethostbyaddr(ip: $client_addr);  

    // 8. 校验主机名解析结果
    // 如果解析出来的主机名（$client_name ）和原始 IP（$client_addr ）一样，说明反向解析失败（没能得到有意义的主机名 ）
    if ($client_name == $client_addr) {  
        // 记录中等 verbosity 级别的日志（告知无法解析主机名情况 ），output: false 表示不直接输出到前端展示，environ: 'WEBU' 说明是 Web 环境相关日志
        cacti_log(string: 'NOTE: Unable to resolve hostname from address ' . $client_addr, 
            output: false, environ: 'WEBU', level: POLLER_VERBOSITY_MEDIUM);
    } else {  
        // 如果解析成功（得到了不同于 IP 的主机名字符串 ），调用 remote_agent_strip_domain 函数去除主机名里的域名部分（比如把 example.com 处理成 example  ）
        $client_name = remote_agent_strip_domain(host: $client_name);  
    }

    // 9. 查询 poller 表数据
    // 调用 db_fetch_assoc 函数（Cacti 里操作数据库查询，返回关联数组结果的函数 ），查询 poller 表的所有数据（SELECT * FROM poller ），
    // log: true 表示查询过程若有问题会记录日志，db_conn: $poller_db_cnn_id 指定用之前引入的全局数据库连接去执行查询，结果存在 $pollers 
    $pollers = db_fetch_assoc(sql: 'SELECT * FROM poller', log: true, db_conn: $poller_db_cnn_id);  

    // 10. 遍历 poller 表数据进行授权校验
    // 如果查询结果非空（cacti_sizeof 是 Cacti 里用于计算变量“有效大小”的函数，这里判断 $pollers 里有数据 ）
    if (cacti_sizeof(object: $pollers)) {  
        // 遍历 $pollers 里的每条记录，每条记录存在 $poller 变量里
        foreach ($pollers as $poller) {  
            // 11. 校验主机名（去除域名后）匹配
            // 把 poller 表当前记录里的 hostname 字段值，也去除域名，然后和之前处理好的客户端主机名（$client_name ）对比。
            // 如果匹配，说明该客户端在授权列表里，返回 true ，授权通过 
            if (remote_agent_strip_domain(host: $poller['hostname']) == $client_name) {  
                return true;
            // 12. 校验 IP 匹配
            // 如果主机名没匹配上，再直接拿 poller 表当前记录的 hostname 字段值，和客户端 IP（$client_addr ）对比。匹配则返回 true ，授权通过  
            } elseif ($poller['hostname'] == $client_addr) {  
                return true;
            }
        }
    }
    // 13. 若以上校验都没通过，默认返回 false（代表授权不通过 ），不过代码里这里没显式写 return false; ，
    // 因为 PHP 函数若没遇到 return 语句，执行完会隐式返回 null ，但函数声明返回 bool ，实际运行可能有类型兼容问题，规范写法应该在最后加 return false; 
}
```

但是这里似乎不是获取ip的地方我们重新回到57行看一下，

这里可以看到，鉴权结束后是走到action这里的



get传递的参数，用户可控，他就一定会走到case polldata这个开关语句，然后又走到了poll_for_data这里

<img width="649" height="444" alt="image" src="https://github.com/user-attachments/assets/b7ee0314-d2a4-4b75-a718-afbced33396b" />

```
// 1. 设置默认动作
// 调用 set_default_action 函数，作用一般是初始化或设置“默认要执行的动作”，具体逻辑看函数实现，这里先执行它做前置准备  
set_default_action();  

// 2. 基于请求参数做 switch 分支判断
// get_request_var // get_request_var 是自定义函数（常见于框架或工具类，用于从请求里获取指定 name 的变量值，这里获取名为 'action' 的参数值 ），
// 拿到值后进入 switch 分支判断，根据不同 action 值执行不同逻辑
switch (get_request_var(name: 'action')) {  
    // 3. case 'polldata' 分支
    case 'polldata':  
        // 3.1 注释说明
        // 注释：仅让实时轮询（realtime polling）运行较短时间，给开发者提示这段逻辑的意图
        // Only let realtime polling run for a short time  
        
        // 3.2 设置最大执行时间
        // ini_set 是 PHP 原生函数，用于设置 php.ini 里的配置项，这里设置 'max_execution_time'（脚本最大执行时间，单位秒 ），
        // 值从 read_config_option 函数获取（推测是从配置系统里读 'script_timeout' 对应配置，比如配置文件、数据库配置项 ），
        // 作用是限制当前“polldata”动作的执行时长，避免长时间运行占资源
        ini_set(option: 'max_execution_time', 
            value: read_config_option(config_name: 'script_timeout'));  

        // 3.3 调试日志输出（开始）
        // debug 是自定义函数（Cacti 等系统常用调试输出函数 ），输出日志“Start: Poling Data for Realtime”，标记实时轮询数据采集开始
        debug(message: 'Start: Poling Data for Realtime');  

        // 3.4 执行轮询数据采集逻辑
        // 调用 poll_for_data 函数，实际去执行“轮询数据”的核心逻辑，比如从设备获取监控数据等操作
        poll_for_data();  

        // 3.5 调试日志输出（结束）
        // 输出日志“End: Poling Data for Realtime”，标记实时轮询数据采集结束
        debug(message: 'End: Poling Data for Realtime');  
}
```

看一下这个poll_for_data()

```
function poll_for_data() {
    // [1] 引入全局配置变量（包含系统路径等设置）
    global $config;

    // [2] 获取请求参数
    $local_data_ids = get_nfilter_request_var('local_data_ids');  // 需采集的数据ID数组（未过滤）
    $host_id        = get_filter_request_var('host_id');         // 主机ID（数字类型）
    $poller_id      = get_nfilter_request_var('poller_id');      // 采集器ID（未过滤）
    $return         = array();                                   // 初始化返回结果数组
    $i = 0;                                                     // 结果索引计数器

    // [3] 检查是否存在需采集的数据ID
    if (cacti_sizeof($local_data_ids)) {
        // [4] 遍历每个数据ID
        foreach($local_data_ids as $local_data_id) {
            // [5] 验证数据ID为合法数字
            input_validate_input_number($local_data_id);

            // [6] 查询数据库获取采集项
            $items = db_fetch_assoc_prepared('SELECT *
                FROM poller_item
                WHERE host_id = ?
                AND local_data_id = ?',
                array($host_id, $local_data_id));
            
            // [7] 统计需要PHP脚本服务器执行的采集项数量
            $script_server_calls = db_fetch_cell_prepared('SELECT COUNT(*)
                FROM poller_item
                WHERE host_id = ?
                AND local_data_id = ?
                AND action = 2',
                array($host_id, $local_data_id));

            // [8] 处理存在的采集项
            if (cacti_sizeof($items)) {
                foreach($items as $item) {
                    // [9] 根据采集类型执行不同操作
                    switch ($item['action']) {
                    // [10] SNMP 采集处理
                    case POLLER_ACTION_SNMP: /* snmp */
                        // 检查SNMP配置有效性
                        if (($item['snmp_version'] == 0) || (($item['snmp_community'] == '') && ($item['snmp_version'] != 3))) {
                            $output = 'U'; // 无效配置返回U(Unavailable)
                        } else {
                            // 查询主机参数
                            $host = db_fetch_row_prepared('SELECT ping_retries, max_oids FROM host WHERE hostname = ?', array($item['hostname']));
                            
                            // 建立SNMP会话
                            $session = cacti_snmp_session(
                                $item['hostname'], $item['snmp_community'], $item['snmp_version'],
                                $item['snmp_username'], $item['snmp_password'], $item['snmp_auth_protocol'], 
                                $item['snmp_priv_passphrase'], $item['snmp_priv_protocol'], $item['snmp_context'], 
                                $item['snmp_engine_id'], $item['snmp_port'], $item['snmp_timeout'], 
                                $host['ping_retries'], $host['max_oids']
                            );
                            
                            if ($session === false) {
                                $output = 'U'; // 会话建立失败
                            } else {
                                // 获取SNMP数据并关闭会话
                                $output = cacti_snmp_session_get($session, $item['arg1']);
                                $session->close();
                            }
                            
                            // 验证结果合法性
                            if (prepare_validate_result($output) === false) {
                                $output = 'U'; // 非法数据标记为U
                            }
                        }
                        
                        // 存储结果
                        $return[$i]['value']         = $output;
                        $return[$i]['rrd_name']      = $item['rrd_name'];
                        $return[$i]['local_data_id'] = $local_data_id;
                        break;
                    
                    // [11] 外部脚本采集处理
                    case POLLER_ACTION_SCRIPT: /* script (popen) */
                        // 执行系统命令并清理输出
                        $output = trim(exec_poll($item['arg1']));
                        
                        // 验证结果
                        if (prepare_validate_result($output) === false) {
                            $output = 'U'; // 非法结果标记为U
                        }
                        
                        // 存储结果
                        $return[$i]['value']         = $output;
                        $return[$i]['rrd_name']      = $item['rrd_name'];
                        $return[$i]['local_data_id'] = $local_data_id;
                        break;
                    
                    // [12] PHP脚本服务器采集处理
                    case POLLER_ACTION_SCRIPT_PHP: /* script (php script server) */
                        // 初始化管道配置
                        $cactides = array(
                            0 => array('pipe', 'r'), // 子进程读端
                            1 => array('pipe', 'w'), // 子进程写端
                            2 => array('pipe', 'w')  // 错误输出
                        );
                        $using_proc_function = false;
                        
                        // [13] 尝试启动PHP脚本服务器
                        if (function_exists('proc_open')) {
                            $cactiphp = proc_open(
                                read_config_option('path_php_binary') . ' -q ' . 
                                $config['base_path'] . '/script_server.php realtime ' . $poller_id,
                                $cactides,
                                $pipes
                            );
                            $output = fgets($pipes[1], 1024); // 读取初始输出
                            $using_proc_function = true;
                        }
                        
                        // [14] 通过脚本服务器执行采集
                        if ($using_proc_function == true) {
                            // 执行PHP脚本采集
                            $output = trim(str_replace("\n", '', exec_poll_php(
                                $item['arg1'], 
                                $using_proc_function, 
                                $pipes, 
                                $cactiphp
                            )));
                            
                            // 验证结果
                            if (prepare_validate_result($output) === false) {
                                $output = 'U'; // 非法结果标记为U
                            }
                        } else {
                            $output = 'U'; // 无proc_open支持时标记为U
                        }
                        
                        // 存储结果
                        $return[$i]['value']         = $output;
                        $return[$i]['rrd_name']      = $item['rrd_name'];
                        $return[$i]['local_data_id'] = $local_data_id;
                        
                        // [15] 关闭脚本服务器（当需要时）
                        if (($using_proc_function == true) && ($script_server_calls > 0)) {
                            // 发送退出命令
                            fwrite($pipes[0], "quit\r\n");
                            // 关闭所有管道
                            fclose($pipes[0]);
                            fclose($pipes[1]);
                            fclose($pipes[2]);
                            // 关闭进程
                            $return_value = proc_close($cactiphp);
                        }
                        break;
                    }
                    
                    // [16] 递增结果索引
                    $i++;
                }
            }
        }
    }
    
    // [17] 以JSON格式输出采集结果
    print json_encode($return);
}
```

所以这里我们要查poller-item能看到一共有六个

```
select * from poller-item \G
```
<img width="631" height="180" alt="image" src="https://github.com/user-attachments/assets/f810a6e1-1ee7-4a9c-82b4-a6bf2fcbe6fe" />

以上的action的值都是1，但是我们根据上面的poll_for_data()，得出我们需要的action是2

<img width="652" height="323" alt="image" src="https://github.com/user-attachments/assets/fdb85824-f71a-4492-8990-2a0563f104dd" />

这里查询local-data-id肯定要是6，因为只有是6，action才能是2,这里host-id不用管他


<img width="655" height="252" alt="image" src="https://github.com/user-attachments/assets/ae32da00-d8c6-4c22-8b7f-0ed35c74bc3c" />

是在$items这里查了六个值，然后再if里面做了个循环

把每一条数据都拿出来，就是上面的那个1 ，2,3那个数据，把里面的action取出来，里面只有6是2其他都是1，当action=0的时候就是走下面这个


<img width="660" height="337" alt="image" src="https://github.com/user-attachments/assets/adecc96f-ee68-4fc4-9eb6-1a071ecbbd14" />

等于1的时候走


<img width="658" height="385" alt="image" src="https://github.com/user-attachments/assets/0aaa2a67-d927-46b9-a650-7b61e070cb69" />

等于2的时候走，很明显我们的action要等于2，这样才能执行下面的步骤


<img width="658" height="177" alt="image" src="https://github.com/user-attachments/assets/bef0da24-7f6b-4155-846a-18ba10ae41e0" />

```
// [1] SNMP 采集处理
case POLLER_ACTION_SNMP: /* snmp */
    // 检查SNMP配置是否有效：版本0无效 或 (社区字符串为空且不是SNMPv3)
    if (($item['snmp_version'] == 0) || (($item['snmp_community'] == '') && ($item['snmp_version'] != 3)) {
        $output = 'U'; // 无效配置直接标记为不可用
    } else {
        // 查询主机相关参数（重试次数和最大OID数）
        $host = db_fetch_row_prepared('SELECT ping_retries, max_oids FROM host WHERE hostname = ?', array($item['hostname']));
        
        // 建立SNMP会话（包含所有必要的认证参数）
        $session = cacti_snmp_session(
            $item['hostname'], 
            $item['snmp_community'], 
            $item['snmp_version'],
            $item['snmp_username'], 
            $item['snmp_password'], 
            $item['snmp_auth_protocol'], 
            $item['snmp_priv_passphrase'],
            $item['snmp_priv_protocol'], 
            $item['snmp_context'], 
            $item['snmp_engine_id'], 
            $item['snmp_port'],
            $item['snmp_timeout'], 
            $host['ping_retries'], 
            $host['max_oids']
        );

        if ($session === false) {
            $output = 'U'; // 会话建立失败
        } else {
            // 获取指定OID的数据（$item['arg1']包含OID）
            $output = cacti_snmp_session_get($session, $item['arg1']);
            $session->close(); // 关闭会话释放资源
        }

        // 验证采集结果是否有效
        if (prepare_validate_result($output) === false) {
            // 记录输出长度（调试用，实际未使用）
            $strout = (strlen($output) > 20) ? 20 : strlen($output);
            $output = 'U'; // 无效结果标记为不可用
        }
    }

    // 存储采集结果
    $return[$i]['value']         = $output;
    $return[$i]['rrd_name']      = $item['rrd_name'];      // 对应的RRD文件名
    $return[$i]['local_data_id'] = $local_data_id;         // 数据ID
    break;

// [2] 外部脚本采集处理
case POLLER_ACTION_SCRIPT: /* script (popen) */
    // 执行外部脚本命令（$item['arg1']包含完整命令）
    $output = trim(exec_poll($item['arg1']));

    // 验证采集结果
    if (prepare_validate_result($output) === false) {
        // 记录输出长度（调试用）
        $strout = (strlen($output) > 20) ? 20 : strlen($output);
        $output = 'U'; // 无效结果标记为不可用
    }

    // 存储采集结果
    $return[$i]['value']         = $output;
    $return[$i]['rrd_name']      = $item['rrd_name'];
    $return[$i]['local_data_id'] = $local_data_id;
    break;

// [3] PHP脚本服务器采集处理
case POLLER_ACTION_SCRIPT_PHP: /* script (php script server) */
    // 定义进程间通信管道
    $cactides = array(
        0 => array('pipe', 'r'), // 标准输入（子进程读取）
        1 => array('pipe', 'w'), // 标准输出（子进程写入）
        2 => array('pipe', 'w')  // 标准错误（子进程写入）
    );
    // 后续代码会继续处理...
```

查询6的时候步骤和我们是不一样的,他是直接查询
```
select * from poller_item where  local_data_id=6 and host_id=1 \G
```

这个pyload这里提交数组


<img width="608" height="155" alt="image" src="https://github.com/user-attachments/assets/b2908620-e04b-4470-8a46-98ed7609cb89" />

是因为这里要做一个循环，正常应该是提交[1,2,3,4,5,6]，他只提交了一个


<img width="656" height="428" alt="image" src="https://github.com/user-attachments/assets/987319ae-0e0c-4953-898a-bff40e924fa4" />

这里好像有个过滤


<img width="659" height="194" alt="image" src="https://github.com/user-attachments/assets/41ab1524-9ecf-45fa-b405-27545350f1ab" />

这里判断id，我们的id是int，他肯定是返回true


<img width="657" height="270" alt="image" src="https://github.com/user-attachments/assets/e978a4d9-4d07-43f2-8935-690097df3727" />


两个都没有过滤，上面因为我们传的是标准的数字所以过滤不掉，这个只是接一下返回类型


<img width="660" height="216" alt="image" src="https://github.com/user-attachments/assets/53f1c2a9-8569-4526-8898-9a53640c7db6" />

我们这里来测试一下，还是在登录页面抓个包

<img width="673" height="392" alt="image" src="https://github.com/user-attachments/assets/7312c0be-0b66-46b0-8eb7-087a86ddab2c" />

修改一下包加入payload然后发送，去看有没有成功

<img width="666" height="302" alt="image" src="https://github.com/user-attachments/assets/9e67bcac-2dfb-484f-bd3e-fcf39232a8cf" />

可以看见成功了，出现了success


<img width="650" height="220" alt="image" src="https://github.com/user-attachments/assets/d1704e2e-1b87-41b2-a3e4-ea0084e8717c" />

我们提交了之后是走到这里的

<img width="646" height="105" alt="image" src="https://github.com/user-attachments/assets/62866e42-92f5-457b-897d-cbde00fa8611" />

这里获取的值我们需要加一个参数X-Forwarded-For:127.0.0.1

​	伪造客户端 IP 为本地回环地址（伪装成 “本地请求”，可能绕过基于 IP 的访问控制）。


<img width="670" height="320" alt="image" src="https://github.com/user-attachments/assets/0148fcc5-595a-40e0-a186-aa60cd39847c" />
```
function remote_client_authorized() {
    // [1] 引入数据库连接标识
    global $poller_db_cnn_id;
    
    // [2] 禁止命令行执行
    /* don't allow to run from the command line */
    $client_addr = get_client_addr();
    if ($client_addr === false) {
        return false; // 无法获取客户端地址
    }
    
    // [3] 验证IP地址格式
    if (!filter_var($client_addr, FILTER_VALIDATE_IP)) {
        cacti_log('ERROR: Invalid remote agent client IP Address.  Exiting');
        return false; // 无效IP地址
    }
    
    // [4] 获取客户端主机名
    $client_name = gethostbyaddr($client_addr);
    
    // [5] 处理主机名解析
    if ($client_name == $client_addr) {
        // 解析失败（返回的还是IP）
        cacti_log('NOTE: Unable to resolve hostname from address ' . $client_addr, 
                 false, 'WEBUI', POLLER_VERBOSITY_MEDIUM);
    } else {
        // 去除域名部分（保留主机名）
        $client_name = remote_agent_strip_domain($client_name);
    }
    
    // [6] 获取所有采集器配置
    $pollers = db_fetch_assoc('SELECT * FROM poller', true, $poller_db_cnn_id);
    
    // [7] 验证客户端是否授权
    if (cacti_sizeof($pollers)) {
        foreach($pollers as $poller) {
            // [7.1] 比较主机名（不含域名）
            if (remote_agent_strip_domain($poller['hostname']) == $client_name) {
                return true; // 主机名匹配
            }
            // [7.2] 直接比较IP地址
            elseif ($poller['hostname'] == $client_addr) {
                return true; // IP地址匹配
            }
        }
    }
    
    // [8] 记录未授权访问
    cacti_log("Unauthorized remote agent access attempt from $client_name ($client_addr)");
    
    // [9] 拒绝访问
    return false;
}
```

这里最关键的漏洞就是get_client_addr()这里


<img width="651" height="220" alt="image" src="https://github.com/user-attachments/assets/a6c6e033-f47e-4e14-86a9-151516e28ba5" />
```
function get_client_addr($client_addr = false) {
    // 定义一个包含多种可能存放客户端IP地址的HTTP头信息的数组
    $http_addr_headers = array(
        'X-Forwarded-For',
        'X-Client-IP',
        'X-Real-IP',
        'X-ProxyUser-Ip',
        'CF-Connecting-IP',
        'True-Client-IP',
        'HTTP_X_FORWARDED',
        'HTTP_X_FORWARDED_FOR',
        'HTTP_X_CLUSTER_CLIENT_IP',
        'HTTP_FORWARDED_FOR',
        'HTTP_FORWARDED',
        'HTTP_CLIENT_IP',
        'REMOTE_ADDR',
    );

    // 初始化客户端IP地址变量为false
    $client_addr = false;
    
    // 遍历HTTP头信息数组，尝试从每个头中获取客户端IP
    foreach ($http_addr_headers as $header) {
        // 检查当前头是否存在于$_SERVER超级全局变量中且不为空
        if (!empty($_SERVER[$header])) {
            // 如果头存在且不为空，将其值按逗号分割（处理多个IP的情况）
            $header_ips = explode(',', $_SERVER[$header]);
            
            // 遍历分割后的每个IP地址
            foreach ($header_ips as $header_ip) {
                // 检查当前IP是否不为空
                if (!empty($header_ip)) {
                    // 使用filter_var函数验证当前IP是否为有效IP格式
                    if (!filter_var($header_ip, FILTER_VALIDATE_IP)) {
                        // 如果IP无效，记录错误日志
                        cacti_log('ERROR: Invalid remote client IP Address found in header (' . $header . ').', false, 'AUTH', POLLER_VERBOSITY_DEBUG);
                    } else {
                        // 如果IP有效，将其赋值给$client_addr变量
                        $client_addr = $header_ip;
                        // 记录调试日志，表明使用了哪个头中的哪个IP
                        cacti_log('DEBUG: Using remote client IP Address found in header (' . $header . '): ' . $client_addr . ' (' . $_SERVER[$header] . ')', false, 'AUTH', POLLER_VERBOSITY_DEBUG);
                        // 跳出两层循环（结束整个IP查找过程）
                        break 2;
                    }
                }
            }
        }
    }

    // 返回找到的客户端IP地址（如果没有找到则返回false）
    return $client_addr;
}
```

知道他是怎么运行之后，我们再查看外面的代码
```
function remote_client_authorized() {
    // 声明使用全局变量$poller_db_cnn_id，这是用于连接轮询器数据库的ID
    global $poller_db_cnn_id;

    /* 不允许从命令行运行 */
    // 调用get_client_addr函数获取客户端IP地址
    $client_addr = get_client_addr();
    // 如果无法获取客户端IP地址，记录日志并返回false
    if ($client_addr === false) {
        return false;
    }

    // 再次验证获取的客户端IP是否为有效格式
    if (!filter_var($client_addr, FILTER_VALIDATE_IP)) {
        cacti_log('ERROR: Invalid remote agent client IP Address.  Exiting');
        return false;
    }

    // 通过IP地址反向解析获取主机名
    $client_name = gethostbyaddr($client_addr);

    // 如果反向解析失败（返回的主机名与IP相同）
    if ($client_name == $client_addr) {
        // 记录中等详细级别的日志，提示无法解析主机名
        cacti_log('NOTE: Unable to resolve hostname from address ' . $client_addr, false, 'WEBUI', POLLER_VERBOSITY_MEDIUM);
    } else {
        // 如果解析成功，调用remote_agent_strip_domain函数去除主机名中的域名部分
        $client_name = remote_agent_strip_domain($client_name);
    }

    // 从数据库中查询所有轮询器记录
    $pollers = db_fetch_assoc('SELECT * FROM poller', true, $poller_db_cnn_id);

    // 检查是否有轮询器记录
    if (cacti_sizeof($pollers)) {
        // 遍历所有轮询器记录
        foreach($pollers as $poller) {
            // 比较去除域名后的轮询器主机名与客户端主机名是否匹配
            if (remote_agent_strip_domain($poller['hostname']) == $client_name) {
                return true; // 匹配则授权通过
            } 
            // 直接比较轮询器主机名与客户端IP是否匹配（处理IP直接配置的情况）
            elseif ($poller['hostname'] == $client_addr) {
                return true; // 匹配则授权通过
            }
        }
    }

    // 如果所有轮询器记录都不匹配，记录未授权访问日志
    cacti_log("Unauthorized remote agent access attempt from $client_name ($client_addr)");

    // 返回false表示未授权
    return false;
}
```

这里的$client_name代表的就是localhost，和$poller里面的hostname是相同的所以直接返回true，这里能返回ture，证明鉴权就绕过了

<img width="652" height="289" alt="image" src="https://github.com/user-attachments/assets/9d1ba647-a0b8-4765-adb1-7b7eabf67519" />

上面的鉴权绕过后，就走到action了，他是get请求传上来的所以用户可控

所以我们的payload就需要action=polldata，当action等于polldata的时候就走到下面poll_for_data这个函数了，然后这个上面解释了。

<img width="652" height="324" alt="image" src="https://github.com/user-attachments/assets/8445d7b1-287a-4add-84b2-dc8df502fffe" />



