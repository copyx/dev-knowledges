# [YAML](https://yaml.org/)

YAML Ain't Markup Language™ 의 약어.

GNU 같네ㅋㅋㅋ 공식사이트 컨텐츠들도 YAML 문법으로 작성되어있음ㅋㅋ

## YAML 설계 목표

- YAML should be easily readable by humans.
- YAML data should be portable between programming languages.
- YAML should match the native data structures of dynamic languages.
- YAML should have a consistent model to support generic tools.
- YAML should support one-pass processing.
- YAML should be expressive and extensible.
- YAML should be easy to implement and use.

## 문법

```yaml
# 주석 가능함

a: b # 가능
a : b # 가능
# a:b 불가능. : 과 값 사이에는 공백이 있어야함.
json: {'a':b} # JSON 스타일에서는 키가 따옴표로 감싸져있다는 조건 하에 가능.
a: # 가능. 암묵적으로 null이 들어감.
? key # key라는 이름의 키를 선언. 값은 암묵적으로 null이 들어감.
: value # value 라는 값을 할당. 결과적으로 key: value 처럼 평가됨.

한글: 세종대왕 # 유니코드 지원
number: 3 # 숫자 인식
PI: 3.141592 # 소수 지원
float: 123.45678e+05 # 부동소수점 지원
not number: "3" # 따옴표로 감싸면 무조건 문자열

# .을 붙여 쓰는 특수한 숫자
+infinity: .inf # 양의 무한대. INF, Inf 모두 인식
-infinity: -.inf # 음의 무한대.
not a number: .NaN # NaN, nan, NAN 모두 인식
quarter: .25 # . 뒤에 숫자가 오면 소수점으로 인식

bool: true
Bool: False
BOOL: TRUE # Boolean 타입 지원. 일반적인 대소문자 표현 인식. TruE는 문자열로 인식
# yes/no, on/off 등도 가능하지만 1.2부터 지원하지 않음.

null value: null # null 사용 가능
null shorthand: ~

date: 2022-01-29 # ISO 시간 포맷 사용 가능
date: 2022-01-29T11:08:32

a apple: test # 문자열에 따옴표는 선택사항. 프로퍼티 이름에 공백이 들어가도 상관 없음.
quotes: 'test'
double quotes: "test\n" # 작은 따옴표, 큰 따옴표 모두 지원. 따옴표 안에서 이스케이프 문자 사용 가능.

long sentence: > # 개행, 빈 줄 없이 한 문장으로 인식. 끝에 공백 포함. >+의 단축 표현
  첫 번째 줄

  두 번째 줄
# 첫 번째 줄 두 번째 줄

sentence with new line: | # 개행, 빈 줄 모두 인식함. |+의 단축 표현
  첫 번째 줄

  두 번째 줄
# 첫 번째 줄
#
# 두 번째 줄

# >-, |-는 마지막 개행을 포함하지 않음.

# 배열은 - 혹은 []로 표현
array:
  - a
  - 1
  - 2022-01-22
single line json style array: [a, 1, 2022-01-22]
multi line json style array: [
  a,
  1,
  2022-01-22
]
deep array:
  - - - - - value # 한 번에 여러번 중첩 가능.

# 객체
object:
  prop1: value1
  prop2: value2
  inner object:
    prop3: value3
json style object: {
  prop1: value1,
  prop2: value2 # 마지막의 ,(trailing comma)는 선택사항
}

# Anchor. 값을 재사용하기 위한 문법
some string: &anchor long long string
reuse anchor: *anchor # long long string 으로 평가됨.

some object: &object-anc
  key1: value1
  key2: value2
reuse object: *object-anc
extended object:
  <<: *object-anc # some object를 기반으로 새 오브젝트 생성
  key1: new value # 새 내용으로 덮어 씌움.
  key3: value3 # 새 필드 추가

# 배열도 앵커 사용 가능. 하지만 <<:는 지원하지 않음.

# YAML은 한 파일에 여러 문서 작성 가능. ---(대시 3개)로 새로운 스트림 시작
---

```
