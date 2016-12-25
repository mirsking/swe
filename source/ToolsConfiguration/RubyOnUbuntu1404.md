# Ubuntu14.04上安装Ruby2.2.0

## 安装Ruby2.2.0
1. 安装rvm

    ```bash
    sudo apt-get update
    sudo apt-get install build-essential make curl
    curl -L https://get.rvm.io | bash -s stable
    source ~/.bash_rc
    ```

2. 安装ruby

    ```bash
    rvm install ruby-2.2.0
    ```

3. 选择默认ruby
    重启终端后运行：
    ```bash
    bash --login
    rvm use --default ruby-2.2.0
    ```
    [bash --login](https://ruby-china.org/topics/3705)
