# Terraform

## Introduction to Infrastructure as Code with Terraform

- IaC(Infrastructure as Code) 도구를 사용하면 그래픽 사용자 인터페이스가 아닌 구성 파일을 사용하여 인프라를 관리가능
- IaC를 사용하면 버전 지정, 재사용 및 공유할 수 있는 리소스 구성을 정의하여 안전하고 일관되며 반복 가능한 방식으로 인프라를 구축, 변경 및 관리가능
- 사람이 읽을 수 있는 선언적 구성 파일에서 리소스와 인프라를 정의하고 인프라의 수명 주기를 관리가능
  - 여러 클라우드 플랫폼에서 인프라를 관리
  - 사람이 읽을 수 있는 구성 언어를 사용하면 인프라 코드를 빠르게 작성
  - Terraform의 상태를 사용하면 배포 전반에 걸쳐 리소스 변경 사항을 추적
  - 인프라에서 안전하게 협업하기 위해 구성을 버전 제어에 커밋

### Manage any infrastructure
providers 
- API를 통해 클라우드 플랫폼 및 기타 서비스와 연동
- 컴퓨팅 인스턴스나 사설 네트워크와 같은 인프라의 개별 단위를 리소스로 정의
- 다양한 공급자의 리소스를 modoule이라는 재사용 가능한 Terraform 구성으로 구성하고 일관된 언어와 워크플로로 관리
- Terraform 공급자는 리소스 간의 종속성을 자동으로 계산하여 올바른 순서로 생성하거나 제거

### Deployment workflow

![Alt text](https://learn.hashicorp.com/img/terraform/terraform-iac.png)

* Scope - 프로젝트의 인프라 식별
* Author - 인프라 config 작성
* Initialize - 인프라 관리를 위한 플러그인을 설치
* Plan - Terraform이 구성과 일치하는지 변경사항을 미리 확인하는 작업으로 .tf 파일에 작업을 정의하고 리소스 생성, 수정, 삭제에 대한 계획을 보여준다.(명령어 : terraform plan)
* Apply - 계획된 변경사항을 수행하는 작업으로 terraform 프로젝트 디렉토리 내 모든 .tf에 정의된 내용으로 리소스(생성, 수정, 삭제)를 적용하는 단계이다. (명령어 : terraform apply)


## Install Terraform

### Homebrew on OS X

* Install the HashiCorp tap
```
$ brew tap hashicorp/tap
```

* Install Terraform
```
$ brew install hashicorp/tap/terraform
```

* Update Homebrew
```
$ brew update
```

* Download and use the latest Terraform version
```
brew upgrade hashicorp/tap/terraform
==> Upgrading 1 outdated package:
hashicorp/tap/terraform 0.15.3 -> 1.0.0
==> Upgrading hashicorp/tap/terraform 0.15.3 -> 1.0.0
```

* Verify the installation
```
$ terraform -help
Usage: terraform [-version] [-help] <command> [args]
...
```

* install the autocomplete package.
```
$ terraform -install-autocomplete
```

* Start Docker
```
$ open -a Docker
```

* Create a directory
```
$ mkdir learn-terraform-docker-container
$ cd learn-terraform-docker-container
```

* Create main.tf file
```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 2.13.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

* Initialize the project
```
$ terraform init
```

* Provision the NGINX server container 
```
$ terraform apply
```

* Stop the container
```
$ terraform destroy
```
