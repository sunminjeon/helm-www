---
title: "Helm Plugin Uninstall"
---

## helm plugin uninstall

卸载一个或多个Helm插件

### 简介

卸载一个或多个Helm插件

```shell
helm plugin uninstall <plugin>... [flags]
```

### 可选项

```shell
  -h, --help   help for uninstall
```

### 从父命令继承的命令

```shell
      --debug                       enable verbose output
      --kube-apiserver string       the address and the port for the Kubernetes API server
      --kube-as-group stringArray   Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --kube-as-user string         Username to impersonate for the operation
      --kube-context string         name of the kubeconfig context to use
      --kube-token string           bearer token used for authentication
      --kubeconfig string           path to the kubeconfig file
  -n, --namespace string            namespace scope for this request
      --registry-config string      path to the registry config file (default "~/.config/helm/registry.json")
      --repository-cache string     path to the file containing cached repository indexes (default "~/.cache/helm/repository")
      --repository-config string    path to the file containing repository names and URLs (default "~/.config/helm/repositories.yaml")
```

### 请参阅

* [helm plugin](helm_plugin.md) - 安装、列举或卸载Helm插件
