学习笔记
递归代码模板

Python代码模板
def recursion(level, param1, param2, ...): 
    # recursion terminator 
    if level > MAX_LEVEL: 
       process_result 
       return 
    # process logic in current level 
    process(level, data...) 
    # drill down 
    self.recursion(level + 1, p1, ...) 
    # reverse the current level status if needed
    
Java 代码模板
public void recur(int level, int param) { 
      // terminator 
      if (level > MAX_LEVEL) { 
        // process result 
        return; 
      }
      // process current logic 
      process(level, param); 
      // drill down 
      recur( level: level + 1, newParam); 
      // restore current status 
}

