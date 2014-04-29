##调试输出框架
###用途 
测试输出用，代替trace简单模式
```actionscript
        debug("hello world");
        debug(1+2);
        debug(a,b,c,d);
        ...
```
###用法
```actionscript
...
    private function initDebug():void 
    	{
            //调试输出管理类
			var dm:DebugerMan = new DebugerMan();
            
			dm.enabled = true;//输出（默认不输出）
			dm.addDebuger(new LocalDebuger());//输出到localConnection
			dm.addDebuger(new TraceDebuger());//输出到控制台
            dm.addDebuger(new JSAlertDebuger());//输出到js弹窗
            dm.addDebuger(new TraceMaxDebuger());//输出到控制台 带堆栈
            
			debug =dm.debug;
		}
...
debug("hello","world",1,a+b);

```
###扩展
如果需要日志或其他输出功能，请自行约定扩展。继承接口IDebuger
```actionscript
package winx.debugers 
{
    import flash.events.AsyncErrorEvent;
	import flash.events.Event;
	import flash.events.SecurityErrorEvent;
	import flash.events.StatusEvent;
	import flash.net.LocalConnection;
	import winx.debugers.core.IDebuger;
	
	/**
	 * ...
	 * @author nba00123
	 */
	public class LogDebuger implements IDebuger 
	{
        private var log:String;
        /**
        log 输出过滤，debug,log,error,infor...
        */
		public function LocalDebuger(log:String="debug") 
		{
			this.log=log;
		}
		
		/* INTERFACE winx.debugers.core.IDebuger */
		
		public function debug(...arg):void 
		{
			if(arg[0]==log){ 
                //do log
                trace(arg);
            }
		}
		
	}

}
```

```actionscript
...
dm.addDebuger(new LogDebuger("log"));
dm.addDebuger(new LogDebuger("debug"));
dm.addDebuger(new LogDebuger("error"));
dm.addDebuger(new LogDebuger("info"));
...
debug("log","这是日志");
debug("debug","这是调试");
debug("info","这是信息");
```
