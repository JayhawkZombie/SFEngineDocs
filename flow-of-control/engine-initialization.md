## Engine Initialization  

Alright, so what happens inside this method here?  It's not static, so it's acting on the singleton instance itself.  

Well, it's not all that exciting.  This method will get the RenderWindow initialized and created, as well as some vital intenal configurations, then it will call `Startup` on itself and return whatever that returns.