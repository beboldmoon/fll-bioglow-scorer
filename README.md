# FLL BIOGLOW 자가 채점표

**사이트 주소**
- 채점표: https://beboldmoon.github.io/fll-bioglow-scorer/
- 비밀번호 만들기: https://beboldmoon.github.io/fll-bioglow-scorer/register.html
- 관리자: https://beboldmoon.github.io/fll-bioglow-scorer/admin.html

2026-27 FIRST LEGO League Challenge (BIOGLOW) 로봇 게임 점수를 학생이 직접 체크하고 Supabase에 저장하는 정적 사이트입니다. GitHub Pages에 그대로 올리면 동작합니다.

| 페이지 | 용도 |
|---|---|
| `index.html` | 학생용 자가 채점표. 체크하면 자동 합산, 시도별 수행 시간 기록, 비밀번호로 저장·불러오기 |
| `record.html` | 경기 1건의 상세 페이지 (임무별 점수·달성 항목·시도 기록). 채점표의 내 기록에서 연결 |
| `register.html` | 학생이 자기 비밀번호를 만들거나 바꾸는 페이지 |
| `admin.html` | 관리자 대시보드. 학생별 점수·임무별 달성률·기록 상세·CSV 내려받기·비밀번호 초기화 |

## 구조
- Supabase 프로젝트 `FLL-BIOGLOW` (서울)
- 테이블: `fll_scores`(점수 기록), `fll_students`(학생+비밀번호 해시), `fll_settings`(관리자 코드 해시)
- 브라우저는 테이블에 직접 접근할 수 없고 RPC 함수로만 동작합니다: `fll_register`, `fll_change_pin`, `fll_save_score`, `fll_my_scores`, `fll_admin_scores`, `fll_admin_students`, `fll_admin_reset_pin`
- 비밀번호와 관리자 코드는 bcrypt로 해시되어 저장됩니다.

## 관리자 코드 바꾸기
Supabase SQL Editor에서:
```sql
update fll_settings set value = extensions.crypt('새코드', extensions.gen_salt('bf')) where key = 'admin_code';
```
