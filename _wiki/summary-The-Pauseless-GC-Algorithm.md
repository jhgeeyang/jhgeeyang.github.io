---
layout  : wiki
title   : (요약) The Pauseless GC Algorithm, 2005
summary : Pauseless GC 알고리즘 논문 요약
date    : 2019-10-15 17:55:05 +0900
updated : 2019-10-16 12:05:42 +0900
tag     : java gc
toc     : true
public  : true
parent  : summary
latex   : false
---
* TOC
{:toc}

* [Teh Pauseless GC Algorithm by Cliff Click, Gil Tene, Michael Wolf. 2005][pdf], [다른 링크][pdf2]
* "역: ~"은 나의 의견이다.

# 요약

## ABSTRACT

**개요**

응답 시간이 중요한 최근의 애플리케이션은 가비지 수집 힙의 크기를 제한하곤 한다.
하지만 힙 사이즈를 늘리게 되면 일시 정지 시간이 길어지는 문제가 생긴다.

(역: 힙 사이즈가 크면 일시 정지 시간도 길어지는 것은 GC의 일반 원칙.)

Azul Systems는 GC 연구를 위해 커스터마이즈된 시스템(CPU, 칩, 보드, OS)을 만들었다고 한다.

그리고 그 커스텀 CPU에는 읽기 장벽(read barrier) 명령(instruction)이 들어가 있고,
읽기 장벽 기능을 쓰면 높은 동시성(concurrent)을 가진 병렬 및 압축(compacting) GC 알고리즘을 수행할 수 있다고 한다.

Pauseless GC 알고리즘은 다음을 위해 설계됐다.

* 모든 GC 단계에서 중단없는 애플리케이션 실행
* 모든 GC 단계에서 일관된 뮤테이터 처리량(mutator throughput) 확보

그리고 Pauseless GC 알고리즘은 다음의 특징을 갖는다.

* Pauseless GC는 GC 페이즈를 완료하기 위해 절대로 "rush" 하지 않는다.
* 어떤 페이즈도 뮤테이터에 일거리를 과도하게 주거나, 작업을 빨리 완료시키기 위한 경합을 벌이지 않는다.
* 뮤테이터 오버 헤드를 제한하는 "자가 치유(self-healing)" 기능이 있다.

## Keywords

**핵심 키워드**

Read barriers, memory management, garbage collection, concurrent GC, Java, custom hardware

## 1. INTRODUCTION

**도입**

엔터프라이즈 애플리케이션을 위해 다음과 같은 조건을 가진 GC가 필요하다.

* 인간의 반응 속도 한계인 10 ~ 100ms 만큼의 짧은 일시 정지 시간.
* 100개 이상의 동시 뮤테이터 스레드를 처리할 수 있는 높은 수준의 동시성.
* 작업에 있어 일관성이 있고 예측 가능할 것.

수많은 현대적인 GC들은 힙 영역들을 넘나들며 참조를 추적하기 위해 쓰기 장벽(write barrier)를 사용한다. generational 한 GC나 region 기반의 GC가 이 기법을 토대로 한다.

그러나 읽기 장벽(read barrier)은 뮤테이터 비용이 높아, 프로덕션 시스템에서는 거의 사용되지 않는다.

Azul Systems 에서는 이를 테스트하기 위해 읽기 장벽 명령이 있는 커스텀 CPU가 있는 커스텀 하드웨어 시스템을 구축했고,
Pauseless GC가 간단하고 효율적이며 Stop-The-World 단계가 없음을 검증한다.

## 2. RELATED WORK

**관련된 연구 작업들**

* GC 일시 정지는 초기의 동시성 수집기 연구의 계기가 되었다.
* GC를 위한 하드웨어와 관련된 이야기들.
    * 하드웨어 차원에서 읽기 장벽을 구현하면 비용을 크게 아낄 수 있다.

증분 수집기(incremental collector)와 관련된 이야기

* 증분 수집기(incremental collector)는 새로운 아이디어가 아니다.
* 증분 수집기는 일시 정지 시간을 줄이기 취해 수집 작업을 분산시켜, GC 작업이 뮤테이터 작업과 혼합되게 한다.

Metronome과 Pauseless의 비교: Pauseless는 완전히 병렬이고, 여러 CPU를 사용할 수 있고 뮤테이터 활용 능력도 더 좋다.

