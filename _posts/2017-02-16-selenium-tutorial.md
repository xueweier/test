---
layout: post
title: selenium 入门
category: tech
tags: selenium spider php composer
---

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201702/selenium1.jpg)

Selenium是用于自动化测试工具，它是在Apache许可证2.0许可的开放源代码工具。Selenium是一套工具，它有助于自动化Web应用程序测试。本文介绍它的快速入门配置与一个很简单的爬虫应用。

# Composer 安装 Selenium：

    composer require facebook/webdriver

# 下载Selenium Server并启动：

下载`Selenium Standalone Server`:<http://www.seleniumhq.org/download/>

安装java jdk后，运行如下命令：

    java -jar selenium-server-standalone-3.0.1.jar
    
# 下载浏览器插件
    
selenium 支持多种浏览器，我目前使用的是chrome。<http://chromedriver.storage.googleapis.com/index.html?path=2.27/>

下载解压，你会得到一个 chromedriver.exe 文件，放到 chrome 的安装目录下（我的：C:\Program Files (x86)\Google\Chrome\Application\）,然后在 path 环境变量中添加 chrome 的安装目录。
    
# 运行测试代码

另启命令提示符，运行后文的php bilibili.php 命令，最后打印了哔哩哔哩顶部的8个视频。

![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201702/QQ%E6%88%AA%E5%9B%BE20170216221036.jpg)




![](http://7vigrt.com1.z0.glb.clouddn.com/blog/pic/201702/QQ%E6%88%AA%E5%9B%BE20170216221048.jpg)

    <?php
    namespace Facebook\WebDriver;

    use Facebook\WebDriver\Remote\DesiredCapabilities;
    use Facebook\WebDriver\Remote\RemoteWebDriver;

    require_once('vendor/autoload.php');

    header("Content-Type: text/html; charset=UTF-8");
    $waitSeconds = 15;  //需等待加载的时间，一般加载时间在0-15秒，如果超过15秒，报错。
    $host = 'http://localhost:4444/wd/hub'; // this is the default
    $capabilities = DesiredCapabilities::chrome();
    $driver = RemoteWebDriver::create($host, $capabilities, 5000);
    $baseUrl = 'http://www.bilibili.com/';
    $driver->get($baseUrl);

    echo consoleText($driver->getTitle()) . "\n";    //cmd.exe中文乱码，所以需转码

    $topLists = $driver->findElement(WebDriverBy::className('container-top-wrapper'))->findElement(WebDriverBy::className('top-list-wrapper'))->findElements(WebDriverBy::tagName('li'));

    foreach ($topLists as $topLi) {
        $itemContent = $topLi->findElement(WebDriverBy::tagName('a'));
        echo consoleText($itemContent->getAttribute('title')) . ' : ' . consoleText($itemContent->getAttribute('href')) . "\n";
    }


    //关闭浏览器
    $driver->quit();

    function consoleText($text, $pageEncoding = '', $consoleEncoding = '')
    {
        // windows
        if (!$consoleEncoding) {
            if (stristr(php_uname('s'), 'win')) {
                $consoleEncoding = "GBK";
            } else {
                $consoleEncoding = 'UTF-8';
            }
        }
        return exchangeEncoding($text, $pageEncoding, $consoleEncoding);
    }

    function exchangeEncoding($text, $pageEncoding = '', $targetEncoding = 'UTF-8')
    {
        if (!$pageEncoding) {
            $pageEncoding = mb_detect_encoding($text, array("ASCII", 'UTF-8', "GB2312", "GBK", 'BIG5'));
        }

        if ($pageEncoding != $targetEncoding) {
            return mb_convert_encoding($text,$targetEncoding,$pageEncoding);
        }

        return $text;
    }

    //切换至最后一个window
    function switchToEndWindow($driver)
    {

        $arr = $driver->getWindowHandles();
        foreach ($arr as $k => $v) {
            if ($k == (count($arr) - 1)) {
                $driver->switchTo()->window($v);
            }
        }
    }

    ?>

# 参考资料

参考文档：
* [PHP Selenium使用教程](http://www.kancloud.cn/wangking/selenium/234575)
* <https://github.com/facebook/php-webdriver>
* <http://facebook.github.io/php-webdriver/namespaces/default.html>
* <https://github.com/chibimagic/WebDriver-PHP/>
* <https://code.google.com/archive/p/php-webdriver-bindings/>
* <https://github.com/Element-34/php-webdriver>
* <https://github.com/Nearsoft/php-selenium-client>
