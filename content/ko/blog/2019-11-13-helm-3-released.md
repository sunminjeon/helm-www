---
title: "Helm 3.0.0 출시!"
slug: "helm-3-released"
authorname: "Matthew Fisher"
author: "@bacongobbler"
authorlink: "https://blog.bacongobbler.com"
date: "2019-11-13"
---

헬름 팀은 헬름 3의 첫번째 안정적 릴리스를 발표하게 된 것을 자랑스럽게 생각한다.

헬름 3는 CLI 도구의 최신 주(major) 릴리스이다. 헬름 2의 성공을 기반으로, 헬름 3도 진화하는 생태계의 요구에 계속 부응하고 있다.

헬름 3의 내부 구현은 헬름 2와는 상당히 다르게 변경되었다. 가장 눈에 띄는 변화는 틸러(Tiller)가 제거된 것이다. 다만 새 릴리스에서 다른 변경사항들도 확인해두는 게 좋다. 커뮤니티의 의견과 요구사항에 따라 다양한 새로운 기능들이 추가되었다. 일부 기능은 헬름 2와 호환되지 않는 방식으로, 더 이상 사용되지 않거나 리팩토링되었다. OCI 지원을 포함하는 일부 실험적 기능도 도입되었다.

추가로, 헬름 Go SDK 도 범용적으로 리팩토링되었다. 목표는 오픈 소스로 만든 코드를 Go 커뮤니티와 폭넓게 공유하고 재사용하는 것이다. 자신의 프로젝트에 헬름을 연계하여 사용하는 다른 엔지니어들의 의견을 활발하게 수용하고 있으며, [#헬름-개발 쿠버네티스 슬랙 채널](https://slack.k8s.io/)에서도 여러분들의 의견을 듣고자 한다.

다음은 헬름 3의 몇 가지 리소스이다:

- [문서](https://helm.sh/docs/)
- [FAQ: 헬름 2에서 바뀐 점](https://helm.sh/docs/faq/#changes-since-helm-2)
- [헬름 설치](https://helm.sh/docs/intro/install/)
- [헬름 2에서 헬름 3로의 마이그레이션에 대한 문서](https://helm.sh/docs/topics/v2_v3_migration/)
- [헬름 2에서 헬름 3로 마이그레이션하는데 도움이 되는 플러그인](https://github.com/helm/helm-2to3)
- [#헬름-사용자 쿠버네티스 슬랙 채널](https://slack.k8s.io/)에서 개발자 및 참여자와 채팅
- <https://github.com/helm/helm/issues>에서 버그 신고

## 헬름이란?

헬름은 쿠버네티스 내에서 애플리케이션의 생성, 설치, 관리를 협업하는 데에 필요한 도구를 팀에 제공한다.

헬름을 통하여, 사용자는 다음과 같은 것들을 할 수 있다.

- 설치하여 사용할만한 패키징된 소프트웨어(차트) 찾기
- 자신만의 패키지를 쉽게 만들고 호스팅
- 모든 쿠버네티스 클러스터에 패키지 설치
- 클러스터에 질의하여 설치되어 실행중인 패키지 확인
- 설치된 패키지의 이력을 업데이트, 삭제, 롤백 및 조회

헬름을 사용하면 쉽게 쿠버네티스 내에서 애플리케이션을 실행할 수 있다.

## 함께 살펴보자!

쿠버네티스 클러스터가 구동 중이고 `kubectl` 이 알맞게 설정되었다면, 헬름 작업은 식은 죽 먹기이다.

헬름을 사용하면 커뮤니티에서 호스팅하는 저장소를 추가하여 새 차트를 쉽게 검색할 수 있다.

```bash
$ helm repo add nginx https://helm.nginx.com/stable
```

저장소를 추가했다면 차트를 검색할 수 있다:

```bash
$ helm search repo nginx-ingress
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
nginx/nginx-ingress     0.3.7           1.5.7           NGINX Ingress Controller
```

헬름은 `helm install` 을 사용하여 차트를 빠르게 설치할 수 있는 방법을 제공한다.

```bash
$ helm install my-ingress-controller nginx/nginx-ingress
```

`kubectl` 로 클러스터를 확인해 보면:

```bash
$ kubectl get deployments
```

구동 중인 인그레스 컨트롤러(ingress controller)가 있다! `helm uninstall my-ingress-controller` 로 간단히 제거할 수 있다.

자, 이제 몇 가지 차트를 적용해보았다. 또한 일부를 커스터마이즈해보았다. 이제 직접 만들 준비가 되었다. 헬름은 이 부분도 쉽게 해준다.

```bash
$ helm create diy
Creating diy
```

이제 `diy` 라는 새 차트가 생겼다. 해당 디렉터리로 이동하여 편집하거나 `helm template` 을 실행하여 렌더링된 출력 결과를 보거나 `helm install` 을 사용하여 설치할 수 있다.

[헬름 허브](https://hub.helm.sh/)에 업스트림으로 제출할 것인가? 주의하자! 헬름 허브에 [나만의 저장소를 추가](https://github.com/helm/hub/blob/master/Repositories.md)하기 위한 문서를 따라야 한다.

## 헬름 3에서 변경되는 사항은?

이쯤에서 이런 의문이 들지도 모르겠다.

> 헬름 2에서는 워크플로가 어떻게 변경되었나? 헬름 2로 이러한 명령을 실행하면 동일한 출력 결과가 표시될까?

헬름 2는 차트 생성, 설치, 관리를 위한 워크플로를 기술하였다. 헬름 3는 이러한 워크플로를 기반으로 진화하는 생태계의 요구사항을 충족하도록 기본 인프라를 변경하였다.

만약 사용자가 헬름 2에 익숙할 경우, 헬름 3 를 사용하면 마치 집에 있는 것처럼 편안하게 느껴질 것이다.

내부적으로 변경된 사항에 대해 자세히 알아보기 위하여 문서의 [FAQ를 확인](https://helm.sh/docs/faq/)하자. 변경 목록과 관련된 변경 사항에 대한 설명이 여기 제공된다.

## 헬름의 미래

핵심 관리자들은 헬름 3를 출시하게 되어 매우 기쁘다. 헬름의 다음 개발 단계에서는 기존 기능에 대한 안정성 및 향상을 목표로 하는 새로운 기능을 볼 수 있다. 로드맵 상의 기능은 다음과 같다:

- `helm test` 의 기능 향상
- 헬름의 OCI 연계 개선
- Go 클라이언트 라이브러리의 기능 향상

### 헬름 2 지원 계획

헬름 2.15.0 릴리스 발표에서 헬름 2의 향후 계획에 대한 세부사항을 공유했다. 이 계획에 대한 자세한 내용은 [공지 게시물](https://helm.sh/blog/2019-10-22-helm-2150-released/)에서 확인할 수 있다.

## 헬름 3와 헬름 1, 2와의 관계

2015년 11월, 첫 번째 KubeCon 에서 헬름 첫 번째 버전이 출시되었다. macOS 소프트웨어 인스톨러 [Homebrew](https://brew.sh/)에서 모델링된 헬름 1 (팀에서는 "헬름 클래식" 이라고 한다)은 개별 개발자가 쿠버네티스 리소스 패키지를 만들고 클러스터에 배포할 수 있도록 설계되었다.

몇 달 후(2016년 1월), Deis 의 코어 헬름 팀은 구글, Skippbox, Bitnami 와 협력하여 개인들에서 팀으로 방점을 옮겨 새로운 버전의 헬름을 제작하였다. 그 과정에서 여러 경험과 교훈들이 적용되었다. 그 결과 팀워크는 핵심가치가 되었고, 정교한 애플리케이션을 설치하려는 쿠버네티스 사용자들의 참여로 급성장한 커뮤니티의  요구를 충족시키기 위해 설계된 도구가 탄생하였다.

2018년 6월 헬름은 [Cloud Native Computing Foundation에 가입](https://helm.sh/blog/helm-enters-the-cncf/)했다. 헬름 3는 Microsoft, 삼성 SDS, IBM 및 Blood Orange의 구성원을 포함하는 주요 유지관리자들의 공통된 노력이다. 첫 번째 알파 릴리스 이래로, 헬름 3는 여러 시간대(time zone)에 걸쳐 커뮤니티 구성원 37명의 기여가 있었다. 그 결과물은 시시각각 변화하고 진화하며 커뮤니티의 요구를 담아낸 도구이다.

## 결론

우리는 쿠버네티스로의 진입로가 되는 도구를 만들었다. 쿠버네티스 사용자가 운영 수준의 워크로드를 더 쉽게 생성, 공유, 실행할 수 있도록 돕고자 했다.

설립 이래 500명 이상의 커뮤니티 회원이 헬름 CLI를 위한 코드에 기여해왔다. 수천 명의 커뮤니티 회원이 헬름 허브에서 차트를 활성적으로 관리한다. 수많은 활성 커뮤니티 회원들이 있다. 이는 이 프로젝트를 간단한 Deis 인스톨러를 모든 쿠버네티스 사용자를 위한 강력한 도구로 탈바꿈시킨 쿠버네티스 커뮤니티의 엄청난 노력에 의한 산물이다.

모두 감사합니다. 깃헙에서 또 봐요!

- 헬름 팀 :heart:
