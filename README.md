<center><font size="15">zigbee3.0 门锁协议</font></center>
<center>杭州软库科技有限公司</center>  
<center>2018年11月</center>  
[TOC]

## 概述
&emsp;&emsp;zigbee3.0 门锁，使用标准zigbee3.0协议实现，涉及指令 下发密码，修改密码，删除密码，清空密码，下发UTC时间, 获取特定密码,获取密码列表, 获取加密芯片SNUID

## 详细指令

<table>
    <tr>
        <td>cluster</td>
        <td>指令</td>
        <td>command/<br>attribute</td>
        <td>操作</td>
        <td>数据</td>
    </tr>
    <!-- 添加密码 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">设置密码</td>
        <td>0x05</td>
        <td>request</td>
        <td>
            User ID -- 密码下标<br>
            &emsp;&emsp;< 65440 (0xFFA0)<br>
            PinCode:<br>
            /* Pin 明文 */<br>
            uint16 option;<br>
            &emsp;&emsp;bit0 ~ 密码值, 1 有效<br>
            &emsp;&emsp;bit1 ~ valid(解冻), 1 有效<br>
            &emsp;&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
            &emsp;&emsp;bit3 ~ patrol(寻更), 1 有效<br>
            &emsp;&emsp;bit4 ~ admin(管理员), 1 有效<br>
            &emsp;&emsp;bit5 ~ time, 1有效<br>
            &emsp;&emsp;bit6 ~ count(次数), 1 有效<br>
            uint32 passScndVal;<br>
            uint32 startTime;<br>
            uint32 endTime;<br>
            uint32 passVal;<br>
            uint16 mask;<br>
            uint8 passwdLen;<br>
            /* Pin 密文 */<br>
            uint8 data[32];<br>
        </td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x02 -- PIN Code Added<br>
            UserId: 密码下标 < 0xffa0<br>
            Status:0 -- 能使用, 0xfe -- 相似密码, 0xff -- 不能使用<br>
        </td>
    </tr>
    <!-- 修改密码 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">修改密码</td>
        <td>0x05</td>
        <td>request</td>
        <td>
            User ID -- 密码下标<br>
            &emsp;&emsp;< 65440 (0xFFA0)<br>
            Pin -- 可变数据区<br>
            /* Pin 明文 */<br>
            uint16 option;<br>
            &emsp;&emsp;bit0 ~ 密码值, 1 有效<br>
            &emsp;&emsp;bit1 ~ valid(解冻), 1 有效<br>
            &emsp;&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
            &emsp;&emsp;bit3 ~ patrol(寻更), 1 有效<br>
            &emsp;&emsp;bit4 ~ admin(管理员), 1 有效<br>
            &emsp;&emsp;bit5 ~ time, 1有效<br>
            &emsp;&emsp;bit6 ~ count(次数), 1 有效<br>
            uint32 passScndVal;<br>
            uint32 startTime;<br>
            uint32 endTime;<br>
            uint32 passVal;<br>
            uint16 mask;<br>
            &emsp;&emsp;bit0 ~ 密码值, 1 有效<br>
            &emsp;&emsp;bit1 ~ valid(解冻), 1 有效<br>
            &emsp;&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
            &emsp;&emsp;bit3 ~ patrol(寻更), 1 有效<br>
            &emsp;&emsp;bit4 ~ admin(管理员), 1 有效<br>
            &emsp;&emsp;bit5 ~ time, 1有效<br>
            &emsp;&emsp;bit6 ~ count(次数), 1 有效<br>
            uint8 passwdLen;<br>
        </td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x04 -- PIN Code Changed<br>
            UserId: 密码下标<br>
            Status:0 -- 能使用, 0xfe -- 相似密码, 0xff -- 不能使用<br>
        </td>
    </tr>
    <!-- 删除密码 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">删除密码</td>
        <td>0x07</td>
        <td>request</td>
        <td>
            User ID -- 密码下标<br>
            &emsp;&emsp;< 65440 (0xFFA0)<br>
        </td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x03 -- PIN Code Deleted<br>
            UserId: 密码下标<br>
            Status:0 -- 能使用, 0xff -- 不能使用<br>
        </td>
    </tr>
    <!-- 清除密码 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">清除密码</td>
        <td>0x08</td>
        <td>request</td>
        <td></td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x03 -- PIN Code Deleted<br>
            UserId: 0xffff<br>
            Status:0 -- 能使用, 0xff -- 不能使用<br>
        </td>
    </tr>
    <!-- 获取密码(单个) -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">获取密码(单个)</td>
        <td >0x06</td>
        <td>request</td>
        <td>
            User ID: 密码下标 (< 65440(0xffa0)) <br>
        </td>
    </tr>
    <tr>
    	<td>0x06</td>
        <td>response</td>
        <td>
            User ID: 密码下标 (< 65440(0xffa0)) <br>
            status: 0 -- 有， 0xff -- 无<br>
            Pin Code:<br>
            &emsp;&emsp;uint16 option;<br>
            &emsp;&emsp;uint32 passScndVal;<br>
            &emsp;&emsp;uint32 startTime;<br>
            &emsp;&emsp;uint32 endTime;<br>
            &emsp;&emsp;uint32 passwd;<br>
            &emsp;&emsp;uint8 passwdLen;<br>
            &emsp;&emsp;uint8 status;<br>
            &emsp;&emsp;uint8 allLen;<br>
            &emsp;&emsp;uint8 curNo;<br>
        </td>
    </tr>
    <!-- 获取密码列表 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">获取总密码列表</td>
        <td rowspan="2">0x06</td>
        <td>request</td>
        <td>
            User ID:0xffff
        </td>
    </tr>
    <tr>
        <td>response</td>
        <td>
            User ID:0xffff<br>
            Status: 0 -- 成功, 0xff -- 失败<br>
            Pin Code: <br>
            &emsp;&emsp;uint8 allItems; // 总密码数<br>
            &emsp;&emsp;uint8 startIdx; // 密码个数下标<br>
            &emsp;&emsp;uint8 size;     // 当前报文密码个数<br>
            &emsp;&emsp;uint16 userId[10];// 密码ID
        </td>
    </tr>
	<!-- 获取密码列表 分段-->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">获取密码列表 分段<br>(总列表中丢失部分分段上报)</td>
        <td>0x06</td>
        <td>request</td>
        <td>
            User ID:0xfff0 ~ 0xfff9<br>
            &emsp;&emsp;0xfff0: startIdx 0 -- 9<br>
            &emsp;&emsp;0xfff1: startIdx 10 -- 19<br>
            &emsp;&emsp;....<br>
            &emsp;&emsp;0xfff9: startIdx 90 -- 99<br>
        </td>
    </tr>
    <tr>
    	<td>0x06</td>
        <td>response</td>
        <td>
            User ID:0xfff0 ~ 0xfff9<br>
            Status: 0 -- 成功, 0xff -- 失败<br>
            Pin Code: <br>
            &emsp;&emsp;uint8 allItems; // 总密码数<br>
            &emsp;&emsp;uint8 startIdx; // 密码个数下标<br>
            &emsp;&emsp;uint8 size;     // 当前报文密码个数<br>
            &emsp;&emsp;uint16 userId[10];// 密码ID
        </td>
    </tr>
    <!-- 根据状态查询密码 -->
    <tr>
        <td rowspan = "2">0x0101</td>
        <td rowspan = "2">根据状态查询密码</td>
        <td rowspan = "2">0x06</td>
        <td>request</td>
        <td>
            User ID:0xfffe -- 查询未被冻结密码<br>
            &emsp;&emsp; 0xfffd-- 查询已冻结密码<br>
        </td>
    </tr>
    <tr>
        <td>response</td>
        <td>
            User ID:0xfffe, 0xfffd<br>
            Status: 0 -- 成功， 0xff -- 失败<br>
            Pin Code:<br>
            &emsp;&emsp;uint8 allItems; // 总密码数<br>
            &emsp;&emsp;uint8 startIdx; // 密码个数下标<br>
        </td>
    </tr>
    <!-- 获取加密芯片信息-->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">获取加密芯片信息</td>
        <td rowspan="2">0x06</td>
        <td>request</td>
        <td>
            User ID: 65504(0xffe0) <br>
        </td>
    </tr>
    <tr>
        <td>response</td>
        <td>
            User ID: 65504(0xffe0)<br>
            status: 0 -- 成功， 0xff -- 失败<br>
            Pin Code:<br>
            &emsp;&emsp;uint8 sn[8];// SN<br>
            &emsp;&emsp;uint8 space;// 空格<br>
            &emsp;&emsp;uint8 uid[8];// UID<br>
        </td>
    </tr>
    <!-- 一键开门 -->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">一键开门</td>
        <td>0x05</td>
        <td>request</td>
        <td>
            User ID -- 65488(0xffd0)<br>
            Pin Code:<br>
            &emsp;&emsp;uint8 data[32];//设置为0<br>
        </td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x02 -- PIN Code Added<br>
            UserId: 65488(0xffd0)<br>
            Status: 0 -- 成功, 0xff -- 失败<br>
        </td>
    </tr>
    <!--- 动态密码种子比对 --->
    <tr>
        <td rowspan="2">0x0101</td>
        <td rowspan="2">动态密码种子比对</td>
        <td>0x05</td>
        <td>request</td>
        <td>
            User ID -- 65440(0xffa0)<br>
            Pin Code:<br>
             uint16 option;<br>
            &emsp;&emsp;bit0 ~ 密码值, 1 有效<br>
            &emsp;&emsp;bit1 ~ valid(解冻), 1 有效<br>
            &emsp;&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
            &emsp;&emsp;bit3 ~ patrol(寻更), 1 有效<br>
            &emsp;&emsp;bit4 ~ admin(管理员), 1 有效<br>
            &emsp;&emsp;bit5 ~ time, 1有效<br>
            &emsp;&emsp;bit6 ~ count(次数), 1 有效<br>
            uint32 passScndVal;<br>
            uint32 startTime;<br>
            uint32 endTime;<br>
            uint32 passVal;<br>
            uint16 mask;<br>
            uint8 passwdLen;<br>
        </td>
    </tr>
    <tr>
        <td>0x21</td>
        <td>response</td>
        <td>
            program event source:0x01 -- RF<br>
            program event code:0x02 -- PIN Code Added<br>
            User ID -- 65440(0xffa0)<br>
            Status:0 -- 比对成功, 0xf9 -- 比对失败<br>
        </td>
    </tr>
    <!-- 本地密码修改事件上报 -->
    <tr>
        <td>0x0101</td>
        <td>本地密码修改</td>
        <td>0x21</td>
        <td>report</td>
        <td>
            program event source: 0x00 -- Keypad<br>
            program event code: 0x04 -- PINCodeChanged<br>
            User ID -- 密码ID <br>
            Status:0 -- 成功
        </td>
    </tr>
    <!-- 开门事件上报 -->
    <tr>
        <td>0x0101</td>
        <td>开门事件上报</td>
        <td>0x20</td>
        <td>report</td>
        <td>
            Operation Event Source:0x00 -- Keypad<br>
            Operation Event Code:0x02 -- Unlock<br>
            User ID：密码ID<br>
            data:上报密码属性值option
        </td>
    </tr>
    <!-- 门状态事件上报 -->
    <tr>
        <td>0x0101</td>
        <td>门状态</td>
        <td>0x0003</td>
        <td>report</td>
        <td>
            0x00 -- Opened,<br>
            0x01 -- Closed
        </td>
    </tr>
    <!-- 报警 -->
   <tr>
       <td>0x0009</td>
       <td>报警</td>
       <td>0x01</td>
       <td>report</td>
       <td>
           Alarm code: <br>
           &emsp;&emsp;0x04 ---- 输错次数过多，系统锁定<br>
           &emsp;&emsp;0x06 ---- 锁遭破坏<br>
           &emsp;&emsp;0x10 ---- 电量过低<br>
           ClusterId:<br>
           &emsp;&emsp;对应cluster Id<br>
           Time stamp:<br>
           &emsp;&emsp;时间戳
       </td>
   </tr>
   <!-- 电池电量上报 -->
   <tr>
       <td>0x0001</td>
       <td>电池电量上报</td>
       <td>0x0021</td>
       <td>report</td>
       <td>
           电量值
       </td>
   </tr>
   <!-- 设置UTC时间 -->
   <tr>
        <td>0x000a</td>
        <td>时间同步</td>
        <td>0x00</td>
        <td>command</td>
        <td>
            UTC:时间戳
        </td>
    </tr>
