---
layout: post
title: TabLayout与ViewPager实现Tab
category: Android
tags: Tab
keywords:
description:
---

在app设计中使用tab切换页面是经常用到的功能，现在使用TabLayout和ViewPager来实现tab页面的切换功能。

主页面的布局文件 activity_main_tab.xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context="com.aurora.hxnt.ui.MainTabActivity">

        <android.support.v4.view.ViewPager
            android:id="@+id/view_pager"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />

        <android.support.design.widget.TabLayout
            android:id="@+id/tab_layout"
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:background="@color/white"
            app:tabGravity="fill"
            app:tabIndicatorHeight="0dp"
            app:tabMode="fixed"
            app:tabTextColor="#000" />

    </LinearLayout>

这个布局文件很简单，就两个控件，一个ViewPager和一个TabLayout。这个TabLayout在ViewPager下方，也就是在页面底部。

TabLayout用到的几个属性：
  - tabGravity：tab的布局方式，有两个可选值fill：填充满，center：居中
  - tabIndicatorHeight：tab选中指示器，必须是一个尺寸的数值。默认是2dp，这里设置为0dp，不显示tab选中指示器, 使用tab图片显示选中状态。
  - tabMode：tab在layout中的行为模式，有两可选值fixed：固定， scrollable：可滚动
  - tabTextColor：tab的文本颜色

MainTabActivity.java的代码：

    public class MainTabActivity extends BaseActivity implements IndexBaseFragment.OnFragmentInteractionListener {
        private ViewPager mViewPager;
        private TabLayout mTabLayout;

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            setContentView(R.layout.activity_main_tab);

            mViewPager = (ViewPager) findViewById(R.id.view_pager);
            mTabLayout = (TabLayout) findViewById(R.id.tab_layout);

            setData();
        }

        private void setData() {
            MyFragmentPagerAdapter pagerAdapter = new MyFragmentPagerAdapter(getSupportFragmentManager(), this);
            mViewPager.setAdapter(pagerAdapter);

            mTabLayout.setupWithViewPager(mViewPager);

            for (int i = 0; i < mTabLayout.getTabCount(); i++) {
                TabLayout.Tab tab = mTabLayout.getTabAt(i);
                if (tab != null) {
                    tab.setCustomView(pagerAdapter.getTabView(i));
                }
            }

            updateTabSelection(0);
        }

        private void updateTabSelection(int position) {
            mTabLayout.getTabAt(mTabLayout.getSelectedTabPosition()).getCustomView().setSelected(false);
            mTabLayout.getTabAt(position).getCustomView().setSelected(true);
            mTabLayout.setScrollPosition(position, 0, true);
        }

        @Override
        public void onFragmentInteraction(Uri uri) {
          //fragment与activity交互的回调函数
        }
      }

这里主要的是setData()方法，前面两行是给viewPager设置adapter。接下来是关联TabLayout和ViewPager。看setupWithViewPager的源码：

    public void setupWithViewPager(@Nullable final ViewPager viewPager) {
            if (mViewPager != null && mPageChangeListener != null) {
                // If we've already been setup with a ViewPager, remove us from it
                mViewPager.removeOnPageChangeListener(mPageChangeListener);
            }

            if (viewPager != null) {
                final PagerAdapter adapter = viewPager.getAdapter();
                if (adapter == null) {
                    throw new IllegalArgumentException("ViewPager does not have a PagerAdapter set");
                }

                mViewPager = viewPager;

                // Add our custom OnPageChangeListener to the ViewPager
                if (mPageChangeListener == null) {
                    mPageChangeListener = new TabLayoutOnPageChangeListener(this);
                }
                mPageChangeListener.reset();
                viewPager.addOnPageChangeListener(mPageChangeListener);

                // Now we'll add a tab selected listener to set ViewPager's current item
                setOnTabSelectedListener(new ViewPagerOnTabSelectedListener(viewPager));

                // Now we'll populate ourselves from the pager adapter
                setPagerAdapter(adapter, true);
            } else {
                // We've been given a null ViewPager so we need to clear out the internal state,
                // listeners and observers
                mViewPager = null;
                setOnTabSelectedListener(null);
                setPagerAdapter(null, true);
            }
        }

