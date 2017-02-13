---
layout: post
title: PHP 的 get_class 方法
category: tech
tags: php laravel
---



今天在写一个虚基类 AbstractUuidModel，因为要从 request 里批量将数据通过 Eloquent 保存至 model。而此时有很多 model 都需要这样一个操作，每个 model 都有20个以上的属性，一个一个赋值，真的是能把人写死。

为了提高效率，遂在虚基类中添加一个抽象度比较高的方法。

由 updateByConsole() 这个方法开始进入逻辑：

    public function updateByConsole()
    {
        $request = request();
        $flag = true;
        foreach ($request->all() as $key => $value) {
        	if(!$flag)continue;
            if(!$request->has($key))continue;
            if(is_string($value))$value = trim($value);
            $flag = $flag && $this->updateProperty($key,$value);
        }
        if($flag) $this->save();

        return $flag;
    }

将每一个 input 交给 updateProperty() 这个方法。于是接下来是核心代码：


    protected function updateProperty($key,$value){
        if(in_array($key,$this->keyExcept))return true;

        if($this->{$key} == $value) return true;

        $className = get_class($this);
        if(in_array($key,$this->keyUnion)){
            $count = $className::where($key,$value)->count();
            if($count>0)return false;
        }

        if(in_array($key,$this->keySpecific)){
            $this->specific($key,$value);
        }else{
            $this->{$key} = $value;
        }

        return true;
    }

可以看出核心逻辑是这样的：

* 是否有不需要存入的属性，有的话继续循环到一个属性
* 前后值是否相等，相等则继续循环到一个属性
* 该属性是否唯一，唯一则判断数据表中是否已有值，有的话继续循环到一个属性（通过 flag 变量做判断，最终的结果是跳过所有循环直到结束，然后上一个 updateByConsole() 方法不保存结果。
* 该属性是否是特殊属性，是的话子类自己实现 specific() 方法；否则赋值给 this， 然后下一个循环，直到遍历所有的 request。

在写这个方法印象最深的还是 php 的这个 get_class() 方法。 php 时不时能在这种地方给我很多惊喜，这种方法真是太实用了！


参考资料：

* [get_class - php.net](http://php.net/manual/en/function.get-class.php)