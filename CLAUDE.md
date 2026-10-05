# sepia 일괄 작업 저장소

이 저장소는 [sepia](https://github.com/Nanako0129/sepia) 스킬로 글을 진단·수정하는 작업 공간이다.
스킬은 `.claude/skills/`에 들어 있다 (sepia v0.12.2, 원본 커밋 `06a5233`, MIT — `.claude/skills/sepia/LICENSE`).

## 폴더

- `sepia/inbox/` — 처리할 원문. 파일 하나가 글 하나 (`.md` 또는 `.txt`).
- `sepia/out/` — 결과. 원문 `foo.md`마다 아래 두 파일을 만든다.
  - `foo.review.md` — sepia review 보고서 (`SEPIA REVIEW` 형식 그대로)
  - `foo.md` — 수정본 (refactor 또는 recreate 결과)

## 일괄 처리 규칙

"inbox 처리해줘" 같은 요청을 받으면:

1. `sepia/inbox/`에서 `sepia/out/`에 `.review.md`가 아직 없는 파일만 고른다. 이미 처리된 파일은 건너뛴다.
2. 파일마다 sepia 스킬(`.claude/skills/sepia/SKILL.md`)을 처음부터 읽고 라우팅을 따른다. 글 종류는 내용을 보고 판단하되, 판단이 애매하면 보고서의 첫 줄에 고른 근거를 적는다.
3. 기본 작업은 refactor다 (보고서 → 수정). review 보고서의 Verdict가 `recreate`이면 recreate로 다시 쓴다. 사용자가 다른 작업(review만, recreate 등)을 지정하면 그것을 따른다.
4. 무인 모드로 동작한다: 중간에 묻지 않고, 사람이 결정해야 하는 항목은 보고서의 `Deferred:` 줄에 남긴다. 원문에 없는 수치·이름·날짜는 지어내지 말고 `TODO`로 남긴다.
5. 원문 파일(`sepia/inbox/`)은 수정하지 않는다.
6. 모두 끝나면 처리한 파일 목록과 각 Verdict를 한 줄씩 요약하고, 결과를 커밋·푸시한다.

## 참고

- sepia는 영어·중국어 기준으로 만들어졌다. 한국어 글에서는 금지어 목록을 의미가 같은 한국어 표현으로 옮겨 적용하고, 그렇게 했다는 것을 보고서의 `Style scan:` 줄에 밝힌다.
- 맞춤법·띄어쓰기 교정은 sepia의 범위가 아니다. 요청받지 않으면 하지 않는다.
- sepia를 새 버전으로 올릴 때는 원본 저장소의 `skills/` 디렉터리를 `.claude/skills/`에 통째로 덮어쓰고, 위 버전·커밋 표기를 갱신한다.
