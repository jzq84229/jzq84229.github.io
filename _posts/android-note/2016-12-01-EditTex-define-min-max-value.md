---
layout: post
title: Android EditText设置最大最小值
category: Android
tags: EditText
keywords:
description:
---

定义一个类实现InputFilter
```
package com.test;

import android.text.InputFilter;
import android.text.Spanned;

public class InputFilterMinMax implements InputFilter {

    private int min, max;

    public InputFilterMinMax(int min, int max) {
        this.min = min;
        this.max = max;
    }

    public InputFilterMinMax(String min, String max) {
        this.min = Integer.parseInt(min);
        this.max = Integer.parseInt(max);
    }

    @Override
    public CharSequence filter(CharSequence source, int start, int end, Spanned dest, int dstart, int dend) {   
        try {
            int input = Integer.parseInt(dest.toString() + source.toString());
            if (isInRange(min, max, input))
                return null;
        } catch (NumberFormatException nfe) { }     
        return "";
    }

    private boolean isInRange(int a, int b, int c) {
        return b > a ? c >= a && c <= b : c >= b && c <= a;
    }
}
```
在activity中设置EditText的filter：
```
EditText et = (EditText) findViewById(R.id.myEditText);
et.setFilters(new InputFilter[]{ new InputFilterMinMax("1", "12")});
```
最后别忘记在布局文件中设置EditText的属性`android:inputType="number"`
这样EditText的值只能输入1-12了

[http://stackoverflow.com/questions/14212518/is-there-a-way-to-define-a-min-and-max-value-for-edittext-in-android](http://stackoverflow.com/questions/14212518/is-there-a-way-to-define-a-min-and-max-value-for-edittext-in-android)
