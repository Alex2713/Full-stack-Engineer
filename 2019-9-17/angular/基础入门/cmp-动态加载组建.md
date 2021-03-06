## 动态加载组件

动态加载简单步骤

1. 引入视图管理模块
2. 创建需加载的组件,获取组件实例
3. 给组建输入输出属性赋值

### 具体流程(第一版)

#### 导入需要的模块

	import { ComponentFactory, ComponentRef, ViewChild, ViewContainerRef, ComponentFactoryResolver } from '@angular/core';

#### 初始化变量

	componentRef: ComponentRef<AppAlertComponent>;
	@ViewChild("alertContainer", { read: ViewContainerRef }) 
	container: ViewContainerRef;

注意:"alertContainer"为模版中一个模块,此处必须在html模版中声明,否则会报错:

	<section #alertContainer></section>

#### 注入依赖

	private resolver: ComponentFactoryResolver

#### 显示组件方法

	const factory: ComponentFactory<AppAlertComponent> =
      this.resolver.resolveComponentFactory(AppAlertComponent);
    this.componentRef = this.container.createComponent(factory);
    this.componentRef.instance.isModalShow = true;
    this.componentRef.instance.closedd.subscribe((isShow:boolean) =>{
      //this.componentRef.instance.isModalShow = isShow;
    });
注意:重新创建组件的时候如果不需要多层共存,需要先清空容器内容,如下:

	this.container.clear();

#### 销毁组件

	ngOnDestroy() {
	  this.componentRef.destroy()
	 }


###具体流程(第二版)

流程和第一版无差,只是简略了些,把创建组件的返回类型(componentRef)删除,直接返回一个let通用类型,声明组件类型的时候省却ComponentFactory<AppAlertComponent>,直接获取组件赋值给factory变量作为createComponent的参数.

	const factory = this.resolver.resolveComponentFactory(AppAlertComponent);
    const com = this.container.createComponent(factory);


###具体流程(第三版)

切记在组件声明文件中添加，否则会报错
	entryComponents: [CoreDescComponent],

###删除操作

需要在子组件的onDestroy方法中删除js相关对象和变量；