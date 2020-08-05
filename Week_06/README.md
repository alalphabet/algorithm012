学习笔记
1.递归代码模板
//Python
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
    
    
// Java
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

// C/C++
void recursion(int level, int param) { 
  // recursion terminator
  if (level > MAX_LEVEL) { 
    // process result 
    return ; 
  }
  // process current logic 
  process(level, param);
  // drill down 
  recursion(level + 1, param);
  // reverse the current level status if needed
}

2.分治代码模板
//Python
def divide_conquer(problem, param1, param2, ...): 
        //recursion terminator 
      if problem is None: 
        print_result 
        return 
      //prepare data 
      data = prepare_data(problem) 
      subproblems = split_problem(problem, data) 
      //conquer subproblems 
      subresult1 = self.divide_conquer(subproblems[0], p1, ...) 
      subresult2 = self.divide_conquer(subproblems[1], p1, ...) 
      subresult3 = self.divide_conquer(subproblems[2], p1, ...) 
      …
      //process and generate the final result 
      result = process_result(subresult1, subresult2, subresult3, …)
      //revert the current level states

C/C++
int divide_conquer(Problem *problem, int params) {
      // recursion terminator
      if (problem == nullptr) {
        process_result
        return return_result;
      } 
      // process current problem
      subproblems = split_problem(problem, data)
      subresult1 = divide_conquer(subproblem[0], p1)
      subresult2 = divide_conquer(subproblem[1], p1)
      subresult3 = divide_conquer(subproblem[2], p1)
      ...
      // merge
      result = process_result(subresult1, subresult2, subresult3)
      // revert the current level status
      return 0;
}

Java
private static int divide_conquer(Problem problem, ) {
      if (problem == NULL) {
        int res = process_last_result();
        return res;     
      }
      subProblems = split_problem(problem)
      res0 = divide_conquer(subProblems[0])
      res1 = divide_conquer(subProblems[1])
      result = process_result(res0, res1);
      return result;
}

3.动态规划关键点
(1)最优子结构 opt[n] = best_of(opt[n-1], opt[n-2], ...)
(2)储存中间状态：opt[i]
(3)递推公式（状态转移方程或者DP方程）
    Fib: opt[i]=opt[n-1]+opt[n-2]
    二维路径：opt[i,j]=opt[i+1][j]+opt[i][j+1] （判断a[i.j]是否空地）
