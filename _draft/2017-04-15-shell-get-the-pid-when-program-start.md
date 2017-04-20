---
layout: post
title: 
category: tech
tags: linux shell
---

![](/assets/img/linux.jpg)

最近写到相关的代码，做个记录。

    BetaCatResume::with(['talent' => function ($query) {
            $query->whereNotNull('contact_email')->where('contact_email_validate', true);
        }])
            ->whereNotNull('expected_job_name_vector')
            ->where('expected_salary', '<', $this->salary_average)
            ->orWhereNull('expected_salary')
            ->where(function ($q) use ($title_vector_array) {
                foreach ($title_vector_array as $index => $job_name) {
                    if ($this->abandonSegment($job_name)) continue;
                    // 职位名的某个分词 在期望职位分词里。
                    if ($index == 0) {
                        $q->where("expected_job_name_str", 'like', '%' . $job_name . "%");
                    } else {
                        $q->orWhere("expected_job_name_str", 'like', '%' . $job_name . "%");
                    }
                }
            })->chunk(1000, function ($rcdResumes) use ($e, &$sendcount, $logFlag) {
    
# 参考资料
    
* [如何在启动一个进程时获取其PID？ - Ubuntu 论坛](http://forum.ubuntu.com.cn/viewtopic.php?f=21&t=260541)    
