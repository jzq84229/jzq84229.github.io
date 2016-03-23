---
layout: post
title: ListView嵌入ViewPager
category: Android
tags: Android
keywords: viewpager
description:
---


在ListView的Item中嵌入ViewPager，在滑动屏幕时会产生冲突。
重写ViewPager的Touch分发事件，根据滑动位置判断touch动作是由自己处理还是传递给父控件。

注：以下代码转自[孤云][1]
    
    /** 
     * @Description: 嵌入ListView的ViewPager
     * 重写dispatchTouchEvent判断ViewPager滑动还是ListView滑动
     */
    public class MyViewPager extends ViewPager {
    	
    //  使用此方会拦截所有ViewPager的touch事件，使父控件ListView无法滑动
    //    @Override  
    //    public boolean dispatchTouchEvent(MotionEvent ev) {  
    //        getParent().requestDisallowInterceptTouchEvent(true);//这句话的作用 告诉父view，我的单击事件我自行处理，不要阻碍我。    
    //	    return super.dispatchTouchEvent(ev);  
    //	}  
    
    	private float xDown;// 记录手指按下时的横坐标。
    	private float xMove;// 记录手指移动时的横坐标。
    	private float yDown;// 记录手指按下时的纵坐标。
    	private float yMove;// 记录手指移动时的纵坐标。
    	private boolean viewpagersroll = false;// 当前是否是viewpager滑动
    
    	public MyViewPager(Context context) {
    		super(context);
    	}
    
    	public MyViewPager(Context context, AttributeSet attrs) {
    		super(context, attrs);
    	}
    
    	@Override
    	public boolean dispatchTouchEvent(MotionEvent ev) {
    		if (ev.getAction() == MotionEvent.ACTION_DOWN) {
    			// 记录按下时的位置
    			xDown = ev.getRawX();
    			yDown = ev.getRawY();
    		} else if (ev.getAction() == MotionEvent.ACTION_MOVE) {
    			xMove = ev.getRawX();
    			yMove = ev.getRawY();
    
    			if (viewpagersroll) {
    				// viewpager自己处理滑动效果
    				getParent().requestDisallowInterceptTouchEvent(true);
    				return super.dispatchTouchEvent(ev);
    			}
    
    			// 这里的动作判断是Viewpager滑动,ListView不滑动
    			if (Math.abs(yMove - yDown) < 5 && Math.abs(xMove - xDown) > 20) {
    				viewpagersroll = true;
    			} else {
    				// 由父容器listview来处理滑动效果
    				return false;
    			}
    		} else if (ev.getAction() == MotionEvent.ACTION_UP) {
    			viewpagersroll = false;
    		}
    		return super.dispatchTouchEvent(ev);
    	}
    }

---


[1]:http://blog.csdn.net/u010142437/article/details/22307287