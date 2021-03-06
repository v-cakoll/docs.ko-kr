---
title: dotnet vstest 명령
description: dotnet vtest 명령은 프로젝트와 모든 종속성을 빌드합니다.
ms.date: 02/27/2020
ms.openlocfilehash: f7db58f4aab59354b8c69ce0371324c23482dafe
ms.sourcegitcommit: fff146ba3fd1762c8c432d95c8b877825ae536fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82975396"
---
# <a name="dotnet-vstest"></a>dotnet vstest

**이 문서의 적용 대상:**  ✔️ .NET Core 2.1 SDK 이상 버전

> [!IMPORTANT]
> `dotnet vstest` 명령은 이제 어셈블리를 실행하는 데 사용할 수 있는 `dotnet test`로 대체됩니다. [`dotnet test`](dotnet-test.md)을 참조하세요.

## <a name="name"></a>이름

`dotnet-vstest` - 지정한 어셈블리에서 테스트를 실행합니다.

## <a name="synopsis"></a>개요

```dotnetcli
dotnet vstest [<TEST_FILE_NAMES>] [--Blame] [--Diag <PATH_TO_LOG_FILE>]
    [--Framework <FRAMEWORK>] [--InIsolation] [-lt|--ListTests <FILE_NAME>]
    [--logger <LOGGER_URI/FRIENDLY_NAME>] [--Parallel]
    [--ParentProcessId <PROCESS_ID>] [--Platform] <PLATFORM_TYPE>
    [--Port <PORT>] [--ResultsDirectory<PATH>] [--Settings <SETTINGS_FILE>]
    [--TestAdapterPath <PATH>] [--TestCaseFilter <EXPRESSION>]
    [--Tests <TEST_NAMES>] [[--] <args>...]]

dotnet vstest -?|--Help
```

## <a name="description"></a>설명

`dotnet-vstest` 명령은 `VSTest.Console` 명령줄 애플리케이션을 실행하여 자동화된 단위 테스트를 실행합니다.

## <a name="arguments"></a>인수

- **`TEST_FILE_NAMES`**

  지정한 어셈블리에서 테스트를 실행합니다. 여러 테스트 어셈블리 이름을 공백으로 구분합니다. 와일드카드가 지원됩니다.

## <a name="options"></a>옵션

- **`--Blame`**

  테스트를 원인 모드로 실행합니다. 이 옵션은 테스트 호스트를 충돌시키는 문제가 있는 테스트를 격리하는 데 유용합니다. 현재 디렉터리에 크래시 전에 테스트 실행 순서를 캡처하는 *Sequence.xml*이라는 출력 파일을 만듭니다.

- **`--Diag <PATH_TO_LOG_FILE>`**

  테스트 플랫폼에 대한 자세한 정보 로그를 사용하도록 설정합니다. 로그는 제공된 파일에 기록됩니다.

- **`--Framework <FRAMEWORK>`**

  테스트 실행에 사용되는 대상 .NET Framework 버전입니다. 유효한 값의 예는 `.NETFramework,Version=v4.6` 또는 `.NETCoreApp,Version=v1.0`입니다. 지원되는 기타 값에는 `Framework40`, `Framework45`, `FrameworkCore10` 및 `FrameworkUap10`이 있습니다.

- **`--InIsolation`**

  격리 모드에서 테스트를 실행합니다. 이렇게 하면 *vstest.console.exe* 프로세스가 테스트 시 오류에서 중지될 가능성은 매우 적지만 테스트 속도가 느려질 수 있습니다.

- **`-lt|--ListTests <FILE_NAME>`**

  지정된 테스트 컨테이너에서 검색된 모든 테스트를 나열합니다.

- **`--logger <LOGGER_URI/FRIENDLY_NAME>`**

  테스트 결과에 대해 로거를 지정합니다.

  - Team Foundation Server에 테스트 결과를 게시하려면 `TfsPublisher` 로거 공급자를 사용합니다.

    ```console
    /logger:TfsPublisher;
        Collection=<team project collection url>;
        BuildName=<build name>;
        TeamProject=<team project name>
        [;Platform=<Defaults to "Any CPU">]
        [;Flavor=<Defaults to "Debug">]
        [;RunTitle=<title>]
    ```

  - Visual Studio 테스트 결과 파일(TRX)에 결과를 기록하려면 `trx` 로거 공급자를 사용합니다. 이 스위치는 지정된 로그 파일 이름을 사용하여 테스트 결과 디렉터리에 파일을 만듭니다. `LogFileName`이 제공되지 않으면 테스트 결과를 포함할 고유한 파일 이름이 만들어집니다.

    ```console
    /logger:trx [;LogFileName=<Defaults to unique file name>]
    ```

