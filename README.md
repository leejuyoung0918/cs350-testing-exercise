# CS350 · 테스트 작성과 자동 생성 비교

작은 `Car` 상태 모델에 수동 단위 테스트를 작성하고, coverage 측정과 Pynguin 자동 생성 테스트를 비교하는 소프트웨어 테스팅 실습입니다.

## 문제와 구현

속도 변경은 현재 속도뿐 아니라 이동 거리와 누적 시간도 바꿉니다. 따라서 반환값 하나를 확인하는 것만으로는 상태 전이를 충분히 검증할 수 없습니다.

- [car.py](car.py): 속도 변경, 0 이하 감속 처리, 이동 거리·시간 누적, 평균 속도
- [test_car.py](test_car.py): 초기 상태·속도 변경·조회에 대한 수동 테스트
- [pynguin-results](pynguin-results): 저장된 자동 생성 결과

## 실행

Python 가상환경에서 실행합니다. 전체 실습 의존성에는 Pynguin 0.40.0이 포함됩니다.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m pytest test_car.py
python -m coverage run --branch -m pytest test_car.py
python -m coverage report -m
```

자동 생성은 별도 출력 디렉터리에 실행합니다.

```bash
pynguin --project-path ./ --output-path pynguin-tests --module-name car -v
```

생성된 테스트는 읽고 검토한 뒤 수동 테스트와 비교합니다. 생성 도구의 설치·실행 환경과 실제 coverage 수치를 함께 기록해야 재현 가능한 결과가 됩니다.

## 결과 해석과 학습

현재 수동 테스트는 초기값과 가속·조회 동작에 집중되어 있습니다. 강한 감속, `step()`의 거리·시간 누적, 이동 전 `average_speed()`의 예외는 추가 검증이 필요한 부분입니다. 높은 line coverage가 모든 상태 전이와 의미 있는 assertion을 보장하지 않는다는 관점에서 branch coverage와 테스트 내용을 함께 확인하는 실습입니다.

이 README 정리에서는 실행 명령에 섞여 있던 비표준 대시를 수정했습니다. 현재 저장소에 CI가 구성되어 있거나 자동 생성만으로 버그 부재를 입증했다고 주장하지 않습니다.