</table>





## Door Lock Cluster(0x0101)
&emsp;&emsp; 门锁实现下发指令：门状态上报,下发密码，删除密码，修改密码，查询密码。zigbee3.0 标准协议  <br>  
#### Attribute:
 <table>
     <tr>
         <td>Attribute ID</td>
         <td>Name</td>
         <td>Type</td>
         <td>Data</td>
     </tr>
     <tr>
        <td>0x0003</td>
        <td>DoorState</td>
        <td>enum8</td>
        <td>00 -- opened<br>/01 -- closed</td>
    </tr>
 </table>

#### Set PIN Code (0x05)
实现下发密码，修改密码.
##### Commond
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
        <td>1</td>
        <td>1</td>
        <td>variable</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint16</td>
        <td>uint8</td>
        <td>enum8</td>
        <td>octstr</td>
    </tr>
    <tr>
        <td>File Name</td>
        <td>User ID</td>
        <td>User Status</td>
        <td>User Type</td>
        <td>PIN</td>
    </tr>
</table>

UserID ---- 密码ID 号<br>
User Status ---- 0x00<br>
User Type ---- 0x00<br>
PIN ---- 具体数据区:
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
        <td>4</td>
        <td>4</td>
        <td>4</td>
        <td>4</td>
        <td>2</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint16</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint16</td>
        <td>uint8</td>
    </tr>
    <tr>
        <td>Descriptor</td>
        <td>option：具体选项</td>
        <td>passwdScdVal:为次数密码时表示次数<br>
            为动态密码时为时间间隔，单位秒</td>
        <td>有效开始时间</td>
        <td>结束时间</td>
        <td>密码值</td>
        <td>mask:使能项目</td>
        <td>密码值长度</td>
    </tr>