- **`--Parallel`**

  테스트를 병렬로 실행합니다. 기본적으로 컴퓨터의 사용 가능한 모든 코어를 사용할 수 있습니다. *runsettings* 파일에서 `RunConfiguration`. 노드 아래의 `MaxCpuCount` 속성을 설정하여 코어의 개수를 명시적으로 지정합니다.

- **`--ParentProcessId <PROCESS_ID>`**

  현재 프로세스를 시작하는 일을 담당하는 부모 프로세스의 프로세스 ID입니다.

- **`--Platform <PLATFORM_TYPE>`**

  테스트를 실행하는 데 사용되는 대상 플랫폼 아키텍처입니다. 유효한 값은 `x86`, `x64` 및 `ARM`입니다.

- **`--Port <PORT>`**

  소켓 연결 및 이벤트 메시지를 받기 위한 포트를 지정합니다.

- **`--ResultsDirectory:<PATH>`**

  테스트 결과 디렉터리가 존재하지 않는 경우 지정된 경로에 생성됩니다.

- **`--Settings <SETTINGS_FILE>`**

  테스트를 실행할 때 사용할 설정입니다.

- **`--TestAdapterPath <PATH>`**

  테스트 실행에서 지정된 경로(있는 경우)의 사용자 지정 테스트 어댑터를 사용합니다.

- **`--TestCaseFilter <EXPRESSION>`**

  지정된 식과 일치하는 테스트를 실행합니다. `<EXPRESSION>`은 `<property>Operator<value>[|&<EXPRESSION>]` 형식입니다. 여기서 Operator는 `=`, `!=` 또는 `~` 중 하나입니다. 연산자 `~`는 '포함' 의미 체계를 가지며 `DisplayName`과 같은 문자열 속성에 적용됩니다. 괄호 `()`를 사용하여 하위 식을 그룹화합니다. 자세한 내용은 [TestCase 필터](https://github.com/Microsoft/vstest-docs/blob/master/docs/filter.md)를 참조하세요.

- **`--Tests <TEST_NAMES>`**

  제공된 값과 같은 이름의 테스트를 실행합니다. 여러 값을 쉼표로 구분합니다.

- **`-?|--Help`**

  명령에 대한 간단한 도움말을 출력합니다.

- **`@<file>`**

  추가 옵션에 대한 지시 파일을 읽습니다.

- **`args`**

  어댑터에 전달될 추가 인수를 지정합니다. 인수는 `<n>=<v>` 형식의 이름-값 쌍으로 지정됩니다. 여기서 `<n>`은 인수 이름이고 `<v>`는 인수 값입니다. 여러 개의 인수를 구분하려면 공백을 사용합니다.

## <a name="examples"></a>예

*mytestproject.dll*에서 테스트를 실행합니다.

```dotnetcli
dotnet vstest mytestproject.dll
```

*mytestproject.dll*에서 테스트를 실행하고 사용자 지정 이름을 갖는 사용자 지정 폴더로 내보냅니다.

```dotnetcli
dotnet vstest mytestproject.dll --logger:"trx;LogFileName=custom_file_name.trx" --ResultsDirectory:custom/file/path
```

*mytestproject.dll* 및 *myothertestproject.exe*에서 테스트를 실행합니다.

```dotnetcli
dotnet vstest mytestproject.dll myothertestproject.exe
```

`TestMethod1` 테스트를 실행합니다.

```dotnetcli
dotnet vstest /Tests:TestMethod1
```

`TestMethod1` 및 `TestMethod2` 테스트를 실행합니다.

```dotnetcli
dotnet vstest /Tests:TestMethod1,TestMethod2
```

## <a name="see-also"></a>참조

- [VSTest.Console.exe 명령줄 옵션](/visualstudio/test/vstest-console-options)
