
   一个多表单对象参数列表型表单提交插件

  传入一个对象

  对象列表里面是规则集合 属性不可设置重复,由于js对象属性限制.否则会后面会覆盖上面的设置的规则。 规则名同样不可重复，同理
  一个规则集合 有"dom" 属性对象 和 一系列 规则对象。必须制定一个名字"dom"的属性名字
  “dom” 下有两个属性   "valueDom" : id(a), 要检查内容的dom元素
  "errorMessageDom": id("span1"),设置错误的dom元素
  一系列 规则对象  分别为名称各不相同的规则对象
  有3中检测方式："rule" 制定规则方式 分别为 1.对象 2.字符串 3. fn

  "rule" 为字符串表示使用插件内部提供的检验规则,指定插件的规则名。 可以提供错误返回信息表示检测不通过的错误dom设置的信息。
  "rule":对象：表示你不满足次规则指定的检测规则，指定插件的一个规则名。可给“detection”属性传入一个正则表达式(只支持正则表达式)，设置自己设置想要的检测方式.此时会使用你的规则去检测.
  "rule"：为一个函数时,则可以写任何代码.目前只支持同步.(我有的时间玩玩异步)



  规则名:
  //isMobile 检测手机号码
  //mail   检测邮件
  //user   检测用户名
  //passWord  检测密码
  //minLength  检测最大可的长度设置值
  //maxLength  检测最小可的长度设置值
  //isNoequal  检测值是否相等

  var regObj=   {
  "a":{
  "dom":{
  "valueDom" : id(a),  表示要检测的dom元素
  "errorMessageDom": id("span1"),  表示要设置的检测规则不通过时设置错误信息的的dom元素
  },
  "rule1":{
  "rule":"isNoequal", //提供的规则方法 必须指定
  "returnMessage":"输入的值不能等于3"  //规则不通过时返时错误dom元素设置的值  可指定或不指定  不指定则使用内部提供的返回错误信息
  },
  "rule2":{
  "rule":{
  "rule":"minLength",  //提供的规则方法名 必须指定
  "detection":3,    //提供的规则方法 必须指定，表示自己要使用的正则表达式
  "returnMessage":"输入的值不能少于3"  //规则不通过时作物  可指定或不指定，不指定则使用内部提供的返回错误信息
  }
  },
  "rule3":{
  "rule":function (){ // 也可设置为一个回调，里面设置你的逻辑代码，返回true时，会打断检测,回调请自行设置错误dom的信息
  // code...
  retrun  true; 会打断表单的校检

  }
  "rule3":{
  rule":"maxLength",  //提供的规则方法名 必须指定
  "detection":10,    //提供的规则方法 必须指定，表示自己要使用的正则表达式
  "returnMessage":"输入的值不能少于3"  //规则不通过时作物  可指定或不指定，不指定则使用内部提供的返回错误信息

  }
  }
  "b":{},
  "c"{},
  "e":{}
  }


  

  提供3个方法
  var a= new Validator(regObj);// 实例化一个检测对象
  a.all() // 该方法调用会立即检测regObj里的规则集合里的规则集合,该检测方式是同步的,会检测所有的的表单元素,只要检测到表单的错误就会停止整个表单检测过程.
  a.one(array,type);// 接受2个参数;1. 为一个指定规则名的数组例如->["a","b"]是regObj的属性,字符串值为.2.参数指定一个事件名称. 作用: 给规则对象里的检测
  dom 绑定指定的事件,事件触发时，会动态检测该规则的指定要检测的dom元素.
  a.cm(array,type) // 接受2个参数;1. 为一个指定规则明的数组例如->["a","b"]是regObj的属性.2.参数指定一个事件名称. 作用: 给规则对象里的错误
  dom 绑定指定的事件,事件触发时，会动态清除该规则的指定的错误的dom元素内容.

  当同一实例化出Validator的第二次调用add方法时添加规则集合时,会刷新检测对象 Validator是深度刷新，包括之前规则集合dom元素绑定的事件也会解绑，所有规则检查的是第二次add方法添加的规则集合。
  清楚之前掉用的add one cm方法的效果,需从新设置方法。
  若是再一次添加一个检测对象，请在实例化一个Validator对象,这样一个页面可检测多个表单.
  */