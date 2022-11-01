# NestJS (이하 네스트)

> A progressive Node.js framework for building efficient, reliable and scalable server-side applications.

효율적이고, 신뢰할 수 있으며, 확장 가능한 서버사이드 애플리케이션을 만들기 위한 혁신적인 Node.js(이하 노드) 프레임워크.

## 철학

노드와 서버사이드 자바스크립트를 위한 수 많은 라이브러리, 헬퍼, 도구 등이 나왔지만 아키텍처의 핵심 문제를 효과적으로 해결하지 못함.

네스트는 테스트 가능하고, 확장 가능하고, 결합이 약하며, 쉽게 유지보수 가능한 즉시 사용할 수 있는 애플리케이션 아키텍처를 제공함. 이 아키텍쳐는 Angular(이하 앵귤러)에서 강하게 영감을 받음.

## Controller (이하 컨트롤러)

서버로 들어오는 요청을 처리하고 응답을 가공하는 역할. 인터페이스를 정의하고, 주고받을 데이터의 구조를 기술.

## Provider (이하 프로바이더)

애플리케이션이 제공하고자 하는 핵심 기능, 비즈니스 로직을 수행하는 역할. Service(이하 서비스), Repository(레포지토리), Factory(팩토리), Helper(헬퍼) 등 여러가지 형태로 구현 가능.

네스트 프로바이더의 핵심은 의존성 주입이 가능하다는 점!

## Pipe (이하 파이프)

파이프의 목적 2가지

- 입력 데이터의 형식 변환(Transformation)
- 입력 데이터의 유효성 검사(Validation) : 맞으면 Pass, 틀리면 Exception

위 두 경우 모두 컨트롤러 라우트 핸들러의 인자에서 동작함. 네스트는 파이프를 함수 호출 전에 두고, 파이프는 그 함수로 전달될 인자를 먼저 받아 처리하고, 처리된 결과가 함수로 전달됨.

미들웨어와 비슷하지만 미들웨어는 실행 컨텍스트(현재 요청이 어떤 핸들러에서 수행되고, 어떤 파라미터를 갖고 있는지)를 알지 못해 애플리케이션의 모든 컨텍스트에서 사용할 수 없음.

### 내장 파이프

`@nestjs/common` 패키지에서 제공.

- `ValidationPipe`
- `ParseIntPipe`
- `ParseFloatPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `ParseEnumPipe`
- `DefaultValuePipe`
- `ParseFilePipe`

```typescript
// 기본 파이프 사용
@Get(':id')
findHabit(@Param('id', ParseIntPipe) id: number) {
  return this.habitsService.findHabit(id);
}

// 파이프 생성 + 옵션
@Get(':id')
findHabit(@Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE })) id: number) {
  return this.habitsService.findHabit(id);
}
```

파싱이 불가능한 데이터를 전달하면 유효성 검사 오류가 발생하며 요청이 컨트롤러로 전달되지 않음.

```sh
curl localhost:3000/habits/a
# {"statusCode":400,"message":"Validation failed (numeric string is expected)","error":"Bad Request"}
```

### 커스텀 파이프

`@nestjs/common`의 `PipeTransform` 제너릭 인터페이스를 구현해 커스텀 파이프를 만들 수 있음.

```typescript
export interface PipeTransform<T = any, R = any> {
  // value: 전달받은 인자 값
  transform(value: T, metadata: ArgumentMetadata): R;
}

export interface ArgumentMetadata {
  // 어떤 데코레이터인지 구별
  readonly type: Paramtype;
  // 라우트 핸들러 메소드에 정의된 매개변수 타입. 매개변수 타입을 지정하지 않거나 바닐라 자바스크립트면 undefined.
  readonly metatype?: Type<any> | undefined;
  // @Param('id', ParseIntPipe) 처럼 데코레이터에 전달된 매개변수 이름. 비워두면 undefined.
  readonly data?: string | undefined;
}

export declare type Paramtype = "body" | "query" | "param" | "custom";
```

아래는 위 인터페이스를 이용해 Joi 라이브러리를 사용하는 커스텀 유효성 검증 파이프

```typescript
// validation.pipe.ts
import {
  ArgumentMetadata,
  BadRequestException,
  Injectable,
  PipeTransform,
} from "@nestjs/common";
import { ObjectSchema } from "joi";

@Injectable()
export class JoiValidationPipe implements PipeTransform {
  constructor(private schema: ObjectSchema) {}
  transform(value: any, metadata: ArgumentMetadata) {
    const { error } = this.schema.validate(value);
    if (error) {
      throw new BadRequestException(
        'Validation failed',
        error.details.toString(),
      );
    }
    return value;
  }
}

// habits.controller.ts
// ...
@Post()
@UsePipes(
  new JoiValidationPipe(
    Joi.object({
      title: Joi.string(),
    }),
  ),
)
create(@Body() dto: CreateHabitDto) {
  const { title } = dto;
  return this.habitsService.create(title);
}
// ...
```

`@UsePipes` 데코레이터를 이용하면 컨트롤러 혹은 메소드 범위에 파이프를 일괄 적용할 수 있음. 컨트롤러 레벨에 적용하면 해당 컨트롤러의 모든 라우트 핸들러에 적용되고, 메소드 레벨에 적용하면 해당 메소드에만 적용됨.

메소드 레벨에 적용됐을 때 해당 메소드의 매개변수가 여러개인 경우, 모든 매개변수에 일괄적용됨.

네스트에서 제공하는 `ValidationPipe`는 `class-validator` 라이브러리를 이용하는 방식. 이는 바닐라 자바스크립트에서는 작동하지 않고, 타입스크립트가 필요함.

아래는 `class-validator`를 이용해 간단하게 만들어본 `ValidationPipe`

```typescript
// $ npm i --save class-validator class-transformer

// create-habit.dto.ts
import { IsString } from "class-validator";

export class CreateHabitDto {
  @IsString()
  title: string;
}

// validation.pipe.ts
@Injectable()
export class ClassValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) {
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    const object = plainToInstance(metatype, value);
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException("Validation failed", errors.toString());
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}

// habits.controller.ts
// ...
@Post()
create(@Body(new ClassValidationPipe()) dto: CreateHabitDto) {
  const { title } = dto;
  return this.habitsService.create(title);
}
// ...
```

검증용 스키마와 DTO 클래스를 따로 관리하지 않아도 된다는 점에서 class-validator를 사용하는 방식이 코드가 훨씬 줄어들고 간편하게 느껴짐.
