---
layout: post
title: ListViewǶ��ViewPager
category: Android
tags: Android
keywords: viewpager
description:
---


��ListView��Item��Ƕ��ViewPager���ڻ�����Ļʱ�������ͻ��
��дViewPager��Touch�ַ��¼������ݻ���λ���ж�touch���������Լ������Ǵ��ݸ����ؼ���

ע�����´���ת��[����][1]
    
    /** 
     * @Description: Ƕ��ListView��ViewPager
     * ��дdispatchTouchEvent�ж�ViewPager��������ListView����
     */
    public class MyViewPager extends ViewPager {
    	
    //  ʹ�ô˷�����������ViewPager��touch�¼���ʹ���ؼ�ListView�޷�����
    //    @Override  
    //    public boolean dispatchTouchEvent(MotionEvent ev) {  
    //        getParent().requestDisallowInterceptTouchEvent(true);//��仰������ ���߸�view���ҵĵ����¼������д�����Ҫ�谭�ҡ�    
    //	    return super.dispatchTouchEvent(ev);  
    //	}  
    
    	private float xDown;// ��¼��ָ����ʱ�ĺ����ꡣ
    	private float xMove;// ��¼��ָ�ƶ�ʱ�ĺ����ꡣ
    	private float yDown;// ��¼��ָ����ʱ�������ꡣ
    	private float yMove;// ��¼��ָ�ƶ�ʱ�������ꡣ
    	private boolean viewpagersroll = false;// ��ǰ�Ƿ���viewpager����
    
    	public MyViewPager(Context context) {
    		super(context);
    	}
    
    	public MyViewPager(Context context, AttributeSet attrs) {
    		super(context, attrs);
    	}
    
    	@Override
    	public boolean dispatchTouchEvent(MotionEvent ev) {
    		if (ev.getAction() == MotionEvent.ACTION_DOWN) {
    			// ��¼����ʱ��λ��
    			xDown = ev.getRawX();
    			yDown = ev.getRawY();
    		} else if (ev.getAction() == MotionEvent.ACTION_MOVE) {
    			xMove = ev.getRawX();
    			yMove = ev.getRawY();
    
    			if (viewpagersroll) {
    				// viewpager�Լ�������Ч��
    				getParent().requestDisallowInterceptTouchEvent(true);
    				return super.dispatchTouchEvent(ev);
    			}
    
    			// ����Ķ����ж���Viewpager����,ListView������
    			if (Math.abs(yMove - yDown) < 5 && Math.abs(xMove - xDown) > 20) {
    				viewpagersroll = true;
    			} else {
    				// �ɸ�����listview��������Ч��
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