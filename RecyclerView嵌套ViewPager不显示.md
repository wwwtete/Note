  首先需求是这样的:显示一个列表，列表的每个Item中又有一个列表可以横向滑动，所以采用了RecyclerView嵌套ViewPager来实现，结果发现ViewPager不显示，尝试采用给ViewPager写死一个宽高值不能解决，又自定义ViewPager重写onMeasure方法也不行。。。
  ### 最终解决方案