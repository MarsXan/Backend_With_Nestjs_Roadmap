# Date and Time Utilities in TypeScript

This document provides an extensive guide to working with dates and times in TypeScript. From getting the current date and time in different formats to performing calculations, this guide covers it all.

## Table of Contents
1. [Getting the Current Date and Time](#getting-the-current-date-and-time)
2. [Date Formatting](#date-formatting)
3. [Date Calculations](#date-calculations)
4. [Time Calculations](#time-calculations)
5. [Comparing Dates and Times](#comparing-dates-and-times)
6. [Is in Range](#is-in-range)

---

### Getting the Current Date and Time

#### Get the Current Date in `YYYY-MM-DD` format
```typescript
const today = new Date().toISOString().split('T')[0];
console.log(today); // Output: 'YYYY-MM-DD'
```

#### Get the Full Current Date and Time
```typescript
const now = new Date();
console.log(now); // Output: full date and time object
```
#### Get Unix Timestamp (in seconds)
```typescript
const seconds = Math.floor(new Date().getTime() / 1000);
console.log(seconds); // Output: Unix timestamp in seconds
```

## Date Formatting

#### Convert to ISO Format `YYYY-MM-DDTHH:MM:SSZ`
```typescript
const isoFormat = new Date().toISOString();
console.log(isoFormat); // Output: ISO format
```
#### Custom Date Formatting
For custom date formatting, we need to manually format the date:

```typescript
const date = new Date();
const formattedDate = `${date.getFullYear()}-${(date.getMonth() + 1).toString().padStart(2, '0')}-${date.getDate().toString().padStart(2, '0')}`;
console.log(formattedDate); // Output: 'YYYY-MM-DD'
```
#### Get Local Date and Time String
```typescript
const localDateTime = new Date().toLocaleString();
console.log(localDateTime); // Output: locale-based date and time
```

## Date Calculations

#### Adding Days to a Date
```typescript
const date = new Date();
date.setDate(date.getDate() + 5); // Adds 5 days
console.log(date); // Output: date 5 days from today
```

#### Subtracting Days from a Date
```typescript
const date = new Date();
date.setDate(date.getDate() - 3); // Subtracts 3 days
console.log(date); // Output: date 3 days before today
```

#### Getting the Difference in Days Between Two Dates
```typescript
const date1 = new Date('2023-01-01');
const date2 = new Date('2023-01-10');
const diffTime = Math.abs(date2.getTime() - date1.getTime());
const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
console.log(diffDays); // Output: 9 days
```

#### Checking if a Date is Before or After Another Date
```typescript
const date1 = new Date('2023-01-01');
const date2 = new Date('2023-01-10');
console.log(date1 < date2); // Output: true (date1 is before date2)
```

## Time Calculations
#### Adding Hours to a Date
```typescript
const date = new Date();
date.setHours(date.getHours() + 4); // Adds 4 hours
console.log(date); // Output: date and time 4 hours from now
```

#### Adding Minutes to a Date
```typescript
const date = new Date();
date.setMinutes(date.getMinutes() + 30); // Adds 30 minutes
console.log(date); // Output: date and time 30 minutes from now
```

#### Getting the Difference in Hours Between Two Times
```typescript
const date1 = new Date('2023-01-01T08:00:00');
const date2 = new Date('2023-01-01T12:00:00');
const diffHours = Math.abs(date2.getTime() - date1.getTime()) / (1000 * 60 * 60);
console.log(diffHours); // Output: 4 hours
```

#### Getting the Difference in Minutes Between Two Times
```typescript
const date1 = new Date('2023-01-01T08:00:00');
const date2 = new Date('2023-01-01T08:45:00');
const diffMinutes = Math.abs(date2.getTime() - date1.getTime()) / (1000 * 60);
console.log(diffMinutes); // Output: 45 minutes
```

## Comparing Dates and Times

#### Check if Two Dates Are the Same Day
```typescript
const date1 = new Date('2023-01-01');
const date2 = new Date('2023-01-01');
const isSameDay = date1.toISOString().split('T')[0] === date2.toISOString().split('T')[0];
console.log(isSameDay); // Output: true
```
#### Check if Time is AM or PM
```typescript
const hours = new Date().getHours();
const amOrPm = hours >= 12 ? 'PM' : 'AM';
console.log(amOrPm); // Output: 'AM' or 'PM'
```

## is In Range
#### isTimeinRange
snippet to check if a specific time is within a given time range:
```typescript
function isTimeInRange(startTime: string, endTime: string, checkTime: string): boolean {
    const [startHour, startMinute] = startTime.split(':').map(Number);
    const [endHour, endMinute] = endTime.split(':').map(Number);
    const [checkHour, checkMinute] = checkTime.split(':').map(Number);

    const start = new Date();
    const end = new Date();
    const check = new Date();

    start.setHours(startHour, startMinute, 0);
    end.setHours(endHour, endMinute, 0);
    check.setHours(checkHour, checkMinute, 0);

    if (end < start) {
        // If the range wraps over midnight
        return check >= start || check <= end;
    } else {
        return check >= start && check <= end;
    }
}

// Usage example:
console.log(isTimeInRange('22:00', '06:00', '23:30')); // Output: true
console.log(isTimeInRange('22:00', '06:00', '07:00')); // Output: false
```

#### isDateInRange

```typescript
function isDateInRange(startDate: string, endDate: string, checkDate: string): boolean {
    const start = new Date(startDate);
    const end = new Date(endDate);
    const check = new Date(checkDate);

    return check >= start && check <= end;
}

// Usage example:
console.log(isDateInRange('2023-01-01', '2023-12-31', '2023-06-15')); // Output: true
console.log(isDateInRange('2023-01-01', '2023-12-31', '2024-01-01')); // Output: false
```
