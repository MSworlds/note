## Zookeeper

### 1.命令

  	创建节点： create   *node*  *value*

​         获取节点：   get. *node*

### 2.hellow. world

```java
package com.atguigu.test;

import java.io.IOException;

import org.apache.log4j.Logger;
import org.apache.zookeeper.CreateMode;
import org.apache.zookeeper.KeeperException;
import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.ZooDefs.Ids;
import org.apache.zookeeper.ZooKeeper;
import org.apache.zookeeper.data.Stat;

/**
 * 
 * @Description:	eclipse此处为Client端，CentOS为ZooKeeper的Server端
 * 
 * 1	通过java程序，新建链接zk，类似jdbc的connection，open.session
 * 2	新建一个znode节点/atguigu并设置为hello1018	等同于create /atguigu hello1018
 * 3	获得当前节点/atguigu的最新值			get /atguigu
 * 4	关闭链接
 * 
 * @author zzyy
 * @date 2018年3月21日
 */
public class HelloZK
{
	/**
	* Logger for this class
	*/
	private static final Logger logger = Logger.getLogger(HelloZK.class);
	//实例常量
	private static final String CONNECTSTRING = "192.168.23.167:2181";
	private static final String PATH = "/atguigu";
	private static final int SESSION_TIMEOUT = 20 * 1000;
	//实例变量
	
	/**
	 * @throws IOException 
	* @Title: startZK
	* @Description: 获得ZK的连接实例
	* @param @return    参数
	* @return ZooKeeper    返回类型
	* @throws
	 */
	public ZooKeeper startZK() throws IOException
	{
		return new ZooKeeper(CONNECTSTRING, SESSION_TIMEOUT, new Watcher() {
			@Override
			public void process(WatchedEvent event)
			{
			}
		});
	}
	
	/**
	 * 
	* @Title: createZnode
	* @Description: 创建一个节点并赋值
	* @param @param zk
	* @param @param path
	* @param @param data
	* @param @throws KeeperException
	* @param @throws InterruptedException    参数
	* @return void    返回类型
	* @throws
	 */
	public void createZnode(ZooKeeper zk,String path,String data) throws KeeperException, InterruptedException
	{
		zk.create(path, data.getBytes(), Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
	}
	
	/**
	 * 
	* @Title: getZnode
	* @Description: 获得节点的值
	* @param @param zk
	* @param @param path
	* @param @return
	* @param @throws KeeperException
	* @param @throws InterruptedException    参数
	* @return String    返回类型
	* @throws
	 */
	public String getZnode(ZooKeeper zk,String path) throws KeeperException, InterruptedException
	{
		String result = "";
		
		byte[] byteArray = zk.getData(path, false, new Stat());
		result = new String(byteArray);
		
		return result;
	}
	
	/**
	 * 
	* @Title: stopZK
	* @Description:关闭连接
	* @param @param zk
	* @param @throws InterruptedException    参数
	* @return void    返回类型
	* @throws
	 */
	public void stopZK(ZooKeeper zk) throws InterruptedException
	{
		if(null != zk) zk.close();
	}
	
	
	public static void main(String[] args) throws Exception
	{
		HelloZK hello = new HelloZK();
		ZooKeeper zk = hello.startZK();
		
		if(zk.exists(PATH, false) == null)
		{
			hello.createZnode(zk, PATH, "hello1018_java");
			
			String result = hello.getZnode(zk, PATH);
			
			if (logger.isInfoEnabled()) {
				logger.info("main(String[]) ***************** String result=" + result);
			}
		}else {
			logger.info("This node is exists......");
		}
		
		hello.stopZK(zk);
	}
}
```

### watch

```java
package com.atguigu.test;

import java.io.IOException;

import org.apache.log4j.Logger;
import org.apache.zookeeper.CreateMode;
import org.apache.zookeeper.KeeperException;
import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.ZooDefs.Ids;
import org.apache.zookeeper.ZooKeeper;
import org.apache.zookeeper.data.Stat;

import lombok.Getter;
import lombok.Setter;



/**
 * 
 * @Description:
 * 1	初始化ZK的多个操作
 * 		1.1	建立ZK的链接
 * 		1.2	创建/atguigu节点并赋值
 * 		1.3	获得该节点的值
 * 
 * 2	watch
 * 		2.1	获得值之后(getZnode方法被调用后)设置一个观察者watcher，如果/atguigu该节点的值发生了变化，(A-->B)
 * 			要求通知Client端eclipse，一次性通知
 * @author zzyy
 * @date 2018年3月21日
 */
public class WatchOne
{
	/**
	* Logger for this class
	*/
	private static final Logger logger = Logger.getLogger(HelloZK.class);
	//实例常量
	private static final String CONNECTSTRING = "192.168.23.167:2181";
	private static final String PATH = "/atguigu";
	private static final int SESSION_TIMEOUT = 20 * 1000;
	//实例变量
	private @Setter@Getter ZooKeeper zk;
	
	/**
	 * @throws IOException 
	* @Title: startZK
	* @Description: 获得ZK的连接实例
	* @param @return    参数
	* @return ZooKeeper    返回类型
	* @throws
	 */
	public ZooKeeper startZK() throws IOException
	{
		return new ZooKeeper(CONNECTSTRING, SESSION_TIMEOUT, new Watcher() {
			@Override
			public void process(WatchedEvent event)
			{
			}
		});
	}
	
	/**
	 * 
	* @Title: createZnode
	* @Description: 创建一个节点并赋值
	* @param @param zk
	* @param @param path
	* @param @param data
	* @param @throws KeeperException
	* @param @throws InterruptedException    参数
	* @return void    返回类型
	* @throws
	 */
	public void createZnode(String path,String data) throws KeeperException, InterruptedException
	{
		zk.create(path, data.getBytes(), Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
	}
	
	/**
	 * 
	* @Title: getZnode
	* @Description: 获得节点的值
	* @param @param zk
	* @param @param path
	* @param @return
	* @param @throws KeeperException
	* @param @throws InterruptedException    参数
	* @return String    返回类型
	* @throws
	 */
	public String getZnode(String path) throws KeeperException, InterruptedException
	{
		String result = "";
		
		byte[] byteArray = zk.getData(path, new Watcher() {
			@Override
			public void process(WatchedEvent event)
			{
				//业务逻辑，也即出发了/atguig节点的变更后，我需要立刻获得最新值
				//将本部分的业务逻辑提出来，新变成一个方法
				try 
				{
					triggerValue(path);
				} catch (KeeperException | InterruptedException e) {
					e.printStackTrace();
				}
			}
		}, new Stat());
		result = new String(byteArray);
		
		return result;
	}	
	
	
	public void triggerValue(String path) throws KeeperException, InterruptedException
	{
		String result = "";
		
		byte[] byteArray = zk.getData(path, null, new Stat());
		result = new String(byteArray);
		
		logger.info("*********watcher after triggerValue result : "+result);
			
	}

	public static void main(String[] args) throws Exception
	{
		WatchOne one = new WatchOne();
		one.setZk(one.startZK());

		if(one.getZk().exists(PATH, false) == null)
		{
			one.createZnode(PATH, "AAA");
			
			String result = one.getZnode(PATH);
			
			logger.info("*********main init result : "+result);
		}else {
			logger.info("This node is exists......");
		}
		
		Thread.sleep(Long.MAX_VALUE);
		
	}
}

```

