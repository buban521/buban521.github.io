### jdbc连接Oracle示例
- 1.在Eclipse里新建Java项目,右键项目名,新建lib文件夹

- 2.在Oracle安装目录里找到oracle驱动jar包,C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib\ojdbc6_g.jar

- 3.右键jar包,build to path,把驱动包导入项目

- 4.编写测试类

```
package cn.lszz;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import org.junit.Test;

public class TestOracle {
	@Test
	public void testOracle() {
		Connection con = null;// 创建一个数据库连接
		PreparedStatement pre = null;// 创建预编译语句对象
		ResultSet result = null;// 创建一个结果集对象
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");// 加载Oracle驱动程序
			System.out.println("开始尝试连接数据库!");
			// 127.0.0.1是本机地址，XE是精简版Oracle的默认数据库名
			String url = "jdbc:oracle:thin:@127.0.0.1:1521:XE";
			String user = "scott";// Oracle用户名
			String password = "tiger";// Oracle密码
			con = DriverManager.getConnection(url, user, password);// 获得连接
			System.out.println("连接成功!");
			// sql查询语句,?号代表需要传入的参数
			String sql = "select * from emp where empno = ?";
			pre = con.prepareStatement(sql);// 实例化编译语句
			pre.setInt(1, 1004);// 设置参数,第一个参数,参数是1004
			result = pre.executeQuery();// 执行查询
			while (result.next()) {
				// 当结果集不为空时
				System.out.println("员工编号:" + result.getInt(1) + " ENAME:" + result.getString(2));
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try { // 逐一将上面的几个对象关闭,因为不关闭的话会影响性能,并且占用资源
					// 注意关闭的顺序,最后使用的最先关闭
				if (result != null) {
					result.close();
				}
				if (pre != null) {
					pre.close();
				}
				if (con != null) {
					con.close();
				}
				System.out.println("数据库连接已经关闭!");
			} catch (Exception e2) {
				e2.printStackTrace();
			}
		}
	}
}
```