</table>
option,mask:<br>
&emsp;bit0 ~ 密码值, 1 有效<br>
&emsp;bit1 ~ valid(解冻), 1 有效<br>
&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
&emsp;bit3 ~ patrol(寻更), 1 有效<br>
&emsp;bit4 ~ admin(管理员), 1 有效<br>
&emsp;bit5 ~ time, 1有效<br>
&emsp;bit6 ~ count(次数), 1 有效<br>

##### Response
<table>
    <tr>
        <td>Byte</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>Status</td>
    </tr>
</table>
Status---- 0 Sucess<br>
&emsp;---- 1 General Failure<br>
&emsp;---- 2 Memory full<br>
&emsp;---- 3 Duplicate Code Error<br>
#### Get PIN Code (0x06)
获取密码
##### Commond
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Data Types</td>
        <td>uint16</td>
    </tr>
    <tr>
        <td>File Name</td>
        <td>User ID</td>
    </tr>
</table>
##### Response
上报密码时不上报密码值。
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
        <td>1</td>
        <td>1</td>
        <td>Variable</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint16</td>
        <td>uint8</td>
        <td>enum8</td>
        <td>octstr</td>
    </tr>
    <tr>
        <td>Filed Name</td>
        <td>User ID</td>
        <td>User Status</td>
        <td>User Type</td>
        <td>PIN</td>
    </tr>
