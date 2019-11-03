# Angular Time Scheduler
[![GitHub issues](https://img.shields.io/github/issues/abhishekjain12/ngx-time-scheduler.svg)](https://github.com/abhishekjain12/ngx-time-scheduler/issues)
[![GitHub forks](https://img.shields.io/github/forks/abhishekjain12/ngx-time-scheduler.svg)](https://github.com/abhishekjain12/ngx-time-scheduler/network)
[![GitHub stars](https://img.shields.io/github/stars/abhishekjain12/ngx-time-scheduler.svg)](https://github.com/abhishekjain12/ngx-time-scheduler/stargazers)
[![GitHub license](https://img.shields.io/github/license/abhishekjain12/ngx-time-scheduler.svg)](https://github.com/abhishekjain12/ngx-time-scheduler/blob/master/LICENSE)
[![latest](https://img.shields.io/npm/v/ngx-time-scheduler/latest.svg)](http://www.npmjs.com/package/ngx-time-scheduler) 
[![npm](https://img.shields.io/npm/dt/ngx-time-scheduler.svg)](https://www.npmjs.com/packagengx-time-scheduler)

A simple Angular Timeline Scheduler library


# Installation
Install via [NPM](https://npmjs.com)
```
npm i ngx-time-scheduler
```


# Getting Started
Import the `NgxTimeSchedulerModule` in your app module.
```typescript
import {NgxTimeSchedulerModule} from 'ngx-time-scheduler';

@NgModule({
  imports: [
    BrowserModule,
    NgxTimeSchedulerModule,
    ...
  ],
  ...
})
export class AppModule { }
```

Use `ngx-ts` in your `app-component.html` template.
```html
<ngx-ts
  [items]="items"
  [periods]="periods"
  [sections]="sections"
  [events]="events"
  [showBusinessDayOnly]="false"
></ngx-ts>
```

And in your `app.component.ts` component class:
```typescript
import {Component, OnInit} from '@angular/core';
import {Item, Period, Section, Events} from 'ngx-time-scheduler';
import * as moment from 'moment';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html'
})
export class AppComponent implements OnInit {
  events: Events = new Events();
  periods: Period[];
  sections: Section[];
  items: Item[];

  ngOnInit() {
    this.events.SectionClickEvent = (section) => { console.log(section); };
    this.events.ItemClicked = (item) => { console.log(item); };

    this.periods = [
      {
        name: '3 days',
        timeFramePeriod: (60 * 3),
        timeFrameOverall: (60 * 24 * 3),
        timeFrameHeaders: [
          'Do MMM',
          'HH'
        ],
        classes: 'period-3day'
      }, {
        name: '1 week',
        timeFrameHeaders: ['MMM YYYY', 'DD(ddd)'],
        classes: '',
        timeFrameOverall: 1440 * 7,
        timeFramePeriod: 1440,
      }, {
        name: '2 week',
        timeFrameHeaders: ['MMM YYYY', 'DD(ddd)'],
        classes: '',
        timeFrameOverall: 1440 * 14,
        timeFramePeriod: 1440,
      }];

    this.sections = [{
      name: 'A',
      id: 1
    }, {
      name: 'B',
      id: 2
    }, {
      name: 'C',
      id: 3
    }, {
      name: 'D',
      id: 4
    }, {
      name: 'E',
      id: 5
    }];

    this.items = [{
      id: 1,
      sectionID: 1,
      name: 'Item 1',
      start: moment().startOf('day'),
      end: moment().add(5, 'days'),
      classes: ''
    }, {
      id: 2,
      sectionID: 3,
      name: 'Item 2',
      start: moment().startOf('day'),
      end: moment().add(3, 'days'),
      classes: ''
    }, {
      id: 3,
      sectionID: 1,
      name: 'Item 3',
      start: moment().add(1, 'days').startOf('day'),
      end: moment().add(3, 'days'),
      classes: ''
    }];

  }

}
```

# Inputs
| Name                  | Required  | Type      | Default                   | Description   |
| ---                   | :---:     | ---       | ---                       | ---           |
| periods               | Yes       | Period[]  | `null`                    | An array of `Period` denoting what periods to display and used to traverse the calendar. |
| sections              | Yes       | Section[] | `null`                    | An array of `Section` to fill up the sections of the scheduler. |
| items                 | Yes       | Item[]    | `null`                    | An array of `Item` to fill up the items of the scheduler. |
| events                | No        | Events    | `null`                    | The events that can be hooked into. |
| currentTimeFormat     | No        | string    | `'DD-MMM-YYYY HH:mm'`     | The momentjs format to use for concise areas, such as tooltips. |
| showCurrentTime       | No        | boolean   | `true`                    | Whether the current time should be marked on the scheduler. |
| showGoto              | No        | boolean   | `true`                    | Whether the Goto button should be displayed. |
| showToday             | No        | boolean   | `true`                    | Whether the Today button should be displayed. |
| showBusinessDayOnly   | No        | boolean   | `false`                   | Whether business days only displayed (Sat-Sun). |
| headerFormat          | No        | string    | `'Do MMM YYYY'`           | The momentjs format to use for the date range displayed as a header. |
| minRowHeight          | No        | number    | `40`                      | The minimum height, in pixels, that a section should be. |
| maxHeight             | No        | number    | `null`                    | The maximum height of the scheduler. |
| text                  | No        | Text      | `new Text()`              | An object containing the text used in the scheduler, to be easily customized. |
| start                 | No        | moment    | `moment().startOf('day')` | The start time of the scheduler as a moment object. It's recommend to use `.startOf('day')`  on the moment for a clear starting point. |


# Model

#### Period
Object with properties which create periods that can be used to traverse the calendar.

| Name              | Type      | Default   | Description   |
| ---               | ---       | ---       | ---           |
| name              | string    | `null`    | The name is used to select the period and should be unique. |
| classes           | string    | `null`    | Any css classes you wish to add to this item.  |
| timeFramePeriod   | number    | `null`    | The number of minutes between each "Timeframe" of the period. |
| timeFrameOverall  | number    | `null`    | The total number of minutes that the period shows. |
| timeFrameHeaders  | string[]  | `null`    | An array of [momentjs formats](http://momentjs.com/docs/#/displaying/format/) which is used to display the header rows at the top of the scheduler. Rather than repeating formats, the scheduler will merge all cells which are followed by a cell which shows the same date. For example, instead of seeing "Tuesday, Tuesday, Tuesday" with "3pm, 6pm, 9pm" below it, you'll instead see "Tuesday" a single time. |

#### Section
Sections used to fill the scheduler.

| Name  | Type   | Default| Description |
| ---   | ---    | ---    | ---         |
| id    | number | `null` |  A unique identifier for the section. |
| name  | string | `null` | The name to display for the section. |

#### Item
Items used to fill the scheduler.

| Name      | Type   | Default| Description |
| ---       | ---    | ---    | ---         |
| id        | number | `null` | An identifier for the item (doesn't have to be unique, but may help you identify which item was interacted with). | 
| name      | string | `null` | The name to display for the item. |
| start     | any    | `null` | A Moment object denoting where this object starts. |
| end       | any    | `null` | A Moment object denoting where this object ends. |
| classes   | string | `null` | Any css classes you wish to add to this item. |
| sectionID | number | `null` | The ID of the section that this item belongs to. |

#### Text
An object containing the text used in the scheduler, to be easily customized.

| Name          | Type   | Default      |
| ---           | ---    | ---          |
| NextButton    | string | `'Next'`     |
| PrevButton    | string | `'Prev'`     |
| TodayButton   | string | `'Today'`    |
| GotoButton    | string | `'Go to'`    |
| SectionTitle  | string | `'Section'`  |

#### Events
A selection of events are provided to hook into when creating the scheduler, and are triggered with most interactions with items.

| Name              | Parameters        | Return type   | Description |
| ---               | ---               | ---           | ---         |
| ItemClicked       | item: Item        | void          | Triggered when an item is clicked. |
| SectionClickEvent | section: Section  | void          | Triggered when a section is clicked. |


# Demo
[Demo](https://abhishekjain12.github.io/ngx-time-scheduler/)


# Credits
This time scheduler is based on the work done by [Zallist](https://github.com/Zallist/TimeScheduler).


# License
[MIT license](http://en.m.wikipedia.org/wiki/MIT_License)