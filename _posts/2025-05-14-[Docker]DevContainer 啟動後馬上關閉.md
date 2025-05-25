# ✅ 解決 DevContainer 啟動後馬上關閉的最佳實踐筆記

## 🎯 問題描述
當 DevContainer 使用開發用 Dockerfile 時，常遇到容器啟動後「馬上關閉」，導致 VS Code 無法掛載工作環境。

## 錯誤LOG

``` bash
Logs:
[217 ms] Dev Containers 0.288.1 in VS Code 1.77.3 (704ed70d4fd1c6bd6342c436f1ede30d1cff4710).
[216 ms] Start: Resolving Remote
[358 ms] Setting up container for folder or workspace: <REPO_PATH>
[362 ms] Start: Run: wsl -l -v
[457 ms] Start: Run: wsl -d docker-desktop -e /bin/sh -c echo ~
[743 ms] Start: Run: wsl -d docker-desktop -e /bin/sh -c cd '/root' && /bin/sh
[757 ms] Start: Run in host: id -un
[842 ms] root
[844 ms]
[845 ms] Start: Run in host: cat /etc/passwd
[849 ms] Start: Run in host: echo ~
[850 ms] /root
[850 ms]
[851 ms] Start: Run in host: test -x '/root/.vscode-remote-containers/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710/node'
[871 ms]
[872 ms]
[873 ms] Start: Run in host: test -f '/root/.vscode-remote-containers/dist/vscode-remote-containers-server-0.288.1.js'
[876 ms]
[876 ms]
[883 ms] userEnvProbe: loginInteractiveShell (default)
[883 ms] userEnvProbe: not found in cache
[884 ms] userEnvProbe shell: /bin/sh
[888 ms] Host server: /bin/sh: /root/.vscode-remote-containers/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710/node: not found
[940 ms] Error: stream ended with:0 but wanted:9
at c (.vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\extension\extension.js:11:101670)
at .vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\extension\extension.js:11:101851
at s (.vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\extension\extension.js:14:5371)
at Socket. (.vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\extension\extension.js:14:5541)
at Socket.emit (node:events:538:35)
at endReadableNT (node:internal/streams/readable:1345:12)
at process.processTicksAndRejections (node:internal/process/task_queues:83:21)
[941 ms] Could not connect to WSL.
[941 ms] stream is closed
[944 ms] Start: Check Docker is running
[945 ms] Start: Run: docker version --format {{.Server.APIVersion}}
[949 ms] Host server terminated (code: 127, signal: null).
[1251 ms] Server API version: 1.41
[1252 ms] Start: Run: docker volume ls -q
[1483 ms] Start: Run: docker ps -q -a --filter label=vsch.local.folder=<REPO_PATH> --filter label=vsch.quality=stable
[1744 ms] Start: Run: docker ps -q -a --filter label=devcontainer.local_folder=<REPO_PATH> --filter label=devcontainer.config_file=<REPO_PATH>.devcontainer.json
[1973 ms] Start: Run: docker inspect --type container d2b769fd1851
[2189 ms] Start: Run: docker ps -q -a --filter label=devcontainer.local_folder=<REPO_PATH>
[2422 ms] Start: Run: docker inspect --type container d2b769fd1851
[2820 ms] Start: Run: \AppData\Local\Programs\Microsoft VS Code\Code.exe --ms-enable-electron-run-as-node .vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\spec-node\devContainersSpecCLI.js up --user-data-folder \AppData\Roaming\Code\User\globalStorage\ms-vscode-remote.remote-containers\data --container-session-data-folder /tmp/devcontainers-1db0b745-e0a4-4ecb-b0ae-c4077a511aee1683123241632 --workspace-folder <REPO_PATH> --workspace-mount-consistency cached --id-label devcontainer.local_folder=<REPO_PATH> --id-label devcontainer.config_file=<REPO_PATH>.devcontainer.json --log-level debug --log-format json --config <REPO_PATH>.devcontainer.json --default-user-env-probe loginInteractiveShell --mount type=volume,source=vscode,target=/vscode,external=true --skip-post-create --update-remote-user-uid-default on --mount-workspace-git-root true
[3252 ms] @devcontainers/cli 0.35.0. Node.js v16.14.2. win32 10.0.19045 x64.
[3252 ms] Start: Run: docker buildx version
[3623 ms] github.com/docker/buildx v0.8.2 6224def4dd2c3d347eee19db595348c50d7cb491
[3623 ms]
[3623 ms] Start: Resolving Remote
[3626 ms] Start: Run: git rev-parse --show-cdup
[3714 ms] Start: Run: docker ps -q -a --filter label=devcontainer.local_folder=<REPO_PATH> --filter label=devcontainer.config_file=<REPO_PATH>.devcontainer.json
[3945 ms] Start: Run: docker inspect --type container d2b769fd1851
[4161 ms] Start: Starting container
[4161 ms] Start: Run: docker start d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[4735 ms] Start: Run: docker ps -q -a --filter label=devcontainer.local_folder=<REPO_PATH> --filter label=devcontainer.config_file=<REPO_PATH>.devcontainer.json
[4962 ms] Start: Run: docker inspect --type container d2b769fd1851
[5204 ms] Start: Inspecting container
[5204 ms] Start: Run: docker inspect --type container d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[5427 ms] Start: Run in container: /bin/sh
[5453 ms] Start: Run in container: uname -m
[5693 ms] x86_64
[5693 ms]
[5693 ms] Start: Run in container: (cat /etc/os-release || cat /usr/lib/os-release) 2>/dev/null
[5695 ms] NAME="Ubuntu"
VERSION="20.04.2 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.2 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
[5695 ms]
[5696 ms] Start: Run in container: cat /etc/passwd
[5702 ms] Start: Run in container: test -f '/var/devcontainer/.patchEtcEnvironmentMarker'
[5703 ms]
[5704 ms]
[5704 ms] Start: Run in container: test -f '/var/devcontainer/.patchEtcProfileMarker'
[5705 ms]
[5705 ms]
[5724 ms] Start: Run: docker inspect --type container d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[6022 ms] Start: Run: docker exec -i -u root d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b /bin/sh -c echo "New container started. Keep-alive process started." ; export VSCODE_REMOTE_CONTAINERS_SESSION=1db0b745-e0a4-4ecb-b0ae-c4077a511aee1683123241632 ; /bin/sh
[6025 ms] Start: Run: \AppData\Local\Programs\Microsoft VS Code\Code.exe --ms-enable-electron-run-as-node .vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\spec-node\devContainersSpecCLI.js read-configuration --workspace-folder <REPO_PATH> --id-label devcontainer.local_folder=<REPO_PATH> --id-label devcontainer.config_file=<REPO_PATH>.devcontainer.json --container-id d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b --log-level debug --log-format json --config <REPO_PATH>.devcontainer.json --mount-workspace-git-root true
[6426 ms] New container started. Keep-alive process started.
[6597 ms] @devcontainers/cli 0.35.0. Node.js v16.14.2. win32 10.0.19045 x64.
[6596 ms] Start: Run: git rev-parse --show-cdup
[6690 ms] Start: Run: docker inspect --type container d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[6939 ms] Start: Run: \AppData\Local\Programs\Microsoft VS Code\Code.exe --ms-enable-electron-run-as-node .vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\spec-node\devContainersSpecCLI.js read-configuration --workspace-folder <REPO_PATH> --id-label devcontainer.local_folder=<REPO_PATH> --id-label devcontainer.config_file=<REPO_PATH>.devcontainer.json --container-id d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b --log-level debug --log-format json --config <REPO_PATH>.devcontainer.json --include-merged-configuration --mount-workspace-git-root true
[7340 ms] @devcontainers/cli 0.35.0. Node.js v16.14.2. win32 10.0.19045 x64.
[7340 ms] Start: Run: git rev-parse --show-cdup
[7435 ms] Start: Run: docker inspect --type container d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[7667 ms] Start: Inspecting container
[7668 ms] Start: Run: docker inspect --type container d2b769fd1851e4b48c55dc40b1a08a9ea54a4bb32c02b00dbf0d5db7c084364b
[7884 ms] Start: Run in container: /bin/sh
[7911 ms] Start: Run in container: uname -m
[8155 ms] x86_64
[8156 ms]
[8156 ms] Start: Run in container: (cat /etc/os-release || cat /usr/lib/os-release) 2>/dev/null
[8159 ms] NAME="Ubuntu"
VERSION="20.04.2 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.2 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
[8159 ms]
[8160 ms] Start: Run in container: cat /etc/passwd
[8163 ms] Start: Setup shutdown monitor
[8164 ms] Forking shutdown monitor: .vscode\extensions\ms-vscode-remote.remote-containers-0.288.1\dist\shutdown\shutdownMonitorProcess \.\pipe\vscode-remote-containers-e7f2ce67-8c68-498a-b8b9-67287a658092-sock singleContainer Debug \AppData\Roaming\Code\logs\20230503T161401\window1\exthost\ms-vscode-remote.remote-containers 1683123242784
[8182 ms] Start: Run in container: test -d /home/builder/.vscode-server
[8190 ms]
[8191 ms]
[8191 ms] Start: Run in container: test ! -f '/home/builder/.vscode-server/data/Machine/.writeMachineSettingsMarker' && set -o noclobber && mkdir -p '/home/builder/.vscode-server/data/Machine' && { > '/home/builder/.vscode-server/data/Machine/.writeMachineSettingsMarker' ; } 2> /dev/null
[8198 ms]
[8199 ms]
[8199 ms] Exit code 1
[8200 ms] Start: Run in container: cat /home/builder/.vscode-server/data/Machine/settings.json
[8207 ms]
[8207 ms] cat: /home/builder/.vscode-server/data/Machine/settings.json: No such file or directory
[8207 ms] Exit code 1
[8208 ms] Start: Run in container: test -d /home/builder/.vscode-server/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710
[8213 ms]
[8214 ms]
[8214 ms] Exit code 1
[8214 ms] Start: Run in container: test -d /vscode/vscode-server/bin/linux-x64/704ed70d4fd1c6bd6342c436f1ede30d1cff4710
[8219 ms]
[8219 ms]
[8220 ms] Start: Run in container: mkdir -p '/home/builder/.vscode-server/bin' && ln -snf '/vscode/vscode-server/bin/linux-x64/704ed70d4fd1c6bd6342c436f1ede30d1cff4710' '/home/builder/.vscode-server/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710'
[8230 ms]
[8230 ms] ln: failed to create symbolic link '/home/builder/.vscode-server/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710': Input/output error
[8230 ms] Exit code 1
[8238 ms] Command in container failed: mkdir -p '/home/builder/.vscode-server/bin' && ln -snf '/vscode/vscode-server/bin/linux-x64/704ed70d4fd1c6bd6342c436f1ede30d1cff4710' '/home/builder/.vscode-server/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710'
[8238 ms] ln: failed to create symbolic link '/home/builder/.vscode-server/bin/704ed70d4fd1c6bd6342c436f1ede30d1cff4710': Input/output error
[8239 ms] Exit code 1
```

## 🧠 問題原因
- Dockerfile 沒有指定長時間運作的 CMD 或 ENTRYPOINT。
- 容器啟動後沒有事情可做，因此直接退出。

## ✅ 推薦解決方案

### 在 `.devcontainer/Dockerfile` 最後加入：
```dockerfile
CMD ["sleep", "infinity"]
```

## 📋 完整範例如下：
```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    vim \
    unzip \
    default-mysql-client

WORKDIR /workspace

CMD ["sleep", "infinity"]
```

## ✅ 效果
- 容器啟動後保持運作狀態。
- VS Code 成功進入 Dev Container。
- 你可以在容器中自由執行 dotnet 指令：
  ```bash
  dotnet build
  dotnet run
  ```

## 🟢 檢查結果
- `docker-compose up` 兩個服務都成功啟動。
- VS Code 可正常進入容器進行開發。

---

✅ 這是符合最佳實踐的開發環境設計方式。