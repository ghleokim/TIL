# secure-coding-general

2020-01-07

## Secure Coding

보안사고의 75% 이상은 애플리케이션의 취약성이 원인이다.

### 보안사고사례

- 농협 전산망 해킹 공격

- SK 컴즈 고객정보 유출 [관련기사(bloter)](http://www.bloter.net/archives/71586)

- KT 고객정보 유출 [관련기사(bloter)](http://www.bloter.net/archives/183968)

- 웹호스팅업체 나야나 - 랜섬웨어

- 여기어때(위드이노베이션) - SQL인젝션

  로그인 페이지에서 취약점 발견. 세션 조작만으로 관리자 로그인 가능.

- (참고) 유튜브 조회수 - 오버플로우 https://arstechnica.com/information-technology/2014/12/gangnam-style-overflows-int_max-forces-youtube-to-go-64-bit/
  → 오버플로우로 인한 보안사고는 이더리움 기반 가상화폐(SMT)에서 일어난 적 있다.

### 보안약점 vs 보안취약점

- 보안약점 ⊃ 보안취약점
- **보안약점(Weakness)** 보안사고에 악용될 수 있는 **보안취약점의 근본 원인**
- **보안취약점(Vulnerabilities)** SW실행시 여러 개의 보안약점 중 **실제 사고의 원인이 되는 해당 보안약점**
- [CVE](cve.mitre.org), [CWE](cwe.mitre.org), [SANS](sans.org), [OWASP](owasp.org) 등의 단체에서 보안취약점, 보안약점을 찾고, 알리는 활동을 하고 있다.
- [CWE Top 25 Most Dangerous Software Errors 2019](https://cwe.mitre.org/top25/archive/2019/2019_cwe_top25.html)
  CWE가 선정하여 발표한 가장 위험한 SW 에러의 목록.
  2011년에도 CWE와 SANS Institute가 공동으로 25개를 발표한 적이 있다. 단 그 방법에 중요한 차이점이 있는데, 2011년에는 설문조사를 만들어 개발자, 보안전문가, 연구원들과 벤더로부터 결과를 받아 선정하였다면, 2019년에는 미 국립표준기술연구소(National Institute of Standards and Technology, NIST)의 미국 취약점DB(National Vulnerability Database)를 기반으로 선정하였다는 차이가 있다.
- [OWASP Top 10 2017](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_2017_Project)