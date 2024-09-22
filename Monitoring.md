# Monitoring

## Graphite
- 시간 기반의 측정치(Metric) 데이터를 저장하는 저장소입니다.
  - Python 으로 작성되어 있습니다.

### Graphite Web
- 저장된 측정치 데이터와 시간을 그래프로 표기해주는 웹 어플리케이션
- Carbon-Cache : 네트워크에서 측정치 데이터를 수집하고 디스크에 저장하는 데몬

### Grafana
- Graphite 대시보드
  - 웹 어플리케이션이며 Graphite Web 보다 UI나 화면 구성이 좀 깔끔합니다.
  - Graphite Web 에서 데이터를 가져오기 때문에 Graphite Web 이 필요하다.

### Diamond
- 서버 자원의 측정치 데이터를 수집하여 Carbon-Cache 로 전송하는 도구
  - CPU, 메모리, 네트워크 사용량, 디스크 I/O 등의 데이터를 수집합니다.