</table>
PIN ---- 具体数据区:
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
        <td>4</td>
        <td>4</td>
        <td>4</td>
        <td>4</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint16</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint32</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint8</td>
    </tr>
    <tr>
        <td>Descriptor</td>
        <td>option：具体选项</td>
        <td>passScndVal:次数</td>
        <td>有效开始时间</td>
        <td>结束时间</td>
        <td>密码值</td>
        <td>密码值长度</td>
        <td>密码状态</td>
        <td>总数</td>
        <td>当前值</td>
    </tr>
</table>
option:<br>
&emsp;bit0 ~ 密码值, 1 有效<br>
&emsp;bit1 ~ valid(解冻), 1 有效<br>
&emsp;bit2 ~ dynamic(动态密码), 1 有效<br>
&emsp;bit3 ~ patrol(寻更), 1 有效<br>
&emsp;bit4 ~ admin(管理员), 1 有效<br>
&emsp;bit5 ~ time, 1有效<br>
&emsp;bit6 ~ count(次数), 1 有效<br>

#### Clear PIN Code (0x07)
删除密码.
##### Commond
<table>
    <tr>
        <td>Bytes</td>
        <td>2</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint16</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>User ID</td>
    </tr>
</table>
如果user ID 不存在， UserId = requested UserId, UserStatus = 0, UserType = 0xff (not Supported).
##### Response
<table>
    <tr>
        <td>Bytes</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>Status</td>
    </tr>
    <tr>
        <td>Field Value</td>
        <td>0 -- pass<br>
            1 -- fail
        </td>
    </tr>
