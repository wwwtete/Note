关于如何自定义TbaLayout可以去网上搜，有很多文章介绍，这里就不叙述了，一般自定义Tab需要调用tab的setCustomView方法，
但是发现设置无效,后来查看源码才知道，原来在调用tablayout.setupWithViewPager方法后会删掉我们之前自定义的tab，所以解决办法就是：
> 在调用tablayout.setupWithViewPager方法之后必须重新调用一下各个tab的setCustomView方法

~~~ java
//1.先设置Adapter
mVpViewPager.setAdapter(mPagerAdapter);
        mTab.setupWithViewPager(mVpViewPager);
        for (int i = 0; i < mTlTab.getTabCount(); i++) {
            mTlTab.getTabAt(i).setCustomView(mPagerAdapter.getTabView(i));
        }
~~~