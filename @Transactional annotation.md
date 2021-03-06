06/28 @Transactional annotation:

Enabled by adding <tx:annotation-driven transaction-manager="xxxxTransactionManager" /> where xxxxTransactionManager is injected like:

This annotation can be applied on class level where every method is annotated by default, or, can be applied at method level which will override class-level annotation.

Spring AOP is proxy-based.

By default, CGLIB (a java framework) will be creating proxy subclass around the target class. So classes declared as final or private can't be proxied.

In proxy mode (which is the default), only external method calls coming in through the proxy are intercepted. If the target object to be proxied implements at least one interface then a JDK dynamic proxy will be used. All of the interfaces implemented by the target type will be proxied. If the target object does not implement any interfaces then a CGLIB proxy will be created. proxy-target-class="true" is used to force CGLIB proxy.

Make sure that all related beans are configured in ApplicationContext!!!!

Propagation.Required attribute is default, meaning the same transaction will be used for underlying transactions.

In place of @Transactional annotation, we can directly use jdbc way:
```java
public boolean testTest() {
		
		boolean respUpdate = false, respInsert = false;
		
		Connection conn = DataSourceUtils.getConnection(getJdbcTemplate().getDataSource());
		
		try {

			conn.setAutoCommit(false);
			PreparedStatement stmtUpdate = conn.prepareStatement(UPDATE_TEST);
			respUpdate = stmtUpdate.execute();
			
			PreparedStatement stmtInsert = conn.prepareStatement(INSERT_TEST);
			respInsert = stmtInsert.execute();
			
			conn.commit();
			
			conn.setAutoCommit(true);
			
		} catch (SQLException e) {
			try {
				conn.rollback();
			} catch (SQLException e1) {
				
			}
		}
		
		return respUpdate && respInsert;
	}
  ```
