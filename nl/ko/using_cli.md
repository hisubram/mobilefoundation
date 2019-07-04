---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-10"

keywords: command line, cli, mfpdev-cli, cli commands

subcollection:  mobilefoundation
---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}
{:note: .note}

# Mobile Foundation CLI
{: #mobile_foundation_cli}

{{ site.data.keyword.mobilefoundation_short }}은 {{site.data.keyword.mobilefoundation_short}} 클라이언트 및 서버 아티팩트를 쉽게 관리할 수 있도록 개발자용 명령행 인터페이스(CLI) 도구 **mfpdev**를 제공합니다.

모든 `mfpdev` 명령은 대화식 또는 직접 모드로 실행될 수 있습니다. 대화식 모드에서는 명령에 필요한 매개변수에 대한 프롬프트가 표시되며 일부 기본값이 사용됩니다. 직접 모드에서는 명령과 함께 매개변수를 제공해야 합니다.


## {{site.data.keyword.mobilefoundation_short}} CLI 설치
{: #installing_mf_cli}

{{site.data.keyword.mobilefoundation_short}}은 [NPM 레지스트리 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.npmjs.com){: new_window}에서 NPM 패키지로 사용 가능합니다.

NPM 패키지를 설치하려면 **node.js** 및 **npm**이 설치되어 있는지 확인하십시오. [nodejs.org ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nodejs.org/){: new_window}의 설치 지시사항에 따라 **node.js**를 설치하십시오. **node.js**가 올바르게 설치되었는지 확인하려면 다음 명령을 실행하십시오.
```bash
node -v
```
{: codeblock}

지원되는 최소 node.js 버전은 **4.2.3**입니다. 또한 node 및 npm 패키지는 빠르게 진화하므로 최신 버전을 포함하여 사용 가능한 모든 버전의 node 및 npm에서 {{site.data.keyword.mobilefoundation_short}} CLI가 완벽하게 기능하지 않을 수 있습니다. CLI가 올바르게 기능하려면 node의 버전이 **6.11.1**이고 npm의 버전이 **3.10.10**인지 확인하십시오.
{: note}

MobileFirst CLI 임시 수정사항 버전 *8.0.2018100112* 이상의 경우 Node 버전 8.x 또는 10.x를 사용할 수 있습니다.

* {{site.data.keyword.mobilefoundation_short}} CLI를 설치하려면 다음 명령을 실행하십시오.
    ```bash
    npm install -g mfpdev-cli
    ```
    {: codeblock}

* MobileFirst Operations Console의 다운로드 센터에서 CLI 압축 파일(*.zip*)을 다운로드한 경우 다음 명령을 사용하십시오.
    ```bash
    npm install -g <path-to-mfpdev-cli.tgz>
    ```
    {: codeblock}

* 선택적 종속 항목 없이 CLI를 설치하려면 다음과 같이 `--no-optional` 플래그를 추가하십시오.
    ```bash
    npm install -g --no-optional path-to-mfpdev-cli.tgz
    ```
    {: codeblock}

Node 8을 사용하는 MobileFirst CLI를 설치하는 동안 터미널 창에서 다음 오류 중 몇 가지를 볼 수 있습니다.
```text
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/bufferutil
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> utf-8-validate@1.2.2 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
> node-gyp rebuild

gyp ERR! clean error 
gyp ERR! stack Error: EACCES: permission denied, rmdir 'build'
gyp ERR! System Darwin 18.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /usr/local/lib/node_modules/mfpdev-cli/node_modules/utf-8-validate
gyp ERR! node -v v8.12.0
gyp ERR! node-gyp -v v3.8.0
gyp ERR! not ok 

> fsevents@1.2.4 install /usr/local/lib/node_modules/mfpdev-cli/node_modules/fsevents
> node install
```
{: codeblock}

이 오류는 [node-gyp의 알려진 버그 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/nodejs/node-gyp/issues/1547){: new_window}로 인해 발생합니다. 이러한 오류는 MobileFirst CLI의 작동에 영향을 미치지 않으므로 무시할 수 있습니다. 이 문제는 `mfpdev-cli` 임시 수정사항 레벨 *8.0.2018100112* 이상에 적용 가능합니다. 이 오류를 극복하려면 설치 중에 `--no-optional` 플래그를 사용하십시오.

* CLI가 올바르게 설치되었는지 확인하려면 다음 명령을 실행하십시오.
    ```bash
    mfpdev
    ```
    {: codeblock}

* CLI 도움말이 출력으로 인쇄됩니다.

    ```
 NAME
     IBM MobileFirst Foundation Command Line Interface (CLI).

     SYNOPSIS
     mfpdev <command> [options]

     DESCRIPTION
     The IBM MobileFirst Foundation Command Line Interface (CLI) is a command-line
     for developing MobileFirst applications. The command-line can be used by itself, or in conjunction  with the IBM MobileFirst Foundation Operations Console. Some functions are available from  the command-line only and not the console.

         For more information and a step-by-step example of using the CLI, see the IBM Knowledge Center for your version of IBM MobileFirst Foundation at
     https://www.ibm.com/support/knowledgecenter.
         ...
         ...
         ...
    ```
    {: screen}

## {{site.data.keyword.mobilefoundation_short}} CLI 명령 목록
{: #list_cli_commands}

<table summary="명령에 대한 자세한 정보를 제공하는 링크가 있는 {{site.data.keyword.mobilefoundation_short}} CLI 명령">
<caption>표 1. {{site.data.keyword.mobilefoundation_short}} CLI 명령</caption>
 <thead>
 <th colspan="6">일반 [mfpdev](#mfpdev) 명령</th>
 </thead>
 <tbody>
 <tr>
 <td>[app](#mfpdev_app)</td>
 <td>[server](#mfpdev_server)</td>
 <td>[adapter](#mfpdev_adapter)</td>
 <td>[help](#mfpdev_help)</td>
 </tr>
   </tbody>
 </table>

### mfpdev
{: #mfpdev}

mfpdev 명령행 인터페이스의 미리보기 브라우저 유형, 미리보기 제한시간 값 및 서버 제한시간 값에 대한 구성 환경 설정을 설정합니다.

```bash
mfpdev config
```
{: codeblock}

운영 체제, 메모리 이용, 노드 버전 및 명령행 인터페이스 버전을 포함하여 환경에 대한 정보를 표시합니다. 현재 디렉토리가 Cordova 애플리케이션인 경우 Cordova `cordova info` 명령을 통해 제공되는 정보도 표시됩니다.

```bash
mfpdev info
```
{: codeblock}

현재 사용 중인 {{site.data.keyword.mobilefoundation_short}} CLI의 버전 번호를 표시합니다.

```bash
mfpdev -v
```
{: codeblock}

디버그 모드: 디버그 출력을 생성합니다.

```bash
mfpdev [-d|--debug]
```
{: codeblock}

상세 디버그 모드: 상세 디버그 출력을 생성합니다.

```bash
mfpdev [-dd|--ddebug]
```
{: codeblock}

명령 출력에 색상을 사용하지 않습니다.

```bash
mfpdev -no-color
```
{: codeblock}

### mfpdev help
{: #mfpdev_help}

MobileFirst CLI(mfpdev) 명령에 대한 도움말을 표시합니다. 명령어를 인수로 사용하여 각 명령 유형 또는 명령에 대한 자세한 도움말 텍스트를 표시합니다 (예: `mfpdev help server add`).

```bash
mfpdev help <command name>
```
{: codeblock}


### mfpdev app
{: #mfpdev_app}

MobileFirst Server에 앱을 등록합니다.

```bash
mfpdev app register
```
{: codeblock}

기본이 아닌 서버 및 런타임에 앱을 등록하려면 다음 명령을 사용하십시오.

```bash
mfpdev app register <server> <runtime>
```
{: codeblock}

Cordova Windows 플랫폼의 경우 명령에 `-w <platform>` 인수를 추가해야 합니다. 플랫폼 인수는 등록될 Windows 플랫폼의 쉼표로 구분된 목록입니다. 올바른 값은 windows, windows8 및 windowsphone8입니다.

```bash
mfpdev app register -w windows8
```
{: codeblock}

앱에 사용할 백엔드 서버 및 런타임을 지정할 수 있습니다. Cordova 앱의 경우 이 명령을 사용하여 시스템 메시지의 기본 언어 및 체크섬 보안 검사 수행 여부와 같은 몇 가지 추가 측면을 구성할 수 있습니다. Cordova 앱에 대한 기타 구성 매개변수가 포함되어 있습니다.

```bash
mfpdev app config
```
{: codeblock}

서버에서 기존 앱 구성을 검색합니다.

```bash
mfpdev app pull
```
{: codeblock}

앱 구성을 서버로 전송합니다.

```bash
mfpdev app push
```
{: codeblock}

대상 플랫폼 유형의 실제 디바이스 없이 Cordova 앱을 미리봅니다. Mobile Browser Simulator 또는 웹 브라우저에서 미리보기가 가능합니다.

```bash
mfpdev app preview
```
{: codeblock}

www 디렉토리에 있는 애플리케이션 리소스를 직접 업데이트 프로세스에 사용될 수 있는 압축(*.zip*) 파일로 패키지합니다.

```bash
mfpdev app webupdate
```
{: codeblock}

이 명령은 업데이트된 웹 리소스를 압축(*.zip*) 파일로 패키지하여 등록된 기본 MobileFirst Server에 업로드합니다. 패키지된 웹 리소스는 `[cordova-project-root-folder]/mobilefirst/` 폴더에 있습니다.

웹 리소스를 다른 서버 인스턴스에 업로드하려면 서버 이름 및 런타임을 명령의 일부로 제공하십시오.

```bash
mfpdev app webupdate <server_name> <runtime>
```
{: codeblock}

–build 매개변수를 사용하면 서버에 업로드하지 않고 패키지된 웹 리소스가 포함된 압축(*.zip*) 파일을 생성할 수 있습니다.
```bash
mfpdev app webupdate --build
```
{: codeblock}

이전에 빌드된 패키지를 업로드하려면 –file 매개변수를 사용하십시오.
```bash
mfpdev app webupdate --file mobilefirst/com.ibm.test-android-1.0.0.zip
```
{: codeblock}

`–encrypt` 매개변수를 사용하여 패키지의 컨텐츠를 암호화하는 옵션도 있습니다.
```bash
mfpdev app webupdate --encrypt
```
{: codeblock}


### mfpdev server
{: #mfpdev_server}

MobileFirst Server에 대한 정보를 표시합니다.

```bash
mfpdev server info
```
{: codeblock}

환경에 서버 정의를 추가합니다.

```bash
mfpdev server add
```
{: codeblock}

서버 정의를 편집합니다.

```bash
mfpdev server edit
```
{: codeblock}

서버를 기본 서버로 설정하려면 다음 명령을 사용하십시오.

```bash
mfpdev server edit <server_name> --setdefault
```
{: codeblock}

환경에서 서버 정의를 제거합니다.

```bash
mfpdev server remove
```
{: codeblock}

MobileFirst Operations Console을 엽니다.

```bash
mfpdev server console
```
{: codeblock}

다른 서버의 콘솔을 열려면 서버 이름을 명령의 매개변수로 제공하십시오.

```bash
mfpdev server console <server-name>
```
{: codeblock}

앱을 등록 취소하고 MobileFirst Server에서 어댑터를 제거합니다.

```bash
mfpdev server clean
```
{: codeblock}

### mfpdev adapter
{: #mfpdev_adapter}

어댑터를 작성합니다.

```bash
mfpdev adapter create
```
{: codeblock}

어댑터를 빌드합니다.

```bash
mfpdev adapter build
```
{: codeblock}

현재 디렉토리와 해당 서브디렉토리에서 모든 어댑터를 찾아서 빌드합니다.

```bash
mfpdev adapter build all
```
{: codeblock}

어댑터를 MobileFirst Server에 배치합니다.

```bash
mfpdev adapter deploy
```
{: codeblock}

다른 서버에 배치하려면 다음 명령을 사용하십시오.
```bash
mfpdev adapter deploy <server_name>
```
{: codeblock}

현재 디렉토리와 해당 서브디렉토리에서 모든 어댑터를 찾아서 MobileFirst Server에 배치합니다.

```bash
mfpdev adapter deploy all
```
{: codeblock}

MobileFirst Server에서 어댑터의 프로시저를 호출합니다.

```bash
mfpdev adapter call
```
{: codeblock}

서버에서 기존 어댑터 구성을 검색합니다.
```bash
mfpdev adapter pull
```
{: codeblock}

어댑터 구성을 서버에 전송합니다.
```bash
mfpdev adapter push
```
{: codeblock}

## {{site.data.keyword.mobilefoundation_short}} CLI 업데이트 및 설치 제거
{: #update_uninstall_mf_cli}

명령행 인터페이스를 업데이트하려면 다음 명령을 실행하십시오.

```bash
npm update -g mfpdev-cli
```
{: codeblock}

명령행 인터페이스를 설치 제거하려면 다음 명령을 실행하십시오.

```bash
npm uninstall -g mfpdev-cli
```
{: codeblock}
