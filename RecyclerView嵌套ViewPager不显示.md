  首先需求是这样的:显示一个列表，列表的每个Item中又有一个列表可以横向滑动，所以采用了RecyclerView嵌套ViewPager来实现，结果发现ViewPager不显示，尝试采用给ViewPager写死一个宽高值不能解决，又自定义ViewPager重写onMeasure方法也不行。。。
  ### 最终解决方案
  >  在RecyclerView的Adapter中重新给VIewPager设定一个**唯一**的ID
  
  #### 问题根本原因：
  > 原来ViewPager在同一个页面中不允许ID共享，也就是说ViewPager不允许存在相同的id，否则就会出现异常！
  