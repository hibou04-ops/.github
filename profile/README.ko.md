[English](./README.md) | **한국어**

# Open-Source Software Developer (Apache-2.0)

AI/ML Verification & Software Quality Engineering

범용 소프트웨어 엔지니어링, ML 하이퍼파라미터 최적화 검증, LLM 평가 및 정적 분석용 Python 도구를 개발합니다.

## 공개 도구와 검증 계약

| 도구 | GitHub Release | 패키지 버전 / 배포 | 검증 계약 |
|---|---|---|---|
| [antemortem-cli](https://github.com/hibou04-ops/antemortem-cli) | [v0.12.4](https://github.com/hibou04-ops/antemortem-cli/releases/tag/v0.12.4) | `0.12.4` · [PyPI](https://pypi.org/project/antemortem/0.12.4/) | `file:line` 및 선택적 SHA-256 결합을 지원하는 코드 인용 검증기와 CI 게이트입니다. `fabrication_rate`와 전체 주장 대비 `effective_coverage`(`verified / total`)를 함께 검사하며, `gate`는 분류된 근거와 현재 파일로 판정을 재검산해 근거 부족과 불일치 승인 아티팩트를 차단합니다. |
| [omega-lock](https://github.com/hibou04-ops/omega-lock) | [v0.3.13](https://github.com/hibou04-ops/omega-lock/releases/tag/v0.3.13) | `0.3.13` · [PyPI](https://pypi.org/project/omega-lock/0.3.13/) | Optuna 및 ML 평가용 Fail-Closed Hold-out 감사 도구입니다. 검증 생략 시 기존 `passed=True`와 별개로 `holdout_evaluated=False`, `deployment_authorized=False`를 강제합니다. Pearson 선형 상관계수와 절대·상대 점수 하락 방어(`max_relative_degradation=0.50`)를 결합하며, 다중시도 보정은 별도로 호출하는 도우미입니다. |
| [omegaprompt](https://github.com/hibou04-ops/omegaprompt) | [v2.2.6](https://github.com/hibou04-ops/omegaprompt/releases/tag/v2.2.6) | `2.2.6` · [PyPI](https://pypi.org/project/omegaprompt/2.2.6/) | 제공자 중립 LLM 평가 및 프롬프트 보정 도구입니다. 미측정 노이즈(`PREFLIGHT_UNMEASURED_NOISE`)와 명시적으로 입력한 0을 구분합니다. 차단 결함이 없고 관측 개선량이 유한한 양수 노이즈 상한을 초과해야 `PREFLIGHT_VALIDATED`가 됩니다. 모든 사전검사 결과는 `holdout_verified=False`, `deployment_authorized=False`를 유지합니다. |
| [mini-antemortem-cli](https://github.com/hibou04-ops/mini-antemortem-cli) | [v0.11.4](https://github.com/hibou04-ops/mini-antemortem-cli/releases/tag/v0.11.4) | `0.11.4` · GitHub 소스 릴리즈; 해당 버전 PyPI 미발행 | LLM 평가 설정용 독립 정적 사전검사기입니다. 분할 ID 중복, few-shot 누출, 죽은 가중치 등 11개 위험 규칙을 검사합니다. 직접 필수 실행 의존성은 Pydantic뿐이며 `omegaprompt`는 선택적 `[bridge]`로 분리했습니다. 모델·Enum·JSON 및 검증 오류의 양방향 호환성을 테스트합니다. |
| [mini-omega-lock](https://github.com/hibou04-ops/mini-omega-lock) | [v0.8.6](https://github.com/hibou04-ops/mini-omega-lock/releases/tag/v0.8.6) | `0.8.6` · [PyPI](https://pypi.org/project/mini-omega-lock/0.8.6/) | 독립 측정 신뢰성 관측 도구입니다. `compute_empirical_noise_ceiling()`은 모집단 표준편차를 이용해 `mean(abs(delta)) + 1.96 * pstdev(abs(delta)) / sqrt(n)`을 계산합니다. 기존 `compute_minimum_detectable_effect()`는 별칭이며 검정력 기반 MDE를 뜻하지 않습니다. 직접 필수 실행 의존성은 Pydantic뿐이고 선택적 `[bridge]` 연동은 명시적 호환성 테스트를 제공합니다. |
| [antemortem-structural-lab](https://github.com/hibou04-ops/antemortem-structural-lab) | [v0.8.2](https://github.com/hibou04-ops/antemortem-structural-lab/releases/tag/v0.8.2) | `0.8.2a1` · GitHub 소스 릴리즈; 해당 버전 PyPI 미발행; 연구 원장 태그와 사전 배포 패키지 메타데이터는 별도 관리 | 계약·Oracle 기반 반례 탐색과 property-based 축소 및 C/D 호출 계측을 제공합니다. `epsilon_static_floor=0.50`은 정적 정책 C의 호출 용량을 예약합니다. 양쪽 정책이 실제 호출 워밍업을 마친 뒤 비교가 확정되고 D만의 확인된 표현형 발견 이득이 0이며 발견 효율이 엄격히 열위이면 `circuit_breaker_triggered`로 잔여 D 용량을 C에 회수할 수 있습니다. 미해결 D 근거는 `UNKNOWN`을 유지합니다. 연구 판정은 `M0_HOLD`이며 유용성과 독립 OOS 일반화는 `NOT_ESTABLISHED`를 유지합니다. |

배포 채널은 버전별로 구분합니다. 위 표의 4종은 해당 버전 PyPI 파일이 공개되어 있으며, 나머지 2종은 GitHub 소스 릴리즈입니다. CI와 회귀 테스트 통과는 검사한 소프트웨어 계약에 대한 근거이며 외부 실전 데이터에서의 유용성이나 일반화를 증명하지 않습니다.

## API 키 없는 짧은 실패 재현

각 저장소 소스 루트에서 명시된 의존성을 설치한 뒤 실행합니다. `omega-lock`은 `[p2]` extra(Optuna)가 필요합니다. API 키나 모델 서비스 없이 소규모 차단 사례를 재현하며, 의존성 설치 시간은 실행 시간에 포함하지 않습니다.

```bash
# antemortem-cli
python examples/reproduce_citation_traps.py
# omega-lock
python examples/reproduce_optuna_traps.py
```

- `antemortem-cli`: `verified=1, fabricated=0, unresolved=9`에서 커버리지 0.10을 차단하고, 변조된 저장 승인과 빈 근거를 거절합니다. 완전한 확인 근거 대조군은 통과합니다.
- `omega-lock`: No-Holdout `SKIP`의 배포 승인을 차단하고, Pearson `r=1.0`인 `[1,2,3] -> [-100,-99,-98]`, 상대 하락률 0.975인 `[0.7,0.8,0.9] -> [0.01,0.02,0.03]`을 차단하며, 동일 Hold-out 대조군은 통과합니다.

두 스크립트는 예상한 차단·통과 결과를 확인하면 정상 종료합니다. 인용 재검산은 무결성 검사이며 주장 자체의 진실성이나 출처 인증을 보장하지 않습니다. 사전검사는 독립 Hold-out 배포 승인을 대신하지 않습니다.

## 라이선스

여섯 공개 도구의 라이선스는 Apache-2.0입니다. 각 저장소의 LICENSE 및 제3자 고지를 확인하십시오.