TabLayout内部包含了一个ViewPager对象mViewPager，做了一系列判断后把传进来的viewPager赋给了TabLayout内部的mViewPager。接下来主要看setPagerAdapter(adapter, true)这个函数:

        private void setPagerAdapter(@Nullable final PagerAdapter adapter, final boolean addObserver) {
            if (mPagerAdapter != null && mPagerAdapterObserver != null) {
                // If we already have a PagerAdapter, unregister our observer
                mPagerAdapter.unregisterDataSetObserver(mPagerAdapterObserver);
            }

            mPagerAdapter = adapter;

            if (addObserver && adapter != null) {
                // Register our observer on the new adapter
                if (mPagerAdapterObserver == null) {
                    mPagerAdapterObserver = new PagerAdapterObserver();
                }
                adapter.registerDataSetObserver(mPagerAdapterObserver);
            }

            // Finally make sure we reflect the new adapter
            populateFromPagerAdapter();
        }

这里也主要是populateFromPagerAdapter()这个函数:

    private void populateFromPagerAdapter() {
            removeAllTabs();

            if (mPagerAdapter != null) {
                final int adapterCount = mPagerAdapter.getCount();
                for (int i = 0; i < adapterCount; i++) {
                    addTab(newTab().setText(mPagerAdapter.getPageTitle(i)), false);
                }

                // Make sure we reflect the currently set ViewPager item
                if (mViewPager != null && adapterCount > 0) {
                    final int curItem = mViewPager.getCurrentItem();
                    if (curItem != getSelectedTabPosition() && curItem < getTabCount()) {
                        selectTab(getTabAt(curItem));
                    }
                }
            } else {
                removeAllTabs();
            }
        }

这里根据mPagerAdapter的getCount()返回的值循环迭代添加Tab，并调用mPagerAdapter的getPageTitle(i)函数返回的值给tab设置名称。然后是判断ViewPager当前选中的页面，并给tab设置选中状态。

分析完setupWithViewPager的代码，再看MainTabActivity.java中setData()接下来的代码：

    for (int i = 0; i < mTabLayout.getTabCount(); i++) {
        TabLayout.Tab tab = mTabLayout.getTabAt(i);
        if (tab != null) {
            tab.setCustomView(pagerAdapter.getTabView(i));
        }
    }

    updateTabSelection(0);

循环迭代TabLayout的tab并给tab设置自定义的view。最后一句updateTabSelection(0)是给tab重新设置选中状态，因为在setupWithViewPager已经给tab设置过选中状态了，而在这之后又重新设置的自定义的tab视图，所以要重新设置选中的tab的选中状态。

最后是MyFragmentPagerAdapter.java代码：

      private class MyFragmentPagerAdapter extends FragmentPagerAdapter {
      final int PAGE_COUNT = 4;
      private String tabTitles[] = new String[]{"Tab1","Tab2","Tab3", "Tab4"};
      private Context context;

      public MyFragmentPagerAdapter(FragmentManager fm, Context context) {
          super(fm);
          this.context = context;
      }

      @Override
      public Fragment getItem(int position) {
        return SampleFragment.newInstance(position);
      }

      @Override
      public int getCount() {
          return PAGE_COUNT;
      }

      @Override
      public CharSequence getPageTitle(int position) {
      //            return tabTitles[position];
          return null;
      }

      public View getTabView(int position) {
          View v = LayoutInflater.from(context).inflate(R.layout.tab_custom_layout, null);
          TextView tv = (TextView) v.findViewById(R.id.tv_tab_title);
          tv.setText(tabTitles[position]);
          ImageView img = (ImageView) v.findViewById(R.id.iv_tab_icon);
          if (position == 0) {
              img.setImageResource(R.drawable.tab_msg_icon);
          } else if (position == 1) {
              img.setImageResource(R.drawable.tab_moments_icon);
          } else if (position == 2) {
              img.setImageResource(R.drawable.tab_service_icon);
          } else {
              img.setImageResource(R.drawable.tab_my_icon);
          }
          return v;
      }

其中
- getPageTitle(position) 就是setupWithViewPager中添加tab时设置tab名称的方法。
- getTabView(position) 就是我们自定义tab视图是调用的方法。

至于SampleFragment，就没什么可说的就是一个Fragment的实现。