Pauseless의 특징

* G1 과 비슷하게 죽은 객체가 가득한 지역에 대한 수거에 중점을 둔다.
* 마크 단계에서는 SATB(Snapshot-At-The-Beginning) 스타일 대신 증분 업데이트 스타일을 사용.
    * SATB 방식은 쓰기 장벽을 필요로 하고 이 작업은 비용이 비싼 편이다.
* Pauseless 수집기는 쓰기 장벽이 필요하지 않습니다.


## 3. HARDWARE SUPPORT
### 3.1 Background

Azul Systems가 구축한 GC 연구용 커스텀 하드웨어 이야기.

### 3.2 OS-level Support

### 3.3 Hardware Read Barrier

## 4. THE PAUSELESS GC ALGORITHM

Pauseless GC 알고리즘의 세 가지 주요 단계

* Mark: 주기적으로 마크 비트를 갱신한다.
* Relocate: 마크 비트를 사용해 라이브 객체가 거의 없는 페이지를 찾아, 해당 페이지를 재배치하고 압축(compact)하고 물리적 메모리를 해제한다.
* Remap: 힙에서 재배치된 모든 포인터를 업데이트한다.

그리고 각각의 단계는 모두 병렬로 작동한다.

* 특정 단계를 빨리 완료하기 위해 무리하는 일이 없다.
* Relocation이 계속 실행되므로 언제든지 프리 메모리를 확보할 수 있다.
* 다른 증분 업데이트 알고리즘과는 달리 리마크(re-Mark), 최종 마크(final-Mark) 단계가 없다.
* 모든 단계가 병렬이므로 GC 스레드를 더 추가하여, 여러 개의 뮤테이터 스레드를 사용할 수 있다.
    * 물론 극단적인 상황에서는 GC 수행 때문에 뮤테이터가 돌지 못하는 일도 있을 수 있다.
* 각 단계는 "self-healing"을 한다.
    * 동일한 참조가 다른 트랩을 발동시키지 않도록 한다.

Pauseless 알고리즘은 Stop-The-World 가 없다.

* 물론 이론상으로만 없고, 기존 JVM 작업을 쉽게 하기 위해 구현에는 Stop-The-World가 들어갈 수 있다.


### 4.1 Mark Phase

* 모든 라이브 오브젝트를 표시하고 태그를 지정하여 죽은 오브젝트와 구별할 수 있게 한다.
* 읽기 장벽으로 기능이 보강된 병렬 및 동시 증분 업데이트 (SATB 아님) 마킹 알고리즘을 쓴다.
* 각 참조에는 NMT 비트가 예상 값으로 설정되어 있다.
* 마크 단계는 또한 1 백만 페이지당 살아있는 객체의 합계를 수집.
    * 이 합계는 페이지에서 실시간 데이터의 보수적인 추정치를 제공하며 재배치(Relocation) 단계에서 사용하게 된다.

마크 단계의 기본 개념

* 루트 세트(정적 전역 변수나 뮤테이터 스택)에서 출발.
    * 도달 가능한 객체를 표시하고, 이 때 NMT 비트도 함께 셋팅한다.
    * 객체 내부에 있는 모든 참조를 재귀적으로 표시한다.

### 4.2 Relocate Phase

* 객체를 재배치하고 페이지를 회수하는 단계.
* 죽은 객체가 가득한 페이지에서 살아있는 객체를 다른 페이지로 옮긴다.
* 지정된 Sparseness threshold 값을 초과하는 페이지 집합부터 잡고 시작한다.
    * 각 페이지는 뮤테이터 엑세스로부터 보호 처리가 된다.
    * 보호 처리가 된 다음, 라이브 객체를 복사해 옮긴다.
    * 재배치된 라이브 객체의 위치 정보는 페이지 외부에 유지된다.
* 만약 뮤테이터가 보호 처리된 페이지에 있는 참조를 읽으면 읽기 장벽(read-barrier)에 의해 GC 트랩이 발동한다.
* GC 트랩 핸들러는 보호된 페이지에 있는 오래된 참조를, 올바르게 전달된 참조로 바꾼다.

페이지 내용이 모두 다른 페이지로 재배치되면, 물리적 메모리(physical memory)를 해제한다.

