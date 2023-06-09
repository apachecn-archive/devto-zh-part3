# 🐔我做了一个 PHP 库“conv-拉弗尔”:自动生成 MySQL 迁移查询。🤖

> 原文：<https://dev.to/howyi/-i-made-a-php-library-conv-laravel-auto-generate-mysql-migration-queries--45nl>

[![thumbnail](img/f1835743a60e0e9ab215bad2838f664f.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--RjgVuhNV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/hpm2d58qbtlmwmw14lgl.gif) 
[全尺寸](https://thepracticaldev.s3.amazonaws.com/i/eh53otutrf9c0vtduxlx.gif)

## ![GitHub logo](img/a73f630113876d78cff79f59c2125b24.png) [豪伊](https://github.com/howyi)/[conv-拉勒维尔](https://github.com/howyi/conv-laravel)

### 在 Laravel/Lumen 上自动生成 MySQL 迁移查询

<article class="markdown-body entry-content container-lg" itemprop="text">

# conv-拉勒韦尔

在 Laravel/Lumen 上自动生成 MySQL 迁移查询

[![](img/e38ad1508862cbfe28572931c1aa1a43.png)](https://camo.githubusercontent.com/7a7bde35be28a4cea2a763ae0f45f63f0ad79734/68747470733a2f2f7265732e636c6f7564696e6172792e636f6d2f70726163746963616c6465762f696d6167652f66657463682f732d2d526a675675684e562d2d2f635f6c696d6974253243665f6175746f253243666c5f70726f6772657373697665253243715f3636253243775f3838302f68747470733a2f2f74686570726163746963616c6465762e73332e616d617a6f6e6177732e636f6d2f692f68706d326435387162746c6d776d7731346c676c2e676966)

## 安装

1.  需要库(Laravel/Lumen)

```
composer require "howyi/conv-laravel" --dev 
```

2.  发布配置(仅限 Laravel)

```
php artisan vendor:publish --provider="Howyi\ConvLaravel\ConvServiceProvider" 
```

3.  模式初始化(Laravel/Lumen)

```
php artisan conv:reflect 
```

在`database/schemas`生成创建查询

## 使用

1.  编辑创建查询(在模式目录中)
2.  `php artisan conv:generate`从实际数据库生成迁移-创建查询
3.  `php artisan migrate`要迁移

</article>

[View on GitHub](https://github.com/howyi/conv-laravel)

# 特征

从实际的数据库(\PDO)和 DDLs(创建查询)生成 MySQL 迁移查询

# 动机

我厌倦了编写迁移查询...😌

## 安装

1.  需要库(Laravel/Lumen)

```
composer require "howyi/conv-laravel" --dev 
```

1.  发布配置(仅限 Laravel)

```
php artisan vendor:publish --provider="Howyi\ConvLaravel\ConvServiceProvider" 
```

1.  模式初始化(Laravel/Lumen)

```
php artisan conv:reflect 
```

在`database/schemas`生成创建查询

## 用法

1.  编辑创建查询(在模式目录中)
2.  `php artisan conv:generate`从实际数据库生成迁移-创建查询
3.  `php artisan migrate`要迁移

# 举例

示例存储库😉

## ![GitHub logo](img/a73f630113876d78cff79f59c2125b24.png) [豪伊](https://github.com/howyi)/[conv-拉腊维尔-举例](https://github.com/howyi/conv-laravel-example)

<article class="markdown-body entry-content container-lg" itemprop="text">

[conv-拉勒维尔](https://github.com/howyi/conv-laravel)的示例存储库

</article>

[View on GitHub](https://github.com/howyi/conv-laravel-example)