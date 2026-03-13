## This is a repo for personal web

### 环境要求

- rbenv (用于管理 Ruby 版本) 1.3.2-10
- Ruby 3.2.2
- bundler 2.6.5

#### 环境配置

配置 Ruby 和 bundler

```bash
# 安装 rbenv（如未安装）
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-installer | bash 
# 安装 rbenv 脚本会自动将 export PATH="$HOME/.rbenv/bin:$PATH" 和 eval "$(rbenv init -)" 添加到 ~/.bashrc 或 ~/.zshrc 中
source ~/.bashrc  # 或者 source ~/.zshrc
# 安装系统依赖
sudo apt update
sudo apt install -y build-essential libssl-dev zlib1g-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev libffi-dev
# Ruby
rbenv install 3.2.2
rbenv global 3.2.2
ruby -v  # 确认 ruby 版本
gem install bundler -v 2.6.5
bundle -v  # 确认 bundler 版本
```

使用 bundler 安装依赖

```bash
# 进入项目根目录
bundle install
```

### 构建方法

```bash
# 根据 Gemfile 构建网站
bundle exec jekyll build

# 本地运行
bundle exec jekyll serve
```

可以在本地构建后通过 local://127.0.0.1:port/ 查看

<!-- ---
layout: single
# author_profile: true  # author
title: "Welcome"
header:
  overlay_image: /assets/images/wallpaper.jpg  # 头部背景图片
  overlay_filter: 0.8  # 背景图片的透明度
--- -->