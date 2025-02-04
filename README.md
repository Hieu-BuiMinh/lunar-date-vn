<h1 align="center">Lunar Date</h1> <br>

<p align="center">Calendar conversion Javascript library.</p>
<p align="center">
Remake from <b><a target="_blank" href="https://www.informatik.uni-leipzig.de/~duc/amlich/calrules.html">Lunar Algorithm</a></b> by Ho Ngoc Duc written in 2004
</p>

## Features

- Convert solar calendar to lunar calendar (of Vietnam) and vice versa.
- Calculate lunar calendar information such as: Lucky hour, name of hour, month, year according to Sexagenary cycle (Can-Chi)

## Installation

### Package manager

NPM Installation

```bash
npm npm i lunar-date-vn
```

See demo with module types: **[LunarDate_Import](https://github.com/Hieu-BuiMinh/lunar-date-vn)**

```typescript showLineNumbers
import { LunarDate, SolarDate } from "lunar-date-vn";
```

If using Typescript, note the `tsconfig.json` configuration as follows:

```json showLineNumbers
{
  "compilerOptions": {
    "esModuleInterop": true,
    "moduleResolution": "node",
    "module": "ESNext" // or "CommonJS" if using CJS
  },
  "include": ["./**/*.ts"],
  "exclude": ["node_modules"]
}
```

If using `require`

```typescript showLineNumbers
const calendar = require("lunar-date-vn");
```

### CDN

If using jsDelivr

```javascript showLineNumbers
<script src="https://cdn.jsdelivr.net/..."></script>
```

## Examples

these following codes converts from solar to lunar (above) and lunar to solar (below).

> **Note** When initializing a LunarDate instance, always call the function `init()`

```typescript showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const solar_date = new SolarDate(new Date());
console.log(solar_date);
console.log(solar_date.toLunarDate());

const lunar_date = new LunarDate({ day: 10, month: 5, year: 2023 });
lunar_date.init(); // initialize lunar_date before using
console.log(lunar_date.toSolarDate());

// SolarDate {
//   day: 19,
//   month: 6,
//   year: 2023,
//   name: 'solar_calendar',
//   jd: 2460115,
//   leap_year: false
// }

// LunarDate {
//   day: 2,
//   month: 5,
//   year: 2023,
//   name: 'lunar_calendar',
//   jd: 2460115,
//   leap_year: true,
//   leap_month: false
// }

// SolarDate {
//   day: 27,
//   month: 6,
//   year: 2023,
//   name: 'solar_calendar',
//   jd: 2460123,
//   leap_year: false
// }
```

If using `CommonJs`

```javascript showLineNumbers
const _calendar = require("lunar-date-vn/dist/index.cjs");

var solar_date = new _calendar.SolarDate(new Date());
var lunar_date = solar_date.toLunarDate();

console.log(lunar_date.getMonthName()); // Mậu Ngọ
```

If using `UMD`

```html showLineNumbers
<script src="https://cdn.jsdelivr.net/..."></script>
<script>
  var lunar_date = new window._calendar.LunarDate({
    day: 1,
    month: 1,
    year: 2020,
  });
  lunar_date.init();
  console.log(lunar_date);
</script>

<!-- SolarDate {
  day: 1,
  month: 1,
  year: 2020,
  name: 'lunar_calendar',
  jd: 2458874,
  leap_year: false,
  leap_month: false,
} -->
```

## API

### Interfaces

#### ICalendarDate

Input of **`Calendar`** (abstract class [**`LunarDate`**](#lunardate) and [**`SolarDate`**](#solardate))

```ts showLineNumbers
export interface ICalendar {
  day: number;
  month: number;
  year: number;
}
```

#### ISolarDate

Input of [**`SolarDate`**](#solardate). Inherited from [**`ICalendarDate`**](#icalendardate)

```ts showLineNumbers
interface ISolarDate extends ICalendar {}
```

#### ILunarDate

Input of [**`LunarDate`**](#lunardate). Inherited from [**`ICalendarDate`**](#icalendardate)

```ts showLineNumbers
interface ILunarDate extends ICalendarDate {
  jd?: number;
  leap_month?: boolean;
  leap_year?: boolean;
}
```

#### ILuckyHour

LuckyHour (giờ hoàng đạo)

```ts showLineNumbers
interface ILuckyHour {
  name: string;
  time: number[];
}
```

### SolarDate

#### Solar constructor 1

Creating instance [**`SolarDate`**](#solardate) from [**`ISolarDate`**](#isolardate).

> **Note** If you enter an incorrect date, it will return an error `Invalid date`. For details on valid dates, see [here](https://github.com/Hieu-BuiMinh/lunar-date-vn/wiki/Valid-date)

```ts showLineNumbers
public constructor(date: ISolarDate);
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

new SolarDate({ day: 1, month: 1, year: 2023 });
```

#### Solar constructor 2

Creating instance [**`SolarDate`**](#solardate) from `Date` object.

> **Note** If the date is entered incorrectly, the `Date` object will automatically correct it. If the date entered is between **05-14/10/1582**, it will return an error `Invalid date`. Details about valid dates see [here](https://github.com/Hieu-BuiMinh/lunar-date-vn/wiki/Valid-date)

```ts showLineNumbers
public constructor(date: Date);
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

new SolarDate(new Date());
```

#### SolarDate.FIRST_DAY

The Julian date corresponds to the first day in the calculation range. `1200-1-31`
(Ngày Julian tương ứng ngày đầu tiên trong giới hạn tính toán `1200-1-31`)

```ts showLineNumbers
public static readonly FIRST_DAY: number = SolarDate.jdn(new Date(1200, 0, 31)); //1200-1-31
```

#### SolarDate.LAST_DAY

The Julian date corresponds to the last day within the calculation range `2199-12-31`
(Ngày Julian tương ứng ngày cuối cùng trong giới hạn tính toán `2199-12-31)`

```ts showLineNumbers
public static readonly LAST_DAY: number = SolarDate.jdn(new Date(2199, 11, 31)); //2199-12-31
```

#### SolarDate.fromJd()

Return an instance [**`SolarDate`**](#solardate) from Julian date.

```ts showLineNumbers
static fromJd(jd: number): SolarDate
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

console.log(SolarDate.fromJd(2460035));

// SolarDate { day: 31, month: 3, year: 2023, jd: 2460035, leap: false }
```

#### SolarDate.jdn()

Return Julian date corresponding to [**`ICalendarDate`**](#icalendardate) or `Date`
Ref: https://ssd.jpl.nasa.gov/tools/jdc/#/jd

```ts showLineNumbers
static jdn(date: ICalendarDate | Date): number
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

console.log(SolarDate.jdn(new Date())); // 2460115
console.log(SolarDate.jdn({ day: 19, month: 6, year: 2023 })); // 2460115
```

#### solar.toDate()

Convert the entity [**`SolarDate`**](#solardate) to **`Date`**

```ts showLineNumbers
toDate(): Date
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const solar = new SolarDate(new Date());
console.log(solar.toDate());

// 2023-06-18T17:00:00.000Z
```

#### solar.toLunarDate()

Convert the entity [**`SolarDate`**](#solardate) to [**`LunarDate`**](#lunardate)

```ts showLineNumbers
toLunarDate(): LunarDate
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

var solar = new SolarDate(new Date());
var lunar = solar.toLunarDate();

console.log(lunar);

// LunarDate {
//   day: 2,
//   month: 5,
//   year: 2023,
//   name: 'lunar_calendar',
//   jd: 2460115,
//   leap_year: true,
//   leap_month: false
// }
```

#### solar.setDate()

Change the entity's time [**`SolarDate`**](#solardate)

```ts showLineNumbers
setDate(date: ICalendarDate | Date): void
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

var solar = new SolarDate(new Date()); // 2023-06-19

solar.setDate(new Date(2023, 1, 1));
console.log(solar);

// SolarDate {
//     day: 1,
//     month: 2,
//     year: 2023,
//     name: 'solar_calendar',
//     jd: 2459977,
//     leap_year: false
// }

solar.setDate({ day: 5, month: 5, year: 2015 });
console.log(solar);

// SolarDate {
//     day: 5,
//     month: 5,
//     year: 2015,
//     name: 'solar_calendar',
//     jd: 2457148,
//     leap_year: false
// }
```

#### solar.get()

Get the entity's time [**`SolarDate`**](#solardate)

```ts showLineNumbers
get();
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const dl = new SolarDate(new Date());
console.log(dl.get());

// {
//   name: 'solar_calendar',
//   day: 19,
//   month: 6,
//   year: 2023,
//   leap_year: false,
//   julian: 2460115
// }
```

### LunarDate

#### Lunar constructor

Create the entity [**`LunarDate`**](#lunardate) from [**`ILunarDate`**](#ilunardate)

> **Note** To enter a leap month, use the additional attr `leap_month = true`. If you use `leap_month = true` for a non-leap month, it will automatically revert to `leap_month = false`.

> **Note** If the date is entered incorrectly, an error will be returned. [**`Invalid date`**](https://github.com/Hieu-BuiMinh/lunar-date-vn/wiki/Error-message)

> **Note** When initializing, you need to fill in `day`, `month`, `year`. If you do not fill in other information (`leap_year`, ...) then the default is `undefined`. After initializing, you can use the function [**`lunar.init()`**](#lunarinit) to automatically fill in the missing information. If the information (`leap_year`, `jd`, ...) is `undefined` then other functions in the entity cannot be used.

```ts showLineNumbers
constructor(date: ILunarDate)
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const al = new LunarDate({ day: 1, month: 1, year: 2023 });
console.log(al);

// LunarDate {
//   day: 1,
//   month: 1,
//   year: 2023,
//   name: 'lunar_calendar',
//   jd: undefined,
//   leap_year: undefined,
//   leap_month: undefined
// }

const al = new LunarDate({ day: 1, month: 2, year: 2023, leap_month: true });
al.init();
console.log(al.toSolarDate());

// SolarDate {
//   day: 22,
//   month: 3,
//   year: 2023,
//   name: 'solar_calendar',
//   jd: 2460026,
//   leap_year: false
// }
```

#### SolarDate.fromSolarDate()

Convert the entity [**`SolarDate`**](#solardate) to [**`LunarDate`**](#lunardate).

```ts showLineNumbers
static fromSolarDate(date: SolarDate): LunarDate
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const dl = new SolarDate(new Date());
console.log(LunarDate.fromSolarDate(dl));

// LunarDate {
//   day: 2,
//   month: 5,
//   year: 2023,
//   name: 'lunar_calendar',
//   jd: 2460115,
//   leap_year: true,
//   leap_month: false
// }
```

#### lunar.init()

Initialize values for the entity. If `force_change = false`, only apply changes to the entity's secondary values (`leap-year`, `jd`, ...) when they are different from `undefined`. If `force_change = true`, always change the secondary values.

(Khởi tạo giá trị cho thực thể. Nếu `force_change = false`, chỉ áp dụng thay đổi giá trị phụ (`leap-year`, `jd`, ...) của thực thể khi chúng khác `undefined`. Nếu `force_change = true`, luôn thay đổi giá trị phụ.)

```ts showLineNumbers
init(force_change: boolean = false)
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

let lunar = new LunarDate({ day: 2, month: 5, year: 2023 });
lunar.init();
console.log(lunar);

// LunarDate {
//   day: 2,
//   month: 5,
//   year: 2023,
//   name: 'lunar_calendar',
//   jd: 2460115,
//   leap_year: true,
//   leap_month: false
// }
```

#### lunar.get()

Get the entity information [**`LunarDate`**](#lunardate).

```ts showLineNumbers
get();
```

**Example:**

```ts showLineNumbers
const dl = new SolarDate(new Date());
const al = LunarDate.fromSolarDate(dl);
console.log(al.get());

// {
//   name: 'lunar_calendar',
//   day: 2,
//   month: 5,
//   year: 2023,
//   leap_year: true,
//   julian: 2460115,
//   year_name: 'Quý Mão',
//   leap_month: false
// }
```

#### lunar.getYearName()

Get the name of the year according to the sexagenary cycle.

(Lấy tên của năm theo can chi.)

```ts showLineNumbers
getYearName(): string
```

#### lunar.getMonthName()

Get the name of the month according to the sexagenary cycle.

(Lấy tên của tháng theo can chi.)

```ts showLineNumbers
getMonthName(): string
```

#### lunar.getDayName()

Get the name of the day according to the sexagenary cycle.

(Lấy tên của ngày theo can chi.)

```ts showLineNumbers
getDayName(): string
```

#### lunar.getFirstHourNameOfTheDay()

Get the first hour's name of the day according to the sexagenary cycle.

(Lấy tên của giờ Tý đầu tiên trong ngày theo can chi.)

```ts showLineNumbers
getFirstHourNameOfTheDay(): string
```

#### lunar.getRealHourName()

Get the current hour's name of the day according to the sexagenary cycle.

(Lấy tên của giờ hiện tại trong ngày theo can chi.)

```ts showLineNumbers
getRealHourName(): string
```

#### lunar.getDayOfWeek()

Get the name of the day of the week.

(Lấy tên thứ trong tuần.)

```ts showLineNumbers
getDayOfWeek(): string
```

#### lunar.getSolarTerm()

Get the name of the solar term.

(Lấy tên tiết khí.)

```ts showLineNumbers
getTietKhi(): string
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

let lunar = new LunarDate({ day: 2, month: 5, year: 2023 });
lunar.init();

console.log(lunar.getYearName()); // Quý Mão
console.log(lunar.getMonthName()); // Mậu Ngọ
console.log(lunar.getDayName()); // Mậu Thân
console.log(lunar.getFirstHourNameOfTheDay()); // Nhâm Tý
console.log(lunar.getSolarTerm()); // Mang chủng
console.log(lunar.getDayOfWeek()); // Thứ hai
```

#### lunar.getLuckyHours()

Get the lucky hour.

(Lấy giờ hoàng đạo.)

```ts showLineNumbers
getLuckyHours(): Array<ILuckyHour>
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const dl = new SolarDate(new Date());
const al = LunarDate.fromSolarDate(dl);
console.log(al.getZodiacHour());

// [
//   { name: 'Tý', time: [ 23, 1 ] },
//   { name: 'Sửu', time: [ 1, 3 ] },
//   { name: 'Thìn', time: [ 7, 9 ] },
//   { name: 'Tỵ', time: [ 9, 11 ] },
//   { name: 'Mùi', time: [ 13, 15 ] },
//   { name: 'Tuất', time: [ 19, 21 ] }
// ]
```

#### lunar.toSolarDate()

Convert [**`LunarDate`**](#lunardate) to [**`SolarDate`**](#solardate).

```ts showLineNumbers
toSolarDate(): SolarDate
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const al = new LunarDate({ day: 2, month: 5, year: 2023 });
al.init();

console.log(al.toSolarDate());

// SolarDate {
//   day: 19,
//   month: 6,
//   year: 2023,
//   name: 'solar_calendar',
//   jd: 2460115,
//   leap_year: false
// }
```

#### lunar.setDate()

Change the time of the entity [**`LunarDate`**](#lunardate)

> **Note** This function does not standardize input data.

```ts showLineNumbers
setDate(date: ILunarDate): void
```

**Example:**

```ts showLineNumbers
import { SolarDate, LunarDate } from "lunar-date-vn";

const al = new LunarDate({ day: 2, month: 5, year: 2023 });
al.init();
al.setDate({ day: 2, month: 10, year: 2023 });

console.log(al.toSolarDate());

// SolarDate {
//   day: 14,
//   month: 11,
//   year: 2023,
//   name: 'solar_calendar',
//   jd: 2460263,
//   leap_year: false
// }
```