* 해제된 물리적 메모리는 OS에 의해 재활용되며, 새로운 할당에 즉시 사용될 수 있다.
* 해제된 페이지에 대한 참조가 있는 가상 메모리는 해제하지 않는데, 이건 Remap 단계에서 처리할 일이다.

```ascii-art
                          Remap
│   Mark   │ Relocate  │<────────>│
│<────────>│<----------│─────────>│
                       │          │
                       │  Mark    │  Relocate
                       │<────────>│<---------- ...

Figure 1: The Complete GC Cycle
```

* 위의 그림과 같이 Relocate 단계는 메모리의 여유 공간을 지속적으로 해제한다.
* 뮤테이터 할당은 여유 공간이 없어 할당을 못 하는 일이 거의 없게 된다.
* Relocate 단계는 단독으로 실행될 때도 있고, 다음 Mark 단계와 같이 실행될 때도 있다.


### 4.3 Remap Phase

* Remap 단계에서 GC 스레드는 힙의 모든 참조에 대해 읽기 장벽을 실행하며, 객체 그래프를 가로지른다(traverse).
* 보호된 페이지를 참조하는 레퍼런스가 있다면 새로운 주소로 포워딩해준다.
* Remap 단계가 완료되면 이전의 Relocate 단계에서 보호된 페이지를 참조할 수 없게 된다. 그리고 이 시점에서 해당 페이지의 가상 메모리가 해제된다.
* Remap과 Mark 단계는 모든 라이브 객체를 터치하므로 함께 작업한다.

* Remap 단계는 Relocate 단계의 뒷부분과 동시에 시작된다. (Figure 1을 보자)
* Relocation 단계에서 생겨나는 부실한 포인터들은 Remap 단계가 끝나야만 사라진다.

## 5. MARK PHASE

* Mark 단계는 마킹 작업목록과 같은 내부 데이터 구조를 초기화하고, 이 단계의 마크 비트(mark-bits)를 지우면서 시작한다.

각각의 객체는 2 개의 마크 비트를 갖는다.

* 이번 GC 주기에서 객체의 참조에 도달할 수 있는지의 여부(라이브 객체인지 여부)를 나타내는 비트.
* 이전 주기에서의 상태를 나타내는 비트.

Mark 단계는 모든 전역 참조를 표시하고 각 스레드의 루트 세트를 스캔하며 스레드 당 예상 NMT 값을 뒤집는다.

* 루트 세트: 일반적으로 CPU 레지스터와 스레드 스택의 모든 참조를 포함.
* 실행중인 스레드는 자신의 루트 세트를 마킹하는 방식으로 이 작업에 참여한다.
* 차단된 스레드는 Mark-phase 스레드에 의해 병렬로 마크된다.
    * 이 마크가 체크 포인트가 된다.
    * 루트 세트를 표시하고, NMT가 뒤집히면 작업을 계속 진행할 수 있다.
    * Mark 단계는 모든 스레드가 체크포인트를 통과 해야만 다음 작업을 할 수 있다.

루트 세트가 모두 표시되면 병렬 동시 마킹 단계(parallel and concurrent marking phase)가 진행된다.

* 라이브 참조는 작업 목록에서 가져온다.
* 라이브 참조의 타겟 객체는 라이브 마킹을 얻게 되며, 그 객체의 내부에 있는 참조는 재귀적으로 마킹 처리된다.
* 마커는 NMT 비트를 무시한다. NMT 비트는 뮤테이터에 의해서만 사용되기 때문이다.
* 라이브 객체로 마킹된 객체의 사이즈는 해당 객체의 1M 페이지의 라이브 데이터 양에 추가된다.
* 모든 라이브 오브젝트에 마킹이 되면 단계가 끝난다.

동시 뮤테이터에 의해 만들어진 새로운 객체는 GC 사이클에서 재배치 되지 않는 페이지에 할당된다.


### 5.1 The NMT Bit

증분 업데이트 마커(incremental update marker)를 만들 때의 어려운 점

* 뮤테이터가 라이브 객체를 "숨겨서" 마킹 스레드가 발견하지 못하게 되는 경우가 있다.
    * 뮤테이터가 마크가 없는 참조를 레퍼런스에서 읽고, 메모리에서 지우는(clear)할 때 발생.
    * 객체의 참조가 레지스터에 있으므로 객체가 아직 살아있는 것인데도, 마크를 얻지 못한다.

해결 방법 1: 이미 마킹된 지역에 문제의 객체 보관하기

