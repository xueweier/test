---
layout: post
title: Laravel 中 Lang get 方法的简单实现
category: tech
tags: linux
---

![](/assets/img/laravel.jpg)

Laravel 中获取语言的方法 Lang::get() 非常好用，我经常将一些错误提示放到这些文件里。比如：

    <?php

    return [
        /*
        |--------------------------------------------------------------------------
        | Authentication Language Lines
        |--------------------------------------------------------------------------
        |
        | The following language lines are used during authentication for various
        | messages that we need to display to the user. You are free to modify
        | these language lines according to your application's requirements.
        |
        */

        'failed' => '验证失败。',
        'throttle' => '登录验证过于频繁，请 :seconds 秒后再试。',
        'link_not_exist_or_expired' => '该链接不存在，或超时失效',

        'validation_code_receiver' => '验证码无法发送。',
        'identity_validation_throttle' => '验证码发送过于频繁，请 :seconds 秒后再次发送。',
        'identity_validation_fail' => '验证码未通过验证。',
        'identity_validation_expired' => '验证码已过期。',
        'identity_already_exists' => ':identity 已存在。',
        'identity_must_be_cellphone_or_email' => ':identity 不是手机号或邮箱。',

        'cellphone_or_email_required' => '需要有手机号或邮件地址其中之一。',
        'credential_name_required' => '需要有手机号或邮件地址或用户名其中之一。',
        'cannot_confirm_email' => '无法确认邮箱。',
        'confirmation_expired' => '已超时，无法确认邮箱。',
        'original_password_is_not_the_same' => '密码不一致，请重新输入！',
        'two_passwords_are_not_the_same' => '两次输入的密码不一致！',
    ];
    
获取的方法则如下：
    
    ['100', Lang::get('auth.throttle', ['seconds' => $this->lockoutTime()])];
    
后面的数组将替换 :seconds 处。
    
期间自己某个功能也需要模板，但又不好使用Lang这个方法，遂自己实现了一个类似的方法。很简单的：

    if (!function_exists('lang_get')) {
        function lang_get($str, array $params = [])
        {
            foreach ($params as $key => $value) {
                $str = str_replace(':' . $key, $value, $str);
            }
            return $str;
        }
    }