[workflow demo](https://github.com/cllouud/vllm-ascend/actions/runs/13787245981)


[workflow file](https://github.com/cllouud/vllm-ascend/actions/runs/13787245981/workflow)


# 可选label
[对标 nvidia runner](https://docs.gha-runners.nvidia.com/runners/#example)

昇腾CI的workflow如下：

linux-arm64-npu-1表示1卡。linux-arm64-npu-2表示2卡。inux-arm64-npu-4表示4卡。linux-arm64-npu-8表示8卡。

镜像由`container.images`字段指定。
```yaml
name: Test NPU 1, 2, 4, 8
on:
  workflow_dispatch:
jobs:
  job_0:
    runs-on: linux-arm64-npu-1
    container:
      image: ascendai/cann:latest
      
    steps:
      - name: Show NPU info
        run: |
          npu-smi info
```


# 对多卡的支持
目前1,2,4卡没问题，8卡存在问题。
日志如下，原因还需排查。
```log
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]  ---> System.Exception: The hook script at '/home/runner/k8s/index.js' running command 'PrepareJob' did not execute successfully
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.Container.ContainerHooks.ContainerHookManager.ExecuteHookScript[T](IExecutionContext context, HookInput input, ActionRunStage stage, String prependPath)
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    --- End of inner exception stack trace ---
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.Container.ContainerHooks.ContainerHookManager.ExecuteHookScript[T](IExecutionContext context, HookInput input, ActionRunStage stage, String prependPath)
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.Container.ContainerHooks.ContainerHookManager.PrepareJobAsync(IExecutionContext context, List`1 containers)
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.ContainerOperationProvider.StartContainersAsync(IExecutionContext executionContext, Object data)
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.JobExtensionRunner.RunAsync()
[WORKER 2025-03-11 03:16:27Z ERR  StepsRunner]    at GitHub.Runner.Worker.StepsRunner.RunStepAsync(IStep step, CancellationToken jobCancellationToken)
```

# 共享盘
在demo中，我在[job_0](https://github.com/cllouud/vllm-ascend/actions/runs/13787245981/job/38558057563)runner下载BigFiles仓库，在[其他job](https://github.com/cllouud/vllm-ascend/actions/runs/13787245981/job/38558057921)的共享盘中展示仓库。

# 下载github源码
通过`https://gh-proxy.test.osinfra.cn/`代理可以下载github源码。
