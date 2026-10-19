# 원큐 MCP

AI 에이전트에 프로젝트 조회, 실행, 브라우저 검증, 요구사항 추적과 출시 판정을 제공하는 로컬 MCP 서버입니다. 코드 작성은 연결된 AI 에이전트가 담당합니다.

이 저장소에는 원큐 MCP 서버·테스트·설정 도구만 포함합니다. SEVEN 등 예제 앱과 호스팅 소스는 포함하지 않습니다.

## 설치와 검증

Node.js 24 이상이 필요합니다. 브라우저 통합 테스트는 Microsoft Edge를 사용합니다.

```sh
npm ci
npm test
npm run client-config -- --write
```

생성된 mcp-client.local.json의 command와 args를 MCP 호스트에 등록하세요. 직접 실행 명령은 node src/index.mjs입니다. 표준 출력은 MCP 프로토콜에 사용하므로 호스트에는 npm start 대신 node 실행 파일과 src/index.mjs의 절대 경로를 등록하세요.

## 프로젝트 설정

projects.json은 기본적으로 이 MCP 저장소 자체를 등록합니다. 다른 프로젝트를 검증하려면 root를 실제 프로젝트 경로로 변경하고 sources와 checks를 그 프로젝트에 맞게 설정하세요. root의 상대 경로 기준은 설정 파일이 위치한 디렉터리입니다.

별도 설정은 node src/index.mjs --config /absolute/path/projects.json으로 지정합니다. 미리보기는 preview에 서버 모듈과 내보낸 함수, healthPath를 등록해야 사용할 수 있습니다. 기본 설정에는 미리보기가 없습니다.

## 사용 흐름

1. inspect_project로 프로젝트와 검증 명령을 확인합니다.
2. onequeue://guide 또는 build_app 프롬프트를 통해 제작 절차를 읽습니다.
3. save_build_brief로 요구사항과 수락 조건을 저장합니다.
4. 호스트에서 코드를 구현하고 run_checks와 브라우저 검증으로 근거를 수집합니다.
5. get_project_status로 로컬 검증·출시 준비·실제 배포 검증을 구분해 확인합니다.

배포 실행은 호스트의 전문 도구가 담당합니다. 원큐는 검증 근거와 출시 조건을 관리합니다. 소스가 바뀌면 기존 근거를 재검토해야 합니다.

상태 조회: npm run status -- --project onequeue. 전체 근거는 --full을 추가하세요. 동일 프로젝트를 소유한 MCP 서버를 중복 실행하거나 잠금 파일을 강제로 삭제하지 마세요.
