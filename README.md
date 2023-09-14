# tdsql
腾讯TDSQL接口未授权访问信息泄露
CVE-2023-42387

漏洞地址：
http://tdsql-xxxxxxx.com/tdsqlpcloud/index.php/api/install/get_db_info

漏洞描述：
tdsql赤兔管理平台，api接口存在未授权返回数据库明文配置信息。

漏洞详情：
代码审计
<img width="919" alt="企业微信截图_e145d467-1516-4bc4-b803-ef885051b2f8(1)" src="https://github.com/ranhn/TDSQL/assets/107679328/61bdb224-fdb1-460d-88ce-28e8368fd7ce">

1，访问上述接口。
![image](https://github.com/ranhn/TDSQL/assets/107679328/1ab3cadb-497a-4546-a707-8f3c2b8ff863)
2，得到明文账号密码，登录数据库。
![image](https://github.com/ranhn/TDSQL/assets/107679328/7e234dea-a837-4594-b1d3-7941eab50d29)

漏洞版本：
赤免管理台 V1.8.9-1 bdffe65





![企业微信截图_16939844855432](https://github.com/ranhn/TDSQL/assets/107679328/82013cb8-6983-4b72-ad55-ad70a415c793)

修复建议：


class Install extends API_Controller{
public function __construct(){
parent::__construct(false); --修改为true
}
/**
* 获取数据库访问信息
*
* @author kevenchen
* @type GET/POST
*
* @return string ip 数据库IP
* @return string port 数据库端口
* @return string user 数据库账号
* @return string pwd 数据库密码
*
* @example {url}
/
public function get_db_info(){ – public修改为protect
$this->config->load(‘database’);
$dbconfs = $this->config->item(‘dbconfs’);
$dbdefault = $this->config->item(‘dbdefault’);
$conf = $dbconfs[$dbdefault];
$info = array(
‘ip’ => $conf[‘hostname’],
‘port’ => $conf[‘port’],
‘user’ => $conf[‘username’],
‘pwd’ => $conf[‘password’],
);
echo json_encode($info);
}
/*
