# RxJS-Season3

RxJS presentation Season 3

## RxJS 技术分享第三季

1. 高阶操作符
2. 辅助类操作符
3. 过滤操作符
4. [代码地址](https://stackblitz.com/edit/rxjs-season3?file=index.ts)
5. [玩具地址1](https://stackblitz.com/edit/rxjs-reactive-scroll?file=index.ts)
6. [玩具地址2](https://stackblitz.com/edit/rxjs-fake-search-engine?file=index.ts)
7. [玩具地址3](https://stackblitz.com/edit/rxjs-save-note?file=index.ts)

### 高阶操作符

1. 高阶Observable 的概念
2. `concatAll` 操作符
3. `mergeAll` 操作符
4. `zipAll` 操作符
5. `combineAll` 操作符

### 辅助类操作符

1. `count` 统计数据流中产生的所有数据个数
2. `max/min` 获得数据流中最大或最小的数据
3. `reduce` 对数据流中所有数据进行规约操作
4. `every` 判断是否所有数据满足某个判定条件
5. `find` 和 findIndex 找到第一个满足判定条件的数据
6. `isEmpty` 判断一个数据流是否不包含任何数据
7. `defaultIfEmpty` 如果一个数据流为空就默认产生一个指定数据

### 过滤数据流

1. `filter` 过滤掉不满足判定条件的数据
2. `first` 获得满足判定条件的第一个数据
3. `last` 获得满足判定条件的最后一个数据
4. `take` 从数据流中选择去最先出现的若干数据
5. `takeLast` 从数据流中选取最后出现的若干数据
6. `takeWhile/takeUntil` 从数据流种选取数据直到某种情况发生
7. `skip` 从数据流中忽略最先出现的若干数据
8. `skipWhile/skipUntil` 从数据流中的忽略数据直到某种情况发生

### future

1. `throttleTime/debounceTime/auditTime` 基于时间的数据流量筛选
2. `throttle/debounce/audit` 基于数据内容的数据流量筛选
3. `sample/sampleTime` 基于采样方式的数据流量筛选
4. `distinct` 删除重复的数据
5. `distinctUntilChanged/distinctUntilKeyChanged` 删除重复的连续数据
6. `ignoreElements` 忽略数据流中的所有数据
7. `elementAt` 只选取指定出现位置的数据
8. `single` 判断是否只有一个数据满足判定条件

### exhaust 的使用场景


### throttleTime 配合 debounceTime 参与进用户笔记及时存储

```typescript
import { of, fromEvent } from 'rxjs'; 
import { map,throttleTime,debounceTime,tap,merge,distinctUntilChanged,switchMap } from 'rxjs/operators';

console.clear();
const source$ = fromEvent(document.getElementById('type-ahead'),'keyup');

const throttleSource$ = source$.pipe(
  throttleTime(3000),
  map((e:any) => e.target.value)
);

const debounceSouce$ = source$.pipe(
  debounceTime(500),
  map((e:any) => e.target.value)
);

const fakeRequest = key => of(key).pipe(
  tap(() => console.log(`查询 ${key} 的请求发送于 ${new Date()}`))
);

const subscription = debounceSouce$.pipe(
  merge(throttleSource$),
  distinctUntilChanged(),
  switchMap(fakeRequest)
).subscribe()
```

尝试使用 marble 图进行分析，理解并优化该功能

### exhaustMap 操作符的使用场景

存在网路延迟的情况下，用户过多次点击存储或其他功能性按钮，减少不必要的 http 请求，优化性能

```typescript
fromEvent(this.saveButton.nativeElement, 'click')
.pipe(
    exhaustMap(() => this.saveCourse(this.form.value))
)
.subscribe();
```