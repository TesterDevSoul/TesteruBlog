@Transactional(rollbackFor = Exception.class): 指定事务回滚的异常类型，如果发生该类型的异常，事务将回滚。




@Transactional(noRollbackFor = RuntimeException.class): 指定事务不回滚的异常类型，即使发生该类型的异常，事务也不会回滚。

注意：@Transactional只对public方法有效，对于同一个类中不同方法之间的调用，事务不会生效。如果需要在同一个类中不同方法之间开启事务，可以将该方法单独封装到一个类中，通过在该类上标注@Transactional注解实现事务。