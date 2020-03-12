## 接口和抽象类

### 抽象类

> 抽象类是包含抽象方法的类

* 抽象类使用`abstarct`关键字修饰
* 抽象类单继承
* 抽象类方法必须是`public`、`protected`修饰（需要子类继承)
* 继承抽象类的子类必须实现所有抽象类未实现抽象方法，否则该类必须用`abstarct`修饰

### 接口

* 接口中变量默认都是 `public static final` 修饰
* 接口中所有方法默认都是 `public abstarct`修饰

### 抽象类和接口的区别
* 语法层面的区别
    * 抽象类可提供方法实现，而接口只能是`public abstarct`修饰
    * 抽象中成员变量可以是各种类型，而接口只能是`public static fianl`修饰
    * 抽象类可以包含静态代码快及静态方法，接口中不能包含静态代码块及静态方法。
    * 抽象类是单继承，接口是多实现
    * 抽象类可以右构造方法，接口不能右构造器
    * 抽象类可以右`main`方法，接口不能
* 设计层面的区别
    * 接口是对行为的抽象 抽象类是对类的抽象(接口：有没有的关系， 抽象类：是不是的关系)
    * 抽象类是模板设计，接口是一种行为规范，是一种辐射式设计




// 发送消息
		String recevier = resourceReportMapper.selectByPrimaryKey(applyAuditDto.getResourceReportId()).getUserId();
		// 获取店经理
		Map<String, String> manager = resourceReportMapper.selectShopManByUserId(recevier);
		String managerId = recevier;
		if (manager != null) {
			managerId = manager.get("id");
		}
    // 给店经理发送消息
			BwMessageDto message = new BwMessageDto("申请提报资源驳回", "您的申请提报资源被驳回", managerId, UserUtils.getUserId(),
					1, 0, UserUtils.getUserId(), null, null, null);
			messageOpFacade.sendMessage(message);