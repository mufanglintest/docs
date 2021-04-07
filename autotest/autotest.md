# 测试框架使用说明
## 一、环境准备
### 下载IDEA并使用
进入idea官网[IntelliJ IDEA: The Java IDE for Professional Developers by JetBrains](http://www.jetbrains.com/idea/)，点击下载，进入下载页面，选择一个合适的版本进行下载。如下图：
![8d28c4b3.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/8d28c4b3.png)
![9347b69d.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/9347b69d.png)
**破解方式请找度娘解决**
### JDK配置
> mac版

- 先安装homebrew，然后通过该工具安装JDK

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- 再执行以下命令即可完成

```
brew cask search java
brew cask info java
brew cask install java
#检查java版本
java -version
```

![ad2699d6.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/ad2699d6.png)

> windows

请参考该文章就不多讲了[Java 开发环境配置](http://www.runoob.com/java/java-environment-setup.html)

### git安装
> mac

与安装jdk一样，直接使用homebrew工具安装就好

```
brew install git
#安装完成后查看版本
git --version
```
![5242e19d.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/5242e19d.png)

> windows

参考该文章进行windows环境下的git安装[Git安装教程（Windows安装超详细教程）](https://www.jianshu.com/p/414ccd423efc)

### maven配置
- 进入到Apache官网找到Maven的下载页面[Maven – Download Apache Maven](https://maven.apache.org/download.cgi?Preferred=http%3A%2F%2Fmirrors.shu.edu.cn%2Fapache%2F)下载Maven。如下图：
![6334e35b.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/6334e35b.png)

- 进入到/Users/mfl/work/apache-maven-3.5.0/conf目录设置settings.xml文件。可到[这里](http://acooly.cn/docs/res/maven/maven-settings-acooly.xml)下载并将conf目录下的settings.xml进行替换。
- 再对其进行环境变量的设置，如下：
> mac

```
#编辑该文件
vim .bash_profile
```
```
#在该文件中加入以下几行
M2_HOME=/Users/mfl/work/apache-maven-3.5.0
export M2_HOME
PATH=$PATH:$M2_HOME/bin
export PATH
```
```
#让环境变量生效
source .bash_profile
```

> windows

请参考[Windows环境下maven 安装与环境变量配置](https://www.cnblogs.com/liuhongfeng/p/5057827.html)该文章进行设置

**好了，基本的环境已经配置完成了，接下来我们开始准备写第一个测试用例了。**

## 二、编写用例
### 导入测试工程
#### 1.拉代码
使用以下命令进行代码拉取

```
git clone http://username:password@gitlab.acooly.cn/prosysoft/autotest/auto-test.git
```
#### 2.打开IDEA
这个应该不用我教了吧，不会的自己剁手
#### 3.点击"open"
![4bce6d34.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/4bce6d34.png)
#### 4.选择测试工程所在路径
![16d3359a.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/16d3359a.png)
### IDEA设置
#### 1.基本设置
打开IDEA后，点击"file->import settings",选择一个setting.jar包导入，可点击[这里](http://acooly.cn/docs/res/ide/idea/idea_settings_acooly.jar)下载一个。
#### 2.maven设置
打开IDEA后点击"Preferences -> Maven"进行设置，如下图：
![b1331709.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/b1331709.png)

>Maven home directory :设置本地maven目录
>
>User settings file :选择本地maven目录conf下的settings.xml文件
>
>Locl repository :选择本地jar包存放路径

### 工程目录介绍

![a6ee2ab8.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/a6ee2ab8.png)

#### 1.assemble
>该目录下存放系统启动配置文件

![0d5312fa.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/0d5312fa.png)
**测试工程的数据库、dubbo、redis配置都在该文件里进行修改**

#### 2.core
>该目录下存放所有的公共类以及生成的dao、entity、service

![ffb952b7.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/ffb952b7.png)
- base目录存放各个系统的base类，可在此类中编写操作数据库的方法
- common目录存放各个工具类
- 其它各个目录按系统名称区分，存放各个系统dao、entity、service类
#### 3.facade
该目录在测试工程中一般不使用
#### 4.test
>该目录存放所有的测试用例

![15c895fb.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/15c895fb.png)
- base目录下存放基础类
- facade目录下编写facade接口测试用例
- openapi目录下编写api接口测试用例
- test目录下编写测试工具类

### 编写我的第一条测试用例
#### 1.编辑配置文件
进入assemble目录下的resources,打开application.properties文件，编辑相关参数。

```
#测试对应该系统的数据库地址及库名
acooly.ds.url=jdbc:mysql://192.168.55.32:3306/hunjiaclass_test
#数据库用户名
acooly.ds.username=root
#数据库密码
acooly.ds.password=123456
#是否启用jpa功能
acooly.jpa.enable=true
#数据源前缀
acooly.mybatis.multi.acooly.dsPrefix=acooly.ds
# dao包路径，位于此包下的dao会使用accout数据库
acooly.mybatis.multi.acooly.scanPackage=com.autotest.hjc
# 配置为主数据库，多个数据源时只能配置一个主数据库
acooly.mybatis.multi.acooly.primary=true
```

如果我们的被测对象对单系统，那我们只需要配置一个数据库连接即可，但是在我们的测试过程中很多情况会遇到soa的系统，那么我们就需要连接多个数据库，对应多个系统，我们只需要在配置文件中启用多数据源功能，并配置相应的数据源即可。

```
#启用多数据源支持
acooly.mybatis.supportMultiDataSource=true
#此数据库作为记录测试用例执行结果使用，提供后台查看结果
mock.ds.url=jdbc:mysql://192.168.55.32:3306/mock_test
mock.ds.username=root
mock.ds.password=123456
#配置mock
acooly.mybatis.multi.mock.dsPrefix=mock.ds
acooly.mybatis.multi.mock.scanPackage=com.autotest.mock
```

以上配置为我多加了一个mock系统的数据源配置，如果有更多的系统就按照以上方式进行配置即可。
**注意:数据源的默认前缀为acooly.ds此项目不能修改**
在soa的系统中我们可能会进行facade的接口测试，那么我们就需要对dubbo进行配置。

```
#服务拥有者
acooly.dubbo.owner=mufanglin
#zk的注册地址
acooly.dubbo.zkUrl=192.168.55.36:2181
#服务注册端口
acooly.dubbo.provider.port=2183
acooly.dubbo.provider.register=true
```

在一场景中我们可能还会使用到缓存，那么我们就需要进行redis的配置。

```
#redis地址
spring.redis.host=192.168.55.36
#redis端口
spring.redis.port=6379
```
#### 2.实体生成
- 进入test目录下的resources，打开application.properties文件，编辑相关参数

```
#要生成dao、实体类的数据源
jdbc.url=jdbc:mysql://localhost:3306/hunjiaclass?useUnicode=true&amp;characterEncoding=UTF-8
jdbc.username=root
jdbc.password=12345678
```
**其它的配置在该文件中都有注释，可自行查看。**
- 打开AcoolyCoder类，对其需要生成的实体类的目录进行指定，以及指定那些表需要生成实体类。

```
#该方法中的参数如果为*时就表示生成所有表，如果要生成多个表用逗号分隔"table_a","table_b"
service.generateTable("b_member_relation");

#此方法中对其生成的实体类进行包路径的指定，此处对应了数据源的包扫描路径
private static String getRootPackage() {
		return "com.autotest.hjc";
	}
```

配置完成后直接运行该类中的main函数即可生成dao、实体类，到指定的目录下。
**注意：使用此方法进行实体生成时会将之前生成的文件全部覆盖，所以生成时一定要十分小心，建议将其指定生成到temp包路径下，然后再将其复制到对应的包下**
#### 3.新建base类
生成完dao和实体类后，我们在core的base目录下新建一个系统对应的base类，在此类中注入所需要的dao和service，以及在这其中编写我们所需要的方法。

```
@Service("mockBase")
@Data
public class MockBase {

	@Resource
	TestcaseReportDao testcaseReportDao;

	public void insertTestCaseReport(String testclass, String testmethod, Integer testid,
									 String description, String input, String output,
									 Integer isassert) {
		TestcaseReport testcaseReport = new TestcaseReport();
		testcaseReport.setDescription(description);
		testcaseReport.setInput(input);
		testcaseReport.setOutput(output);
		testcaseReport.setIsassert(isassert);
		testcaseReport.setTestclass(testclass);
		testcaseReport.setTestmethod(testmethod);
		testcaseReport.setTestid(testid);
		testcaseReportDao.insert(testcaseReport);
	}

}
```

#### 4.编写dao的查询
在我们的测试用例中可能使用到一些查询数据的方法，在这里我们可以直接在之前生成的dao中编写我们的查询方法。

```
public interface ApiAuthDao extends EntityMybatisDao<ApiAuth> {
	
	@Delete("delete from api_auth where access_key = #{accessKey}")
	void deleteApiAuthByAccessKey(@Param("accessKey") String accessKey);
	
	@Select("select * from api_auth where access_key = #{accessKey}")
	ApiAuth findApiAuthByAccessKey(@Param("accessKey") String accessKey);
	
}
```

#### 5.编写测试用例
##### 1）新建测试类
在test目录下新建一个类继承TestBase类，用例的类名格式：service名+测试方法名+Test，一个完成的测试用例有三个cases组成：
- 成功用例(Success)：所有成功测试用例
- 失败用例一(FailOne)：参数异常（必填非必填，非空等）、等价类、边界值的异常用例
- 失败用例二(FailTwo)：业务逻辑上的异常

测试用例方法名命名格式：类名(首字母小写)+对应测试用例情况(Success/FailOne/FailTwo)

```
@Slf4j
public class NetBankPayFacadeAppPayTest extends TestBase {

	public void netBankPayFacadeAppPayTestSuccess() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailOne() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailTwo() {
    ....
	}
}
```

在测试类中注入我们需要的bean，比如之前我们创建的base类，以及我们测试的dubbo服务service，如果是测试API接口则不需要注入service服务。

```
@Slf4j
public class NetBankPayFacadeAppPayTest extends TestBase {
	@Resource
	FintechBase			fintechBase;
	
	@Reference(version = "1.0")
	NetBankPayFacade netBankPayFacade;
  
	public void netBankPayFacadeAppPayTestSuccess() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailOne() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailTwo() {
    ....
	}
}
```

**特别提醒：当我们的被测对象为dubbo接口时，我们需要先将其facade包导入工程，在我们的主pom文件中加入包依懒**

```
<dependency>
    <groupId>com.acooly</groupId>
    <artifactId>acooly-component-safety</artifactId>
    <version>4.1.0-SNAPSHOT</version>
</dependency>
```

在执行的测试方法上加上注解
- @Test:是Junit框架中标识这个方法为测试方法注解
- @CsvParameter:将Csv文件参数化注解，其中value参数为读取的csv文件路径（**特别说明：在执行测试方法时如果csv文件不存在会在value值配置的路径下生成一个csv文件**）
- @TestCaseName:标识测试用例说明注解，其中参数名称可以随意定义，大括号中的数字表示取csv文件的第几列的值（下标从0开始），一般我们会定义两个参数，testId表示执行的用例编号，description表示该条用例的说明

```
@Slf4j
public class NetBankPayFacadeAppPayTest extends TestBase {
	@Resource
	FintechBase			fintechBase;
	
	@Reference(version = "1.0")
	NetBankPayFacade netBankPayFacade;
  
	@Test
	@CsvParameter(value = "facade/gateway/netBankPayFacadeAppPayTestSuccess.csv")
	@TestCaseName("testId={0},description={1}")
	public void netBankPayFacadeAppPayTestSuccess() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailOne() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailTwo() {
    ....
	}
}
```

加好需要的注解后在方法中编写具体的实现
- 清除数据：在进行测试前先将垃圾数据清除掉。
- 准备数据：完成垃圾数据的清理后再准备我们所需要的前置数据，例如我们所需要的会员账号等。
- 测试过程：准备调用我们被测对象所需要的参数
- 调用接口：向我们的被测对象发送请求（可能是dubbo服务，也可能是API接口）
- 结果验证：验证我们被测对象的返回数据是否与我们的期望值一致
- 数据验证：验证数据库的值是否与我们期望的一致
- 清除数据：完成测试后将我们该条用例产生的数据，防止数据干扰（**在这里我们尽量指定唯一条件进行删除，防止用例间的干扰**）

```
@Slf4j
public class NetBankPayFacadeAppPayTest extends TestBase {
	@Resource
	FintechBase			fintechBase;
	
	@Reference(version = "1.0")
	NetBankPayFacade netBankPayFacade;
  
	@Test
	@CsvParameter(value = "facade/gateway/netBankPayFacadeAppPayTestSuccess.csv")
	@TestCaseName("testId={0},description={1}")
	public void netBankPayFacadeAppPayTestSuccess() {
    //清除数据
    fintechBase.getMusNetbankPaymentTxnDao().deleteMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);  
    //准备数据
    BMember bMember = fintechBase.getBMemberEntityService().inserBMember(status, parentid,
				parentUserNo, insertUserNo, username, password, salt, mobileNo, email,
				insertRealName, certNo, userType, grade, comments);
    
    //测试过程
    AppPayOrder appPayOrder = new AppPayOrder();
    appPayOrder.setAppId(appId);
    appPayOrder.setIsRaw(isRaw);
    appPayOrder.setMchCreateIp(mchCreateIp);
    appPayOrder.setAmount(amount);
    appPayOrder.setGoodsMemo(goodsMemo);
    appPayOrder.setGatewayTradeType(gatewayTradeType);
    appPayOrder.setGatewayChannelApi(gatewayChannelApi);
    appPayOrder.setGatewayChannelType(gatewayChannelType);
    appPayOrder.setBizOrderNo(bizOrderNo);
    appPayOrder.setMerchOrderNo(merchOrderNo);
    appPayOrder.setGid(gid);
    appPayOrder.setPartnerId(partnerId);    
    
    //调用接口
    AppPayResult result = netBankPayFacade.appPay(appPayOrder);
    logger.info("resultCode:{},resultMessage:{}", result.getCode(), result.getDetail());    
    
    //结果验证
    TestUtil.assertEqualsProcessing(result);
    
    //数据验证
    MusNetbankPaymentTxn musNetbankPaymentTxn = fintechBase.getMusNetbankPaymentTxnDao().selectMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);
    TestUtil.assertEquals("BP", musNetbankPaymentTxn.getStatus());
    TestUtil.assertEquals(gid, musNetbankPaymentTxn.getGid());    
    //清除数据
    fintechBase.getMusNetbankPaymentTxnDao().deleteMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);
	}
	
	public void netBankPayFacadeAppPayTestFailOne() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailTwo() {
    ....
	}
}
```

在方法上添加我们所需要的参数

```
@Slf4j
public class NetBankPayFacadeAppPayTest extends TestBase {
	@Resource
	FintechBase			fintechBase;
	
	@Reference(version = "1.0")
	NetBankPayFacade netBankPayFacade;
  
	@Test
	@CsvParameter(value = "facade/gateway/netBankPayFacadeAppPayTestSuccess.csv")
	@TestCaseName("testId={0},description={1}")
	public void netBankPayFacadeAppPayTestSuccess(int testId, String description, String resultCode,
												//基本参数
												String returnUrl, String notifyUrl, String signType,
												String version, String context, String service,
												String partnerId, String merchOrderNo,
												String requestNo, String securityCheckKey,
												//业务参数
												String a, String userNo,
												//会员账务表数据
												String a1, String accountNo, Long userId,
												String accountType, String username, Long balance,
												Long freeze, String comments, String status) {
    //清除数据
    fintechBase.getMusNetbankPaymentTxnDao().deleteMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);  
    //准备数据
    BMember bMember = fintechBase.getBMemberEntityService().inserBMember(status, parentid,
				parentUserNo, insertUserNo, username, password, salt, mobileNo, email,
				insertRealName, certNo, userType, grade, comments);
    
    //测试过程
    AppPayOrder appPayOrder = new AppPayOrder();
    appPayOrder.setAppId(appId);
    appPayOrder.setIsRaw(isRaw);
    appPayOrder.setMchCreateIp(mchCreateIp);
    appPayOrder.setAmount(amount);
    appPayOrder.setGoodsMemo(goodsMemo);
    appPayOrder.setGatewayTradeType(gatewayTradeType);
    appPayOrder.setGatewayChannelApi(gatewayChannelApi);
    appPayOrder.setGatewayChannelType(gatewayChannelType);
    appPayOrder.setBizOrderNo(bizOrderNo);
    appPayOrder.setMerchOrderNo(merchOrderNo);
    appPayOrder.setGid(gid);
    appPayOrder.setPartnerId(partnerId);    
    
    //调用接口
    AppPayResult result = netBankPayFacade.appPay(appPayOrder);
    logger.info("resultCode:{},resultMessage:{}", result.getCode(), result.getDetail());    
    
    //结果验证
    TestUtil.assertEqualsProcessing(result);
    
    //数据验证
    MusNetbankPaymentTxn musNetbankPaymentTxn = fintechBase.getMusNetbankPaymentTxnDao().selectMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);
    TestUtil.assertEquals("BP", musNetbankPaymentTxn.getStatus());
    TestUtil.assertEquals(gid, musNetbankPaymentTxn.getGid());    
    //清除数据
    fintechBase.getMusNetbankPaymentTxnDao().deleteMusNetbankPaymentTxnByPaySerialNo(bizOrderNo);
	}
	
	public void netBankPayFacadeAppPayTestFailOne() {
    ....
	}
	
	public void netBankPayFacadeAppPayTestFailTwo() {
    ....
	}
}
```

**如果被测对象为API接口请使用以下代码模板编写测试用例**

- 方法中的参数a、a1...an，表示的不同参数间的分隔，在csv文件中需要占一列，进行参数说明使用。
- 在API的测试模板中加入try{}catch{}finally{}是为了生成测试报告，在执行测试用例过程中会生成excel和将数据插入数据库中，为以后生成测试报告做准备。

```
@Slf4j
public class AccountQueryServiceTest extends TestBase {
	@Resource
	FintechBase fintechBase;
	
	@Test
	@CsvParameter(value = "openapi/accountQueryServiceTestSuccess.csv")
	@TestCaseName("testId={0},description={1}")
	public void accountQueryServiceTestSuccess(	int testId, String description, String resultCode,
												//基本参数
												String returnUrl, String notifyUrl, String signType,
												String version, String context, String service,
												String partnerId, String merchOrderNo,
												String requestNo, String securityCheckKey,
												//业务参数
												String a, String userNo,
												//会员账务表数据
												String a1, String accountNo, Long userId,
												String accountType, String username, Long balance,
												Long freeze, String comments, String status,
												//会员参数
												String a2, Long parentid, String parentUserNo,
												String salt, String email, String certNo,
												String userType, Integer grade, String insertUserNo,
												String password, String insertRealName,
												String mobileNo,
												//会员扩展参数
												String a3, String province, String city,
												String district, String isRegion, String regionNo,
												String isSetPsw, Long equityValue,
												String companyName) {
		//定义请求参数map,请求响应体，测试报告实体
		Map<String, String> map = new HashMap<String, String>();
		JSONObject responseEntity = new JSONObject();
		TestCaseUtil testCase = new TestCaseUtil();
		try {
			//清除数据
			fintechBase.getApiOrderInfoDao().deleteApiOrderInfoByRequestNo(requestNo);
            fintechBase.getBMemberDao().deleteBMemberByUserName(username);
            fintechBase.getBMemberDao().deleteBMemberByUserNo(userNo);
            fintechBase.getBMemberExtendDao().deleteBMemberExtendByUserName(username);
            fintechBase.getBMemberExtendDao().deleteBMemberExtendByUserNo(userNo);
            fintechBase.getAcAccountDao().deleteAcAccountByUserNo(userNo);

			//准备数据
			BMember bMember = fintechBase.getBMemberEntityService().inserBMember(status, parentid,
				parentUserNo, insertUserNo, username, password, salt, mobileNo, email,
				insertRealName, certNo, userType, grade, comments);
			fintechBase.getBMemberExtendEntityService().insertBMemberExtend(userNo, username,
				province, city, district, isRegion, regionNo, comments, isSetPsw, equityValue,
				bMember.getId(), companyName);
			fintechBase.getAcAccountEntityService().insertAcAccount(accountNo, userId, userNo,
				accountType, username, balance, freeze, comments, status);

			//测试过程
			TestUtil.buildMap(notifyUrl, returnUrl, requestNo, service, partnerId, signType,
				version, context, merchOrderNo, map);
			map.put("userNo", userNo);
			
			//调用接口
			responseEntity = httpRequest(url, map, securityCheckKey, partnerId);
			
			//结果验证
			TestUtil.assertEquals(resultCode, responseEntity.get("code"));
			TestUtil.assertEquals("成功", responseEntity.get("message"));
			TestUtil.assertEquals(cent(balance).subtract(cent(freeze)).getAmount().toString(), responseEntity.get("available"));
			TestUtil.assertEquals(cent(balance).getAmount().toString(), responseEntity.get("balance"));
			TestUtil.assertEquals(cent(freeze).getAmount().toString(), responseEntity.get("freeze"));
			TestUtil.assertEqualsCommon(map, responseEntity);
			
			//数据验证
			
			//清除数据
			fintechBase.getApiOrderInfoDao().deleteApiOrderInfoByRequestNo(requestNo);
			//验证通过，给用例是否通过参数赋值
			testCase.setIsAssert(1);
		} catch (Exception e) {
			e.printStackTrace();
			logger.info(testId + "用例未通过");
			TestUtil.assertTrue(false);
		} finally {
			try {
				//将用例说明，用例结果写入测试报告
				FileUtil.toFile(testCase, this.getClass(), TestCaseMethod.SUCCESS.getCode(),
					map.toString(), responseEntity.toJSONString(), testId, description);
			} catch (Exception e) {
				e.printStackTrace();
				logger.info("123出错了。。。。");
			}
		}
	}
```

编写好测试代码后运行测试用例后将会生成CSV文件，然后打开对应的CSV文件进行参数的填写
![c18b3587.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/c18b3587.png)
**特别提醒：在自动生成CSV文件时一定要记得在IDEA中配置编译参数，不然生成的CSV文件中的参数名无法正常显示**
![9db64b91.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/9db64b91.png)
编写好后我们就可以开始执行我们的测试用例了

#### 几个小知识点
##### 1.Lombok插件
添加此插件后可直接使用@Slf4j、@Data等方法来减少代码的编写，可自行百度lombok更多的使用。
![1905afb9.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/1905afb9.png)
##### 2.代码模板设置
在我们的测试代码中都是固定的流程，所以我们可以设置一个代码模板，使用快捷键快速将代码生成。
- 先在设置里添加一个自己的快捷键分组
![a091eeb6.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/a091eeb6.png)
- 再在自己建立的分组下新建快捷键
![21d77dba.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/21d77dba.png)
- 写入自己所需要的代码模板
![9cd7cffb.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/9cd7cffb.png)
- 选择模板分类为java
![b10a32c4.png](../images/f5f9f1c0-194a-48e2-914e-cb029644ee8d/b10a32c4.png)
这样在我们写代码时输入我们的模板名称就可以生成我们的代码模板了，是不是很爽呀！


## 三、注意事项

1. List、Map类型的参数不能通过CSV文件方法直接传递，只能在代码里进行组装
2. CSV文件里的列一定要与测试代码的方法中第一个参数对应，不然可能会报错哟