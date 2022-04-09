
# 如何为不同账户配置不同的 ssh key

有时我们需要使用多个 git 账户来推送代码，比如同时使用公司和私人账户。这就需要我们使用多个 ssh key 来进行身份验证。

如何生成 ssh key 和如何在 github 账户里配置 key，这里就不再赘述，重点在于在同一台电脑上为不同账户配置不同的 key。

首先，我们需要修改 ssh config, 重点是添加不同的 `Host` 来映射不同的 ssh 私钥文件地址。例如下面的代码中，我们使用 `id_rsa_a` 作为 `gh_personal` 所使用的私钥，`id_rsa_b` 作为 `gh_work` 使用的私钥。

```

## ~/.ssh/conifg

Host gh_personal
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_a
    User git

Host gh_work
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_b
    User git
```

当然 `id_rsa_a` 和 `id_rsa_b` 需要是已经被添加到系统的 ssh key，可以用 `ssh-add -l` 来确认。

然后，是比较关键的一点，当我们拉下仓库代码之后，需要修改 remote url 为带有 host 的 url。拿本 repo 举例，默认 git clone 地址为：
```
git@github.com:zzayne/exp.git
```
需要修改为 
```
git@gh_personal:zzayne/exp.git
```

这样 git 就会自动使用 host 匹配的 ssh key来推送代码。


## Reference
[Using Multiple SSH keys - Beginner Friendly](https://gist.github.com/aprilmintacpineda/f101bf5fd34f1e6664497cf4b9b9345f)