* 이런 객체는 힙에서 이미 마킹된 지역(region)에 보관할 수도 있다.
    * 이 방법은 마킹 단계가 끝날 때 또다른 Stop-The-World 일시 중지가 필요할 수 있다.
    * 이 두 번째 STW 동안 마킹 스레드는 힙의 루트 세트 및 수정된 부분을 다시 돌면서 발견된 새로운 참조에 마킹을 해야 한다.

해결 방법 2: SATB

* SATB(Snapshot-At-The-Beginning) 방법을 쓰는 GC 알고리즘도 있다.
    * 비싸다는 단점이 있다.

해결 방법 3: NMT 트랩 사용

* NMT: Not-Marked-Through 비트를 사용하는 방법.
* 참조는 64비트를 사용하지만, 하드웨어는 $$2^64$$보다 더 작은 가상 주소 공간을 구현하므로 비트에 여유가 있다.
    * 사용하지 않는 비트를 활용한다.
* NMT 비트가 올바르게 설정되면 읽기 장벽 비용이 발생하지 않게 된다.
* 올바르게 설정된 NMT 비트를 가진 참조는 어떤 경우에도 마킹 스레드에 확실히 전달된다.

만약 뮤테이터 스레드가 잘못 설정된 NMT 비트를 가진 참조를 로드하고 읽기 장벽을 설정했다면,
아직 방문하지 않은 참조를 발견하게 된 것이다.

* 뮤테이터는 NMT 트랜 핸들러로 점프하고, NMT 트랜 핸들러는 NMT 비트를 올바르게 설정한다.
    * 그리고 참조는 마크 단계 로직으로 기록된다.
    * 그리고 나서 수정된 참조가 다시 메모리에 저장된다.
    * 참조가 메모리에서 변경되므로, 문제의 참조는 나중에 트랩을 유발하지 않게 된다.

이 방법은 핵심이 되는 아이디어이며, "self-healing"이라 부른다.

이 기법이 없다면 마커 스레드가 뮤테이터의 작업 세트에서 NMT 비트를 뒤집을 수 있을 때까지 모든 뮤테이터가 계속 NMT 트랩을 사용하게 된다. 그러나 이 방법으로 인해 각 뮤테이터는 실행될 때 자신의 워킹 셋을 뒤집기만 하면 된다.


### 5.2 The NMT Bit and The Initial Stack-Scan

### 6.1 Read-Barrier Trap Handling

### 6.2 Other Relocate Phase Actions

### 6.3 The Remap Phase

## 7. REALITY CHECK

### 7.1 At the Mark Phase Start

### 7.2 At the Mark Phase End

### 7.3 At the Relocation Phase Start

### 7.4 Relocate doesn't run during Mark/Remap

## 8. EXPERIMENTS

### 8.1 Methodology

### 8.2 Transaction Times

### 8.3 Reported Pause Times

## 9. Conclusions

* Pauseless GC 알고리즘은 대규모 다중 프로세서 시스템을 위해 설계된 완전 병렬 및 동시 알고리즘이다.
    * Stop-The-World 일시 중지가 없고, 모든 뮤테이터 스레드가 한꺼번에 중지되는 지점이 없다.
    * GC 주기 동안 언제든지 죽은 객체가 있던 공간을 확보할 수 있다.
    * 뮤테이터가 할당할 공간이 부족해서 GC를 발생시켜야 하는 단계가 없다.
    * 짧게 발생하는 "trap storm"이 있긴 한데, "self-healing" 기능이 있어 "trap storm"의 비용이 작은 편이다.

Azul의 커스텀 하드웨어에는 읽기 장벽 명령이 포함되어 있다.

* 이를 통해 Stop-The-World 없는 동시 증분 업데이트 Mark 단계가 가능하다.

현재의 작업과 미래의 작업에 대한 체크

* Pauseless는 단일 세대 알고리즘이지만, generational 변형이 있을 수 있음.
* 동시, 병렬 알고리즘에 수많은 CPU를 바탕으로 연구하였고, 각 Mark/Remap 주기마다 전체 힙을 스캔했으므로 숨겨진 비용이 있을 수 있다.



[pdf]: https://www.usenix.net/legacy/events/vee05/full_papers/p46-click.pdf
[pdf2]: https://www.researchgate.net/publication/221137840_The_pauseless_GC_algorithm