</table>
#### Clear ALL PIN Codes (0x08)
删除所有密码.
##### Commond
直接下发指令，不带任何载荷数据<br>
##### Response
<table>
    <tr>
        <td>Byte</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>Status</td>
    </tr>
    <tr>
        <td>Field Value</td>
        <td>0 = pass <br>
            1 = faile<br>
        </td>
    </tr>
</table>

#### Operating Event Notification(0x20)
操作事件通知
##### Command
<table>
    <tr>
        <td>Octets</td>
        <td>1</td>
        <td>1</td>
        <td>2</td>
        <td>Variable/0</td>
        <td>4</td>
        <td>Variable/0</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint16</td>
        <td>string</td>
        <td>uint32</td>
        <td>string</td>
    </tr>
    <tr>
        <td>File Name</td>
        <td>Operation Event Source</td>
        <td>Operation Event Code</td>
        <td>User ID</td>
        <td>Pin</td>
        <td>UTC</td>
        <td>Data</td>
    </tr>
</table>
Operation Event Source:<br>
&emsp;Keypad ---- 0x00  <br>
&emsp;RF ---- 0x01<br>
&emsp;Manual ---- 0x02<br>
Operation Event Codes:<br>
&emsp;Unlock ---- 0x02<br>
User ID: 用户密码ID<br>
PIN:空<br>
UTC: 时间戳<br>
Data:16进制转为字符串<br>
&emsp;Option ----


#### Programming Event Notification(0x21)
编程事件通知
##### Command
<table>
    <tr>
        <td>Octets</td>
        <td>1</td>
        <td>1</td>
        <td>2</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>4</td>
        <td>Variable</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint16</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>uint32</td>
        <td>string</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>Program Event Source</td>
        <td>Program Event Code</td>
        <td>User ID</td>
        <td>PIN</td>
        <td>User Type</td>
        <td>User Status</td>
        <td>UTC</td>
        <td>Data</td>
    </tr>
</table>
Program Event Source:<br>
&emsp;Keypad ---- 0x00<br>
&emsp;RF ---- 0x01<br>
&emsp;Manual ---- 0x02<br>
Program Event Codes:<br>
&emsp;PINCodeAdded ---- 0x02<br>
&emsp;PINCodeDeleted ---- 0x03<br>
&emsp;PINCodeChanged ---- 0x04<br>
User ID: 密码ID<br>
PIN：空<br>
User Type: 0x00<br>
User Status:<br>
&emsp;0x00 ---- 成功<br>
&emsp;0xf9 ---- 种子比对失败<br>
&emsp;0xfa ---- 解密失败<br>
&emsp;0xfb ---- 密码存储失败<br>
&emsp;0xfc ---- 密码长度不对<br>
&emsp;0xfd ---- 密码表满<br>
&emsp;0xfe ---- 密码相似，不可用<br>
&emsp;0xff ---- 密码不可用<br>
UTC: 时间戳<br>
Data:空<br>

## Power Configuration Cluster(0x0001)
&emsp;&emsp; 功耗管理，电量上报<br>  
#### Attribute:
 <table>
     <tr>
         <td>Attribute ID</td>
         <td>Name</td>
         <td>Type</td>
         <td>Data</td>
     </tr>
     <tr>
        <td>0x0021</td>
        <td>BatteryPercentageRemaining</td>
        <td>uint8</td>
        <td>0x00 -- 0xff</td>
    </tr>
 </table>

## Alarm Cluster(0x0009)
#### Get alarm Response(0x01)
 <table>
    <tr>
        <td>Octets</td>
        <td>1</td>
        <td>1</td>
        <td>2</td>
        <td>4</td>
    </tr>
    <tr>
        <td>Data Type</td>
        <td>uint8</td>
        <td>uint8</td>
        <td>clusterId(uint16)</td>
        <td>uint32</td>
    </tr>
    <tr>
        <td>Field Name</td>
        <td>Status</td>
        <td>Alarm Code</td>
        <td>Cluster ID</td>
        <td>UTC</td>
    </tr>
</table>

## Time Cluster（0x000a)

#### Attribute:
 <table>
     <tr>
         <td>Attribute ID</td>
         <td>Name</td>
         <td>Type</td>
         <td>Data</td>
     </tr>
     <tr>
        <td>0x0000</td>
        <td>Time</td>
        <td>UTC(4Byte)</td>
        <td>0x00 -- 0xfffffffe</td>
    </tr>
 </table>
 UTC:时间戳
