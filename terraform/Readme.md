# 注意
本部署均基于`ubuntu`操作系统

* 需要一对已经存在的`ssh key`，可以通过`ssh-keygen`生成
* 注意如果修改了外部脚本的地址，需要在对应的`module.tf`里修改`file`的路径
* `shell`脚本中需要为耗时操作设置等待时间，否则直接会退出，例如`sudo`操作时的权限竞争
* 创建`s3`需要注意名称只允许小写字母和数字
* 注意采用了`lifecycle`的属性之后的无法通过`terraform destroy`直接进行销毁
* 加速构建可以采用

```
provider "aws" {
  region = "us-east-1"

  # Make it faster by skipping something
  skip_get_ec2_platforms      = true
  skip_metadata_api_check     = true
  skip_region_validation      = true
  skip_credentials_validation = true
  skip_requesting_account_id  = true
}
```


# 问题
* [Terraform doesn't reuse an AWS Role it just created and fails?](https://github.com/hashicorp/terraform/issues/8184)

# 使用指南

* 无需交互的启动机器

`terraform apply -input=false` Mac目前无用
`terraform apply -auto-approve=false` Mac目前无用

* 从命令行启动一个以配置为`bch`的机器

`terraform init -var "source=coin/bch"`

* 从配置文件启动一个以某某为配置的机器

`terraform init -var-file terraform.tfvars`

* 导入aws key

` terraform import aws_key_pair.local local `

* [远程执行的nohup并不运行](https://stackoverflow.com/questions/36207752/how-can-i-start-a-remote-service-using-terraform-provisioning)

```
  provisioner "remote-exec" {
    inline = [
      "sudo bash /tmp/install_krb.sh" ,
      "nohup karbowanecd &>/dev/null &" ,
      "sleep 1",
    ]
  }

```

# ToDo 

- [x] 存储状态到S3，开启S3记录其版本
- [ ] 更新远程状态，推送脚本
- [ ] 自动扩展机器性能
- [x] 多区域部署机器
- [ ] EFS的跨区配置, 目前看来只能在支持的区域内，用同一个
- [x] lifecycle使用
- [ ] 规范化目录结构和文件名命名
- [ ] 全局变量如何使用，多个子项目内如何使用
- [x] 控制ec2向s3拉取数据
- [x] 创建IAM子账号
- [ ] timeout
- [ ] 检查是否存在某个资源或者策略，然后判断是否接着部署
- [x] 配置S3策略
- [ ] 复用S3策略
- [x] 启动特定IAM的实例
- [ ] 前后依赖的顺序关系
- [x] 编写同步脚本并由该实例执行，脚本应该是命令行配置的，例如`./sync.sh btc`, `./sync.sh dash`

# Other

* 导出现有的aws配置
`terraforming s3 `

* 有时候不同的区域内可能有不同的镜像，所以要通过过滤的形式然后去构建
